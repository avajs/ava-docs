___
**Nota del traduttore**

Questa è la traduzione del file [browser-testing.md](https://github.com/avajs/ava/blob/master/docs/recipes/browser-testing.md). Qui c'è il [link](https://github.com/avajs/ava/compare/c09462c3e515c41da8177a3d9ba5fb0f19759653...master#diff-0730bb7c2e8f9ea2438b52e419dd86c9) dove si confrontano le differenze tra commit di riferimento di questa traduzione e l'ultimo commit di AVA sul branch master (Se si clicca sul link, e non si vede il file `browser-testing.md` nella lista dei file modificati, questa traduzione è aggiornata).
___
# Preparare AVA per il Browser testing

Traduzioni: [Français](https://github.com/avajs/ava-docs/blob/master/fr_FR/docs/recipes/browser-testing.md), [Русский](https://github.com/avajs/ava-docs/blob/master/ru_RU/docs/recipes/browser-testing.md)

Con AVA non è possibile eseguire test all'interno di un browser [per ora](https://github.com/avajs/ava/issues/24). Alcune librerie richiedono alcune variabili globali specifiche del browser (`window`, `document`, `navigator`, etc).
React ne è un esempio, o almeno se vuoi utilizzare ReactDOM.render e simulare gli eventi con ReactTestUtils.

Questa ricetta funziona per ogni libreria che ha bisogno di un finto ambiente browser.

## Installare browser-env

> **️❗️ Nota importante**
>
>`browser-env` aggiunge proprietà dal namespace window al namespace globale in Node.js. Questo è un qualcosa esplicitamente [non raccomandato](https://github.com/tmpvar/jsdom/wiki/Don't-stuff-jsdom-globals-onto-the-Node-global) da `jsdom`. Leggi la pagina di wiki nel link e sii sicuro di comprendere le possibili problematiche. Se non hai molte dipendenze che potrebbero richiedere un ambiente browser allora [`window`](https://github.com/lukechilds/window#universal-testing-pattern) potrebbe essere una soluzione alternativa migliore.

Installare [browser-env](https://github.com/lukechilds/browser-env).

> Simula un ambiente browser con l'ausilio di jsdom.

```
$ npm install --save-dev browser-env
```

## Configurare browser-env

Creare un file helper e salvarlo nella cartella `test/helpers`. Questo assicura che AVA non lo consideri un file di test.

`test/helpers/setup-browser-env.js`:

```js
import browserEnv from 'browser-env';
browserEnv();
```

`browser-env` aggiungerà tutte le variabili del browser globali alla variabile globale in Node.js, imitando un ambiente browser completo. Questo dovrebbe essere sufficientemente compatibile con molte librerie front-end, anche se, non è in genere consigliata la creazione di molte variabili globali senza reale necessità. Se conosci esattamente quali variabili browser globali hai bisogno, puoi passare un Array con la lista.

```js
import browserEnv from 'browser-env';
browserEnv(['window', 'document', 'navigator']);
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

Ora puoi scrivere i tuoi test e usare un (finto) ambiente browser.

`test/my.dom.test.js`:

```js
import test from 'ava';

test('Insert to DOM', t => {
	const div = document.createElement('div');
	document.body.appendChild(div);

	t.is(document.querySelector('div'), div);
});
```


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
		React.createElement(CustomInput, onUserBlur),
		div
	);

	Simulate.blur(input);

	t.true(onUserBlur.calledOnce);
});
```
