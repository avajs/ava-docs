___
**Note du traducteur**

C'est la traduction du fichier [vue.md](https://github.com/avajs/ava/blob/master/docs/recipes/vue.md). Voici un [lien](https://github.com/avajs/ava/compare/6bba766241e20873406f844f34df56a1c0c894df...master#diff-09533a825d46c23878919cd6e9bb1182) vers les différences avec le master de AVA (Si en cliquant sur le lien, vous ne trouvez pas le fichier `vue.md` parmi les fichiers modifiés, vous pouvez donc en déduire que la traduction est à jour).
___
# Tester les composants Vue.js

Traductions : [English](https://github.com/avajs/ava/blob/master/docs/recipes/vue.md)

## Dépendances

- [Require extension hooks](https://github.com/jackmellis/require-extension-hooks) :
	- `npm i --save-dev require-extension-hooks require-extension-hooks-vue require-extension-hooks-babel`

- [browser-env](browser-testing.md)
	- `npm i --save-dev browser-env`

## Configuration

La première étape consiste à mettre en place un helper pour configurer l'environnement afin de transpiler les fichiers `.vue` et exécuter dans un navigateur comme environnement :

`package.json`

```json
{
	"ava": {
		"require": [
			"./test/helpers/setup.js"
		]
	}
}
```

```js
// ./test/helpers/setup.js

// Configuration de l'environnement navigateur
require('browser-env')();
const hooks = require('require-extension-hooks');
const Vue = require('vue');

// Configuration de Vue.js pour enlever production tip
Vue.config.productionTip = false;

// Configuration des fichiers vue pour être traités par `require-extension-hooks-vue`
hooks('vue').plugin('vue').push();
// Configuration des fichiers vue et js pour être traités par `require-extension-hooks-babel`
hooks(['vue', 'js']).plugin('babel').push();
```

Vous pouvez trouver plus d'informations sur la configuration de Babel avec AVA dans la [recette Babel](babel.md).

## Exemple de test instantané

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

## Rapport de couverture de code

Suivez la [recette couverture de code](code-coverage.md), en ajoutant en plus l'extension `.vue`, pour que la config de `nyc` traite les fichiers `.vue`.

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
