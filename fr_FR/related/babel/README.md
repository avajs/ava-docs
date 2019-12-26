___
**Note du traducteur**

C'est la traduction du fichier [README.md](https://github.com/avajs/babel/blob/master/README.md). Voici un [lien](https://github.com/avajs/babel/compare/8c241bca4be0e326d3454b066d740bfa8a30147b...master#diff-04c6e90faac2675aa89e2176d2eec7d8) vers les différences avec le master de babel de AVA (Si en cliquant sur le lien, vous ne trouvez pas le fichier `README.md` parmi les fichiers modifiés, vous pouvez donc en déduire que la traduction est à jour).
___
# @ava/babel

Ajoutez la prise en charge de [Babel 7](https://babeljs.io) à [AVA](https://avajs.dev) afin que vous puissiez utiliser la dernière syntaxe JavaScript dans vos tests. Nous faisons cela en compilant des fichiers de test et de helper en utilisant notre preset [`@ava/stage-4`](https://github.com/avajs/babel-preset-stage-4). Nous utilisons également un [second preset, `@ava/transform-test-files`](https://github.com/avajs/babel-preset-transform-test-files) pour activer [une amélioration des messages d'assertion](https://github.com/avajs/ava-docs/blob/master/fr_FR/docs/03-assertions.md#messages-dassertions-améliorés) et détecter une utilisation incorrecte des assertions `t.throws()`.

Par défaut, notre pipeline Babel est appliqué aux fichiers de test et de helper se terminant par `.js`. Si votre projet utilise Babel, nous compilerons automatiquement ces fichiers en utilisant la configuration Babel de votre projet. Le preset `@ava/transform-helper-files` est appliqué en premier et le preset `@ava/stage-4` en dernier.

Si vous utilisez Babel pour vos fichiers sources, vous devez également [configurer la compilation du source](#compiler-les-sources).

## Activer le support de Babel

Ajoutez ce package à votre projet :

```console
npm install --save-dev @ava/babel
```

Ensuite, activez le support Babel dans le `package.json` ou `ava.config.js` :

**`package.json`:**

```json
{
	"ava": {
		"babel": true
	}
}
```

## Personnaliser la manière dont AVA compile votre fichier de test

Vous pouvez remplacer la configuration par défaut de Babel utilisée par AVA pour la compilation de fichiers de test dans `package.json` ou `ava.config.js`. Par exemple, la configuration ci-dessous ajoute la prise en charge de la syntaxe JSX et des fonctionnalités du stage 3.

**`package.json`:**

```json
{
	"ava": {
		"babel": {
			"testOptions": {
				"plugins": ["@babel/plugin-syntax-jsx"],
				"presets": ["@babel/preset-stage-3"]
			}
		}
	}
}
```

Toutes les [options de Babel] sont autorisées dans l'objet `testOptions`.

## Réinitialiser le cache de AVA

AVA met en cache les fichiers de test compilés et les helpers. Il recompile automatiquement ces fichiers lorsque vous les modifiez. AVA fait de son mieux pour détecter les modifications apportées à vos fichiers de configuration, plugins et presets Babel. Toutefois, si vous pensez que votre dernière configuration Babel n’est pas appliquée, vous pouvez réinitialiser le cache d’AVA :

```console
$ npx ava reset-cache
```

## Ajouter des extensions supplémentaires

Vous pouvez configurer AVA pour reconnaître les extensions de fichiers supplémentaires et compiler ces fichiers de test et d'aide en utilisant Babel.

**`package.json`:**

```json
{
	"ava": {
		"babel": {
			"extensions": [
				"js",
				"jsx"
			]
		}
	}
}
```

Consultez aussi l'[option `extensions`](https://github.com/avajs/ava-docs/blob/master/fr_FR/docs/06-configuration.md#options).

## Faire en sorte qu'AVA ignore les options Babel de votre projet

Vous ne voulez pas que AVA utilise les options Babel de votre projet, par exemple si votre projet s'appuie sur Babel 6. Définissez les options `babelrc` et `configFile` à `false`.

**`package.json`:**

```json
{
	"ava": {
		"babel": {
			"testOptions": {
				"babelrc": false,
				"configFile": false
			}
		}
	}
}
```

## Désactiver le preset stage-4 de AVA

Vous pouvez désactiver le preset stage-4 de AVA.

**`package.json`:**

```json
{
	"ava": {
		"babel": {
			"testOptions": {
				"presets": [
					["module:ava/stage-4", false]
				]
			}
		}
	}
}
```

Veuillez noter que cela *n'empêche pas* AVA de compiler vos fichiers de test en utilisant Babel.

Si vous le souhaitez, vous pouvez désactiver le preset dans la configuration de Babel de votre projet.

## Préserver la syntaxe du module ES

Par défaut, le preset stage-4 de AVA convertira la syntaxe du module ES en CommonJS. Cela peut être désactivé.

**`package.json`:**

```json
{
	"ava": {
		"babel": {
			"testOptions": {
				"presets": [
					["module:ava/stage-4", {"modules": false}]
				]
			}
		}
	}
}
```

Vous devrez utiliser le module [`esm`](https://github.com/standard-things/esm) pour que AVA puisse toujours charger vos fichiers de test. [Consultez notre recette pour plus de détails](https://github.com/avajs/ava-docs/blob/master/fr_FR/docs/recipes/es-modules.md).

## Désactiver les améliorations

Par défaut, AVA compilera les fichiers de test et de helper avec quelques améliorations supplémentaires. Désactivez-le en définissant `compileEnhancements` à `false` :

**`package.json`:**

```json
{
	"ava": {
		"babel": {
			"compileEnhancements": false
		}
	}
}
```

## Utiliser les polyfills de Babel

AVA vous permet d'écrire vos tests en utilisant la nouvelle syntaxe JavaScript, même sur les versions de Node.js qui ne le supporteraient pas. Cependant, il n'ajoute ni ne modifie les built-ins de votre environnement actuel. Par exemple en utilisant AVA, cela ne fournit pas les fonctionnalités modernes telles que `Object.entries()` à un environnement inférieur à Node.js 10.

En chargeant le [module `polyfill` de Babel](https://babeljs.io/docs/usage/polyfill/), vous pouvez opter pour ces fonctionnalités. Notez que cela modifiera l'environnement, ce qui peut influencer sur le comportement de votre programme.

Vous pouvez activer le module `polyfill` en l'ajoutant à l'option `require` de AVA.

**`package.json`:**

```json
{
	"ava": {
		"require": [
			"@babel/polyfill"
		]
	}
}
```

Vous devrez installer vous-même `@babel/polyfill`.

## Compiler les sources

AVA ne compile pas actuellement les fichiers source. Vous devrez charger le [module `register` de Babel](http://babeljs.io/docs/usage/require/), qui compilera les fichiers source si nécessaire.

Vous pouvez activer le module `register` en l'ajoutant à l'option `require` de AVA.

**`package.json`:**

```json
{
	"ava": {
		"require": [
			"@babel/register"
		]
	}
}
```

Vous devrez installer vous-même `@babel/register`.

`@babel/register` traitera *également* vos fichiers de test et de helper. Pour la plupart des cas d'utilisation, ceci est inutile. Si vous créez un nouveau fichier qui nécessite `@babel/register` vous pouvez lui dire quels sont les chemins de fichiers à ignorer. Par exemple dans votre répertoire `test`, créez `_register.js` :

```js
// test/_register.js:
require('@babel/register')({
	// Ces patterns sont relatifs au répertoire du projet (où se trouve le fichier `package.json`) :
	ignore: ['node_modules/*', 'test/*']
});
```

Maintenant, au lieu d'exiger `@babel/register`, demandez à la place `./test/_register`.

**`package.json`:**

```json
{
	"ava": {
		"require": [
			"./test/_register.js"
		]
	}
}
```

Notez que le chargement de `@babel/register` dans chaque processus de travail a un coût de performance non trivial. Si vous avez beaucoup de fichiers de test, vous pouvez envisager d'utiliser une étape de compilation pour compiler vos sources *avant* d'exécuter vos tests. Ce n'est pas idéal, car cela complique l'utilisation du mode watch de AVA, donc nous recommandons d'utiliser `@babel/register` jusqu'à ce que la pénalité de performance devienne trop grande. La mise en place d'une étape de précompilation est en dehors du périmètre de ce document, mais nous vous recommandons de consulter l'un des nombreux [systèmes de build qui prennent en charge Babel](http://babeljs.io/docs/setup/). Il y a une [issue](https://github.com/avajs/ava/issues/577) qui porte sur les façons dont nous pourrions améliorer cette expérience.

## Alias de webpack

Les [alias de webpack](https://webpack.js.org/configuration/resolve/#resolve-alias) peuvent être utilisés pour fournir un raccourci vers des chemins profondément imbriqués ou peu pratiques. Si vous utilisez déjà des alias dans vos fichiers source, assurez vous de pouvoir utiliser les mêmes alias dans vos fichiers de test.

Installez `babel-plugin-webpack-alias-7` comme dépendance de développement. Puis ajoutez le plugin à la config Babel de AVA :

`package.json`:

```json
{
	"ava": {
		"babel": {
			"testOptions": {
				"plugins": [
					[
						"babel-plugin-webpack-alias-7",
						{
							"config": "./path/to/webpack.config.test.js"
						}
					]
				]
			}
		}
	}
}
```

[options de Babel]: https://babeljs.io/docs/en/options
