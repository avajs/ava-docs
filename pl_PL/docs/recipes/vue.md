# Testowanie komponentów Vue.js

Tłumaczenia: [Français](https://github.com/avajs/ava-docs/blob/main/fr_FR/docs/recipes/vue.md)

## Zależności

- [Require extension hooks](https://github.com/jackmellis/require-extension-hooks):
	- `npm i --save-dev require-extension-hooks require-extension-hooks-vue require-extension-hooks-babel@beta`

- [jsdom-global](https://github.com/rstacruz/jsdom-global/blob/master/README.md)
	- `npm i --save-dev jsdom jsdom-global`

- Opcjonalne: [babel-plugin-webpack-alias-7](https://github.com/shortminds/babel-plugin-webpack-alias-7) jeśli chcesz użyć [webpack aliases](https://webpack.js.org/configuration/resolve/#resolve-alias) lub użyć ich w swoich plikach źródłowych
	- `npm i --save-dev babel-plugin-webpack-alias-7`

## Ustawienie

Pierwszym krokiem jest skonfigurowanie pomocnika do skonfigurowania środowiska do transponowania plików `.vue` i uruchomienia w środowisku przypominającym przeglądarkę.

**`package.json`:**

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
// ./test/_setup.js

// Set up JSDom.
require('jsdom-global')()

// Fix the Date object, see <https://github.com/vuejs/vue-test-utils/issues/936#issuecomment-415386167>.
window.Date = Date

// Setup browser environment
const hooks = require('require-extension-hooks');
const Vue = require('vue');

// Setup Vue.js to remove production tip
Vue.config.productionTip = false;

// Setup vue files to be processed by `require-extension-hooks-vue`
hooks('vue').plugin('vue').push();
// Setup vue and js files to be processed by `require-extension-hooks-babel`
hooks(['vue', 'js']).exclude(({filename}) => filename.match(/\/node_modules\//)).plugin('babel').push();
```

**Uwaga:** Jeśli używasz _babel-plugin-webpack-alias-7_, musisz również wykluczyć swój plik webpack - np. `filename.includes(/\/node_modules\//) || filename.includes('webpack.config.test.js')`

Więcej informacji na temat konfigurowania Babel z AVA znajdziesz w [`@ava/babel`](https://github.com/avajs/babel).

## Przykładowy test migawki

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

## Raportowanie pokrycia

Postępuj zgodnie z [przepisem raportowania zasięgu](code-coverage.md), dodatkowo dodając rozszerzenie `.vue` do `nyc` configa dla plików `.vue`.

```json
{
	"nyc": {
		"extension": [
			".js",
			".vue"
		]
	}
}
```
