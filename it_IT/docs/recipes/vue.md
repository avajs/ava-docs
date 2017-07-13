___
**Nota del traduttore**

Questa è la traduzione del file [vue.md](https://github.com/sindresorhus/ava/blob/master/docs/recipes/vue.md). Qui c'è il [link](https://github.com/avajs/ava/compare/0069a7ef4797a88149031124e7eade090a18ad4a...master#diff-09533a825d46c23878919cd6e9bb1182) dove si confrontano le differenze tra commit di riferimento di questa traduzione e l'ultimo commit di AVA sul branch master (Se si clicca sul link, e non si vede il file `vue.md` nella lista dei file modificati, questa traduzione è aggiornata).
___
# Testare componenti Vue.js

Traduzioni: [Français](https://github.com/avajs/ava-docs/blob/master/fr_FR/docs/recipes/vue.md), [Italiano](https://github.com/avajs/ava-docs/blob/master/it_IT/docs/recipes/vue.md)

## Dipendenze

- [Estensione per gli hook require](https://github.com/jackmellis/require-extension-hooks):
	- `npm i --save-dev require-extension-hooks require-extension-hooks-vue require-extension-hooks-babel`

- [browser-env](browser-testing.md)
	- `npm i --save-dev browser-env`

## Setup

Il primo passo consiste nell'impostare un helper per configurare l'ambiente per la traspilazione dei file `.vue` e la simulazione di un ambiente browser:

```json
{
	"ava": {
		"babel": "inherit",
		"require": [
			"./test/helpers/setup.js"
		]
	}
}
```

```js
// ./test/helpers/setup.js

// Setup browser environment
require('browser-env')();
const hooks = require('require-extension-hooks');
const Vue = require('vue');

// Setup Vue.js to remove production tip
Vue.config.productionTip = false;

// Setup vue files to be processed by `require-extension-hooks-vue`
hooks('vue').plugin('vue').push();
// Setup vue and js files to be processed by `require-extension-hooks-babel`
hooks(['vue', 'js']).plugin('babel').push();
```

Puoi trovare maggiori informazioni su come configurare Babel con AVA nella ricetta [babelrc](https://github.com/avajs/ava-docs/blob/master/it_IT/docs/recipes/babelrc.md).

## Snapshot test semplice

```js
import test from 'ava';
import Vue from 'vue';
import Component from 'component.vue';

test('renders', t => {
	const vm = new Vue(Component).$mount();
	const tree = {
		$el: vm.$el.outerHTML
	};
	t.snapshot(tree);
});
```

## Report di code coverage

Come primo passo segui la ricetta [code coverage]
Follow the [coverage reporting recipe](https://github.com/avajs/ava-docs/blob/master/it_IT/docs/recipes/code-coverage.md), poi aggiungi l'estensione `.vue` alla configurazione di `nyc` per instrumentare i file `.vue`.

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