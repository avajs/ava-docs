___
**Note du traducteur**

C'est la traduction du fichier [vue.md](https://github.com/avajs/ava/blob/main/docs/recipes/vue.md). Voici un [lien](https://github.com/avajs/ava/compare/b208d143ad852dc95aa8b44eed94ac1f404a25f4...main#diff-da520a78ba1e2ca77e222dc26ab93529528ac720ea37001e03df3b6b5d1421d7) vers les différences avec le master de AVA (Si en cliquant sur le lien, vous ne trouvez pas le fichier `vue.md` parmi les fichiers modifiés, vous pouvez donc en déduire que la traduction est à jour).
___
# Tester les composants Vue.js

Traductions : [English](https://github.com/avajs/ava/blob/main/docs/recipes/vue.md)

## Dépendances

- [Require extension hooks](https://github.com/jackmellis/require-extension-hooks) :
	- `npm i --save-dev require-extension-hooks require-extension-hooks-vue require-extension-hooks-babel@beta`

- [jsdom-global](https://github.com/rstacruz/jsdom-global/blob/master/README.md)
	- `npm i --save-dev jsdom jsdom-global`

- Facultatif : [babel-plugin-webpack-alias-7](https://github.com/shortminds/babel-plugin-webpack-alias-7) si vous voulez utiliser les [alias de webpack](https://webpack.js.org/configuration/resolve/#resolve-alias) ou les utiliser dans vos fichiers source
	- `npm i --save-dev babel-plugin-webpack-alias-7`

## Configuration

La première étape consiste à mettre en place un helper pour configurer l'environnement afin de transpiler les fichiers `.vue` et exécuter dans un navigateur comme environnement.

**`package.json` :**

```json
{
	"ava": {
		"require": [
			"./test/_setup.js"
		]
	}
}
```

```js
// ./test/_setup.cjs

// Configuration de JSDom.
const jsdomGlobal = require('jsdom-global');
jsdomGlobal();

// Corrige l'objet Date, voir <https://github.com/vuejs/vue-test-utils/issues/936#issuecomment-415386167>.
window.Date = Date

// Configuration de l'environnement navigateur
const hooks = require('require-extension-hooks');
const Vue = require('vue');

// Configuration de Vue.js pour enlever production tip
Vue.config.productionTip = false;

// Configuration des fichiers vue pour être traités par `require-extension-hooks-vue`
hooks('vue').plugin('vue').push();
// Configuration des fichiers vue et js pour être traités par `require-extension-hooks-babel`
hooks(['vue', 'js']).exclude(({filename}) => filename.match(/\/node_modules\//)).plugin('babel').push();
```

**Remarque :** Si vous utilisez _babel-plugin-webpack-alias-7_, vous devez aussi exclure votre fichier webpack, par exemple `filename.includes(/\/node_modules\//) || filename.includes('webpack.config.test.js')`

## Exemple de test instantané

```js
const test = require('ava');
const Vue = require('vue');
const Component = require('component.vue');

test('renders', t => {
	const vm = new Vue(Component).$mount();
	const tree = {
		$el: vm.$el.outerHTML
	};
	t.snapshot(tree);
});
```

## Rapport de couverture de code

Suivez la [recette couverture de code](code-coverage.md), en ajoutant en plus l'extension `.vue`, pour que la config de `c8` traite les fichiers `.vue`.

```json
{
	"c8": {
		"extension": [
			".js",
			".vue"
		]
	}
}
```
