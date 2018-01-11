___
**Note du traducteur**

C'est la traduction du fichier [precompiling-with-webpack.md](https://github.com/avajs/ava/blob/master/docs/recipes/jspm-systemjs.md). Voici un [lien](https://github.com/avajs/ava/compare/c1faf95b9bbaa1981013e98576fc58a4d6217020...master#diff-b4c25a4a1412e9204ac923133f67814f) vers les différences avec le master de AVA (Si en cliquant sur le lien, vous ne trouvez pas le fichier `precompiling-with-webpack.md` parmi les fichiers modifiés, vous pouvez donc en déduire que la traduction est à jour).
___
## Précompilation des fichiers sources avec webpack

Traductions : [English](https://github.com/avajs/ava/blob/master/docs/recipes/precompiling-with-webpack.md)

Le [readme](https://github.com/avajs/ava#transpiling-imported-modules) de AVA mentionne la précompilation de vos modules importés comme une alternative à la compilation lors de l'exécution, mais il ne l'explique pas. Cette recette décrit plusieurs approches à l'aide de webpack v2. Les différentes approches sont décrites car chacune a ses propres avantages et inconvénients. Vous devez choisir la méthode qui convient le mieux à votre cas d'utilisation. Voir la discussion originale [ici](https://github.com/avajs/ava/pull/1385).

- [Fichier de test unique](#fichier-de-test-unique)
- [Plusieurs fichiers de test](#plusieurs-fichiers-de-test)

### Fichier de test unique

C'est le cas d'utilisation le plus simple. Vous pourriez avoir besoin de cela si vous [utilisez des alias](https://github.com/avajs/ava/issues/1011).

###### `webpack.config.js`

```js
const path = require('path');
const nodeExternals = require('webpack-node-externals');

module.exports = {
	entry: ['tests.js'],
	target: 'node',
	output: {
		path: path.resolve(__dirname, '_build'),
		filename: 'test.js'
	},
	externals: [nodeExternals()],
	module: {
		rules: [
			{
				test: /\.(js|jsx)$/,
				use: 'babel-loader',
				options: {
					cacheDirectory: true
				}
			}
		]
	}
};
```

Les points importants sont `target: 'node'`, qui ne tient pas compte des `require`s spécifiques de Node.js (par exemple `fs`, `path`, etc...) et `externals: nodeModules` qui empêche webpack d'empaqueter les modules externes de Node.js qui pourraient être gênants.

Vous pouvez maintenant exécuter `$ ava _build/test.js` pour lancer les tests contenus dans ce fichier généré.

### Plusieurs fichiers de test

Les choses sont un peu plus compliquées avec plusieurs fichiers de test. Nous recommandons [l'utilisation de babel-register](babelrc.md) tant que cela ne pénalise pas trop les performances.

Les approches possibles sont :

- [Renvoyer la source précompilée dans les tests](#renvoyer-la-source-précompilée-dans-les-tests)
- [Fichier d'entrée unique](#fichier-dentrée-unique)
- [Plusieurs fichiers en entrée](#plusieurs-fichiers-en-entrée)
- [Test sur les sources précompilées](#test-sur-les-sources-précompilées)

#### Renvoyer la source précompilée dans les tests

Les fichiers sources sont compilés dans le répertoire `_src` et sont référencés dans les tests. Bien que ce soit le moins élégant, il fonctionne bien et le flux de travail peut être optimisé avec le [mode watch de `babel-cli`](https://babeljs.io/docs/usage/cli/#babel).

```js
// Avant
import fresh from '../src';
// Après
import fresh from '../_src';
```

#### Fichier d'entrée unique

Plusieurs fichiers de test peuvent être compilés dans un seul fichier. Cela peut permettre d'avoir la meilleure performance, mais ceci a un coût. Tous les tests seront dans le même fichier, ce qui peut rendre plus difficile la recherche du test qui a échoué, puisque AVA ne peut pas afficher le nom de fichier orignal du test. Vous perdez aussi l'[isolement du processus](https://github.com/avajs/ava-docs/tree/master/fr_FR#isolement-du-processus).

###### `webpack.config.js`

[Réponse associée sur Stack Overflow](http://stackoverflow.com/questions/32874025/how-to-add-wildcard-mapping-in-entry-of-webpack/34545812#34545812)

```js
const path = require('path');
const glob = require('glob');
const nodeExternals = require('webpack-node-externals');

module.exports = {
	target: 'node',
	entry: glob.sync('./test/**/*.js'),
	output: {
		path: path.resolve(__dirname, '_build'),
		filename: 'tests.js'
	},
	externals: [nodeExternals()],
	module: {
		rules: [
			{
				test: /\.(js|jsx)$/,
				use: {
					loader: 'babel-loader',
					options: {
						cacheDirectory: true
					}
				}
			}
		]
	}
};
```

<details>
<summary>Comparaison du rapport d'erreur</summary>

```
# Avant
  aggregations-test » cardinality-agg » sets precision_threshold option
  E:\Projects\repos\elastic-builder\test\_macros.js:167

   166:         const expected = getExpected(keyName, recursiveToJSON(propValue));
   167:         t.deepEqual(value, expected);
   168:     }

  Difference:

      Object {
        my_agg: Object {
          cardinality: Object {
    -       precision_threshol: 5000,
    +       precision_threshold: 5000,
          },
        },
      }

# Après
  sets precision_threshold option
  E:\Projects\repos\elastic-builder\_build\tests.js:106

   105:                     column: 21
   106:                 }
   107:             },

  Difference:

      Object {
        my_agg: Object {
          cardinality: Object {
    -       precision_threshol: 5000,
    +       precision_threshold: 5000,
          },
        },
      }

```
</details>

#### Plusieurs fichiers en entrée

Nous pouvons demander à webpack de générer plusieurs fichiers en entrée. Cela permet de conserver les noms de fichiers pour que les rapports d'erreur soient faciles à interpréter.  Mais chaque fichier en entrée a sa propre copie du fichier source. Cela entraîne une taille de fichier considérablement plus importante.  Cela peut entrainer une [faible performance](https://github.com/avajs/ava/pull/1385#issuecomment-304684047) sur la première exécution.

###### `webpack.config.js`

```js
const path = require('path');
const glob = require('glob');
const nodeExternals = require('webpack-node-externals');

const entryObj = glob.sync('./test/**/*.js')
	.reduce((acc, file) => {
		acc[path.basename(file, path.extname(file))] = file;
		return acc;
	}, {});

module.exports = {
	target: 'node',
	entry: entryObj,
	output: {
		path: path.resolve(__dirname, '_build'),
		filename: '[name].js'
	},
	externals: [nodeExternals()],
	module: {
		rules: [
			{
				test: /\.(js|jsx)$/,
				use: {
					loader: 'babel-loader',
					options: {
						cacheDirectory: true
					}
				}
			}
		]
	}
};
```

#### Test sur les sources précompilées

C'est le plus compliqué à configurer, mais fonctionne très bien et conserve également les noms de fichiers. Dans cette approche, nous utilisons `babel-cli` pour compiler les fichiers sources, en préservant la structure des fichiers.  Les chemins requis dans les tests sont réécrits lors de leur compilation dans webpack. L'exemple suivant concerne une structure de fichier spécifique. Selon la façon dont vos fichiers sources et de test sont organisés, vous devrez peut-être apporter des modifications.

Structure des fichiers :

```
├───src
│   ├───my-pkg-fldr
│   │   ├───my-module.js
│   │   └───index.js
│   └───index.js
└───test
    ├───my-pkg-fldr
    │   └───my-module.test.js
    └───index.test.js

# Structure des fichiers générés
├───_src
│   ├───my-pkg-fldr
│   │   ├───my-module.js
│   │   └───index.js
│   └───index.js
└───_build
    ├───my-module.test.js
    └───index.test.js
```

npm scripts:

```js
{
	"scripts": {
		"precompile-src": "cross-env NODE_ENV=test babel src --out-dir _src",
		"precompile-tests": "cross-env NODE_ENV=test webpack --config webpack.config.js",
		"pretest": "npm run precompile-src && npm run precompile-tests",
		"test": "cross-env NODE_ENV=test nyc --cache ava _build"
	}
}
```

###### `webpack.config.js`

[docs de `externals` de Webpack](https://webpack.js.org/configuration/externals/#function)

```js
const path = require('path');
const glob = require('glob');
const nodeExternals = require('webpack-node-externals');

const entryObj = glob.sync('./test/**/*.js')
	.reduce((acc, file) => {
		acc[path.basename(file, path.extname(file))] = file;
		return acc;
	}, {});

module.exports = {
	target: 'node',
	entry: entryObj,
	output: {
		path: path.resolve(__dirname, '_build'),
		filename: '[name].js'
	},
	externals: [
		nodeExternals(),
		// Rewrite the require paths to use `_src`
		(context, request, callback) => {
			// This is a little messy because tests are not output in original file structure
			// test/index.test.js → _build/index.test.js
				//=> ../src → ../_src
			// test/my-pkg-fldr/my-module.test.js → _build/my-module.test.js
				//=> ../../src → ../_src
			if (request.includes('/src')) {
				const requestReqwrite = request
					.replace('/src', '/_src')
					.replace('../../_src', '../_src');
				return callback(null, `commonjs ${requestReqwrite}`);
			}

			callback();
		}
	]
};
```
