___
**Nota del traduttore**

Questa è la traduzione del file [readme.md](https://github.com/sindresorhus/ava/blob/master/readme.md). Questo è il [link](https://github.com/sindresorhus/ava/compare/8d47119458e83d3899683ad3ea3a4c1c01b7dd49...master#diff-8d47119458e83d3899683ad3ea3a4c1c01b7dd49) con le differenza tra il ramo master di AVA ed il commit di quando è stata aggiornata questo file (Se si clicca sul link, e non si vede il file `readme.md` nella lista dei file modificati, questa è traduzione aggiornata).
___
# Preparare AVA per il Browser testing

Traduzioni: [Français](https://github.com/sindresorhus/ava-docs/blob/master/fr_FR/docs/recipes/browser-testing.md), [Русский](https://github.com/sindresorhus/ava-docs/blob/master/ru_RU/docs/recipes/browser-testing.md)

Con AVA non è possibile eseguire test all'interno di un browser [ancora](https://github.com/sindresorhus/ava/issues/24). Alcune librerie richiedono alcune variabili globali specifiche del browser (`window`, `document`, `navigator`, etc).
React ne è un esempio, o almeno se vuoi utilizzare ReactDOM.render e simulare gli eventi con ReactTestUtils.

Questa ricetta funziona per ogni libreria che ha bisogno di un finto ambiente browser.

## Istallare jsdom

Istallare [jsdom](https://github.com/tmpvar/jsdom).

> Un'implementazione JavaScript degli standard WHATWG DOM  e HTML, da usare con node.js

```
$ npm install --save-dev jsdom
```

## Configurare jsdom

Creare un file helper e salvalo nella cartella `test/helper`. Questo assicura che AVA non lo consideri un file di test.

`test/helpers/setup-browser-env.js`:

```js
global.document = require('jsdom').jsdom('<body></body>');
global.window = document.defaultView;
global.navigator = window.navigator;
```

## Configura i test per l'utilizzo di jsdom

Configura AVA per richiedere con `require` il file helper per ogni file di test.

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

## Tutto fatto!

Scrivi i tuoi test e usa il finto oggetto window.

`test/my.react.test.js`:

```js
import test from 'ava';
import React from 'react';
import {render} from 'react-dom';
import {Simulate} from 'react-addons-test-utils';
import sinon from 'sinon';
import CustomInput from './components/custom-input.jsx';

test('Input calls onBlur', t => {
	const onUserBlur = sinon.spy();
	const input = render(
		React.createElement(CustomInput, {onUserBlur),
		div
	)

	Simulate.blur(input);

	t.true(onUserBlur.calledOnce);
});
```
