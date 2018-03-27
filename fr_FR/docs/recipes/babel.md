___
**Note du traducteur**

C'est la traduction du fichier [babel.md](https://github.com/avajs/ava/blob/master/docs/recipes/babel.md). Voici un [lien](https://github.com/avajs/ava/compare/84a884ed89277329724f1675a459b7f045bdec75...master#diff-dc9bcfba97caa3c85c58f839ac3f6d37) vers les différences avec le master de AVA (Si en cliquant sur le lien, vous ne trouvez pas le fichier `babel.md` parmi les fichiers modifiés, vous pouvez donc en déduire que la traduction est à jour).
___
# Configuration de Babel

Traductions : [English](https://github.com/avajs/ava/blob/master/docs/recipes/babel.md)

AVA utilise [Babel 7](https://babeljs.io) donc vous pouvez utiliser la dernière syntaxe JavaScript dans vos tests. Nous faisons cela en compilant des fichiers de test et de helper en utilisant notre preset [`@ava/stage-4`](https://github.com/avajs/babel-preset-stage-4). Nous utilisons également un [second preset](https://github.com/avajs/babel-preset-transform-test-files) pour activer [une amélioration des messages d'assertion](../../readme.md#messages-dassertions-améliorés) et détecter une utilisation incorrecte des assertions `t.throws()`.

Par défaut, notre pipeline Babel est appliqué aux fichiers de test et de helper se terminant par `.js`. Si votre projet utilise Babel, nous compilerons automatiquement ces fichiers en utilisant la configuration Babel de votre projet.

Si vous utilisez Babel pour vos fichiers sources, vous devez également [configurer la compilation du source](#compiler-les-sources).

AVA ne recherche que les fichiers de configuration Babel dans le répertoire de votre projet. C'est-à-dire, les fichiers `.babelrc` ou `.babelrc.js` à côté de votre fichier `package.json`, ou le fichier `package.json` lui-même.

## Personnaliser la manière dont AVA compile votre fichier de test

Vous pouvez remplacer la configuration par défaut de Babel utilisée par AVA pour la compilation de fichiers de test dans `package.json`. Par exemple, la configuration ci-dessous ajoute la prise en charge de la syntaxe JSX et des fonctionnalités du stage 3 :

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

Toutes les options de `.babelrc` sont autorisées dans l'objet `testOptions`.

## Faire ignorer à AVA les options de Babel de votre projet

Vous ne voulez pas que AVA utilise les options Babel de votre projet, par exemple si votre projet s'appuie sur Babel 6. Vous pouvez définir l'option `babelrc` à `false` :

```json
{
	"ava": {
		"babel": {
			"testOptions": {
				"babelrc": false
			}
		}
	}
}
```

## Désactiver le preset stage-4 de AVA

Vous pouvez désactiver le preset stage-4 de AVA :

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

## Préserver la syntaxe du module ES

Par défaut, le preset stage-4 de AVA convertira la syntaxe du module ES en CommonJS. Cela peut être désactivé :

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

Vous devrez utiliser le module [`esm`](https://github.com/standard-things/esm) pour que AVA puisse toujours charger vos fichiers de test. [Consultez notre recette pour plus de détails](./es-modules.md).

## Désactiver le pipeline Babel d'AVA

Vous pouvez désactiver complètement l'utilisation de Babel par AVA :

```json
{
	"ava": {
		"babel": false,
		"compileEnhancements": false
	}
}
```

## Utiliser les polyfills de Babel

AVA vous permet d'écrire vos tests en utilisant la nouvelle syntaxe JavaScript, même sur les versions de Node.js qui ne le supporteraient pas. Cependant, il n'ajoute ni ne modifie les built-ins de votre environnement actuel. Par exemple en utilisant AVA, cela ne fournit pas les fonctionnalités modernes telles que `Object.entries()` à un environnement Node.js 6.

En chargeant le [module `polyfill` de Babel](https://babeljs.io/docs/usage/polyfill/), vous pouvez opter pour ces fonctionnalités. Notez que cela modifiera l'environnement, ce qui peut influencer sur le comportement de votre programme.

Vous pouvez activer le module `polyfill` en l'ajoutant à l'option `require` de AVA :

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

Vous pouvez activer le module `register` en l'ajoutant à l'option `require` de AVA :

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

`@babel/register` traitera *également* vos fichiers de test et de helper. Pour la plupart des cas d'utilisation, ceci est inutile. Si vous créez un nouveau fichier qui nécessite `@babel/register` vous pouvez lui dire quels sont les chemins de fichiers à ignorer. Par exemple dans votre répertoire `test`, créez `_register.js`:

```js
// test/_register.js:
require('@babel/register')({
	// Ces patterns sont relatifs au répertoire du projet (où se trouve le fichier `package.json`):
	ignore: ['test/*']
});
```

Maintenant, au lieu d'exiger `@babel/register`, demandez à la place `test/_register` :

```json
{
	"ava": {
		"require": [
			"test/_register.js"
		]
	}
}
```

Notez que le chargement de `@babel/register` dans chaque processus de travail a un coût de performance non trivial. Si vous avez beaucoup de fichiers de test, vous pouvez envisager d'utiliser une étape de compilation pour compiler vos sources *avant* d'exécuter vos tests. Ce n'est pas idéal, car cela complique l'utilisation du mode watch de AVA, donc nous recommandons d'utiliser `@babel/register` jusqu'à ce que la pénalité de performance devienne trop grande. La mise en place d'une étape de précompilation est en dehors du périmètre de ce document, mais nous vous recommandons de consulter l'un des nombreux [systèmes de build qui prennent en charge Babel](http://babeljs.io/docs/setup/). Il y a une [issue](https://github.com/avajs/ava/issues/577) pour discuter des possibilités pour rendre cette expérience meilleure.
