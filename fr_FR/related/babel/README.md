___
**Note du traducteur**

C'est la traduction du fichier [README.md](https://github.com/avajs/babel/blob/master/README.md). Voici un [lien](https://github.com/avajs/babel/compare/578822a18c88dd95439eb0634b009ef6026f671a...master#diff-04c6e90faac2675aa89e2176d2eec7d8) vers les différences avec le master de babel de AVA (Si en cliquant sur le lien, vous ne trouvez pas le fichier `README.md` parmi les fichiers modifiés, vous pouvez donc en déduire que la traduction est à jour).
___
# @ava/babel

Traductions : [English](https://github.com/avajs/babel/blob/master/README.md)

Ajoutez la prise en charge de [Babel 7](https://babeljs.io) à [AVA](https://avajs.dev) afin que vous puissiez utiliser la dernière syntaxe JavaScript dans vos tests. Nous faisons cela en compilant des fichiers de test et de helper en utilisant notre preset `@ava/babel/stage-4`. Nous utilisons également un second preset, `@ava/babel/transform-test-files` pour activer [une amélioration des messages d'assertion](https://github.com/avajs/ava-docs/blob/master/fr_FR/docs/03-assertions.md#messages-dassertions-améliorés) et détecter une utilisation incorrecte des assertions `t.throws()`.

Par défaut, notre pipeline Babel est appliqué aux fichiers de test et de helper se terminant par `.cjs` and `.js`. Si votre projet utilise Babel, nous compilerons automatiquement ces fichiers en utilisant la configuration Babel de votre projet. Le preset `@ava/babel/transform-helper-files` est appliqué en premier et le preset `@ava/babel/stage-4` en dernier.

Si vous utilisez Babel pour vos fichiers sources, vous devez également [configurer la compilation du source](#compiler-les-sources).

## Activer le support de Babel

Ajoutez ce package à votre projet :

```console
npm install --save-dev @ava/babel
```

Ensuite, activez le support Babel dans le `package.json` ou `ava.config.*` :

**`package.json`:**

```json
{
	"ava": {
		"babel": true
	}
}
```

## Personnaliser la manière dont AVA compile votre fichier de test

Vous pouvez remplacer la configuration par défaut de Babel utilisée par AVA pour la compilation de fichiers de test dans `package.json` ou `ava.config.*`. Par exemple, la configuration ci-dessous ajoute la prise en charge de la syntaxe JSX et des fonctionnalités du stage 3.

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

## Compiler des fichiers supplémentaires

Par défaut, seuls les fichiers de test sont compilés. Vous pouvez compiler des fichiers supplémentaires en tant que tests en fournissant des glob patterns :

**`package.sjon`:**

```json
{
	"ava": {
		"babel": {
			"compileAsTests": ["test/helpers/**/*"]
		}
	}
}
```

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
					["module:@ava/babel/stage-4", false]
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
					["module:@ava/babel/stage-4", {"modules": false}]
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

## `@ava/babel/stage-4`

A pour ambition d'apporter les [propositions ECMAScript terminées](https://github.com/tc39/proposals/blob/master/finished-proposals.md) aux fichiers de test et helpers de AVA.

D'appliquer efficacement le minimum de transformations pour exécuter la dernière syntaxe JavaScript sur Node.js 10 et 12.

Les fonctions intégrées ne sont ni ajoutées ni étendues, donc des fonctionnalités comme Proxies, `Array.prototype.includes` ou `String.prototype.padStart` ne seront disponibles que si la version Node.js exécutant les tests le prend en charge. Consultez [node.green] pour plus de détails.

Parfois, une fonctionnalité particulière est *principalement* implémentée dans Node.js. Dans ce cas, les transformations ne sont pas appliquées.

Toutes les propositions ne peuvent pas être prises en charge via les transformations Babel, voir ci-dessous pour plus de détails. Babel peut avoir besoin de plugins "syntaxiques" pour analyser certains fichiers. Ces plugins doivent être appliqués explicitement car ce preset peut ne pas les inclure. Si vous utilisez la compilation Babel d'AVA, cela est déjà pris en charge.

### Propositions prises en charges

| Propositions                                                                                   | Pris en charge
| ---------------------------------------------------------------------------------------------- | --------------
| [`Array.prototype.includes`][array-includes]                                                   | Non
| [`Object.values`/`Object.entries`][object-values-entries]                                      | Non
| [String padding][string-padding]                                                               | Non
| [`Object.getOwnPropertyDescriptors`][object-gopds]                                             | Non
| [Virgules terminales dans les listes de paramètres de fonction et les appels][function-commas] | Oui
| [Mémoire partagée et atomique][atomics]                                                        | Non
| [Levée de la restriction littérale du template][template-literal-lift]                         | Non
| [`s` (`dotAll`) indicateur pour les expressions régulières][dot-all]                           | Oui
| [RegExp groupes de capture nommés][named-groups]                                               | Non
| [RegExp assertions arrière][lookbehind]                                                        | Non
| [RegExp échappements de propriété Unicode][unicode-escapes]                                    | Non
| [`Promise.prototype.finally`][finally]                                                         | Non
| [Itération asynchrone][async-iteration]                                                        | Partiellement<sup>†</sup>
| [Liaison du `catch` en option][optional-catch]                                                 | Oui
| [JSON superset][json-superset]                                                                 | Non
| [`Symbol.prototype.description`][symbol-description]                                           | Non
| [`Function.prototype.toString` revision][function-tostring-revision]                           | Non
| [`Object.fromEntries`][object-fromentries]                                                     | Non
| [`JSON.stringify` bien formé][well-formed-stringify]                                           | Non
| [`String.prototype.{trimStart,trimEnd}`][string-left-right-trim]                               | Non
| [`Array.prototype.{flat,flatMap}`][flatmap]                                                    | Non
| [`String.prototype.matchAll`][string-matchall]                                                 | Non
| [`import()`][dynamic-import]                                                                   | Oui
| [`Promise.allSettled`][promise-allsettled]                                                     | Non
| [Chaînage optionnel][chaining]                                                                 | Oui
| [Opérateur Nullish coalescing][nullish-coalescing]                                             | Oui

† [`@babel/plugin-proposal-async-generator-functions`](https://www.npmjs.com/package/@babel/plugin-proposal-async-generator-functions) s'appuie sur `Symbol.asyncIterator`, qu'AVA ne "polyfill" pas pour vous.

[array-includes]: https://github.com/tc39/Array.prototype.includes
[async-iteration]: https://github.com/tc39/proposal-async-iteration
[atomics]: https://github.com/tc39/ecmascript_sharedmem
[Babel options]: https://babeljs.io/docs/en/options
[chaining]: https://github.com/tc39/proposal-optional-chaining
[dot-all]: https://github.com/tc39/proposal-regexp-dotall-flag
[dynamic-import]: https://github.com/tc39/proposal-dynamic-import
[finally]: https://github.com/tc39/proposal-promise-finally
[flatmap]: https://github.com/tc39/proposal-flatMap
[function-commas]: https://github.com/tc39/proposal-trailing-function-commas
[function-tostring-revision]: https://github.com/tc39/Function-prototype-toString-revision
[json-superset]: https://github.com/tc39/proposal-json-superset
[lookbehind]: https://github.com/tc39/proposal-regexp-lookbehind
[named-groups]: https://github.com/tc39/proposal-regexp-named-groups
[node.green]: http://node.green
[nullish-coalescing]: https://github.com/tc39/proposal-nullish-coalescing
[object-fromentries]: https://github.com/tc39/proposal-object-from-entries
[object-gopds]: https://github.com/ljharb/proposal-object-getownpropertydescriptors
[object-values-entries]: https://github.com/tc39/proposal-object-values-entries
[optional-catch]: https://github.com/tc39/proposal-optional-catch-binding
[promise-allsettled]: https://github.com/tc39/proposal-promise-allSettled
[string-left-right-trim]: https://github.com/tc39/proposal-string-left-right-trim
[string-matchall]: https://github.com/tc39/String.prototype.matchAll
[string-padding]: https://github.com/tc39/proposal-string-pad-start-end
[symbol-description]: https://github.com/tc39/proposal-Symbol-description
[template-literal-lift]: https://github.com/tc39/proposal-template-literal-revision
[unicode-escapes]: https://github.com/tc39/proposal-regexp-unicode-property-escapes
[well-formed-stringify]: https://github.com/tc39/proposal-well-formed-stringify
