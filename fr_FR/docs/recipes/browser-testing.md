___
**Note du traducteur**

C'est la traduction du fichier [browser-testing.md](https://github.com/sindresorhus/ava/blob/master/docs/recipes/browser-testing.md). Voici un [lien](https://github.com/sindresorhus/ava/compare/8e2f3dca177a4283ad882596d3c1425cabb998ef...master#diff-9d3d394077fa7f97cbbb0fefc098ac60) vers les différences avec le master de AVA (Si en cliquant sur le lien, vous ne trouvez pas le fichier `browser-testing.md` parmi les fichiers modifiés, vous pouvez donc en déduire que la traduction est à jour).
___
# Configuration de AVA pour des tests de navigateur

 Traductions : [English](https://github.com/sindresorhus/ava/blob/master/docs/recipes/browser-testing.md), [Русский](https://github.com/sindresorhus/ava-docs/blob/master/ru_RU/docs/recipes/browser-testing.md), [Italiano](https://github.com/sindresorhus/ava-docs/blob/master/it_IT/recipes/browser-testing.md)

AVA ne supporte pas [encore](https://github.com/sindresorhus/ava/issues/24) l'exécution de tests dans les navigateurs. Certaines bibliothèques exigent des globales, spécifiques aux navigateurs (`window`, `document`, `navigator`, etc).
Par exemple, c'est le cas de React, si vous voulez utiliser ReactDOM.render et simuler les événements avec ReactTestUtils.

Cette recette fonctionne pour toutes les bibliothèques qui ont besoin d'un environnement de navigateur maquetté.

## Installez jsdom

Installez [jsdom](https://github.com/tmpvar/jsdom).

> Une implémentation JavaScript des standards du DOM et du HTML WHATWG, pour une utilisation avec node.js

```
$ npm install --save-dev jsdom
```

## Configuration de jsdom

Créez un fichier helper et placez le dans le répertoire `test/helpers`. Cela permet de garantir que AVA ne le traitera pas comme un fichier de test.

`test/helpers/setup-browser-env.js`:

```js
global.document = require('jsdom').jsdom('<body></body>');
global.window = document.defaultView;
global.navigator = window.navigator;
```

## Configurez les tests pour qu'ils utilisent jsdom

Configurez le `require` de AVA avec le helper pour l'exiger avant chaque fichier de test.

`package.json`:

```json
{
  "ava": {
    "require": [
      "./test/helpers/setup-browser-env.js"
    ]
  }
}
```

## Amusez-vous !

Écrivez vos tests et profitez d'un objet window maquetté.

`test/my.react.test.js`:

```js
import test from 'ava';
import React from 'react';
import {render} from 'react-dom';
import {Simulate} from 'react-addons-test-utils';
import sinon from 'sinon';
import CustomInput from './components/custom-input.jsx';

test('Input appelle onBlur', t => {
	const onUserBlur = sinon.spy();
	const input = render(
		React.createElement(CustomInput, {onUserBlur),
		div
	)

	Simulate.blur(input);

	t.true(onUserBlur.calledOnce);
});
```
