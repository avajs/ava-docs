___
**Nota del traduttore**

Questa è la traduzione del file [react.md](https://github.com/sindresorhus/ava/blob/master/docs/recipes/react.md). Qui c'è il [link](https://github.com/avajs/ava/compare/0069a7ef4797a88149031124e7eade090a18ad4a...master#diff-2cb79c7fb78b66228297358846395c3a) dove si confrontano le differenze tra commit di riferimento di questa traduzione e l'ultimo commit di AVA sul branch master (Se si clicca sul link, e non si vede il file `react.md` nella lista dei file modificati, questa traduzione è aggiornata).
___
# Testare componenti React

Translations: [Español](https://github.com/avajs/ava-docs/blob/master/es_ES/docs/recipes/react.md), [Français](https://github.com/avajs/ava-docs/blob/master/fr_FR/docs/recipes/react.md), [Italiano](https://github.com/avajs/ava-docs/blob/master/it_IT/docs/recipes/react.md)

## Configurare Babel

Il primo passo per configurare Babel per transpilare codice JSX nei test aggiungendo una sezione AVA al tuo `package.json`:

```json
{
	"ava": {
		"require": ["babel-register"],
		"babel": "inherit"
	},
	"babel": {
		"presets": ["react"]
	}
}
```

Puoi trovare maggiori informazioni su come configurare Babel con AVA nella ricetta [babelrc](https://github.com/avajs/ava-docs/blob/master/it_IT/docs/recipes/babelrc.md).

## Usare [Enzyme](https://github.com/airbnb/enzyme)

Vediamo come utilizzare AVA con uno delle librerie di testing React più popolari: [Enzyme](https://github.com/airbnb/enzyme).
Se intendi utilizzare solamente il [rendering superficiale dei componenti](https://facebook.github.io/react/docs/test-utils.html#shallow-rendering), allora non hai bisogno di seguire questa configurazione aggiuntiva.

Per prima cosa installa [i pacchetti Enzyme necessari](https://github.com/airbnb/enzyme/#installation):

```console
$ npm install --save-dev enzyme react-addons-test-utils react-dom
```

Puoi quindi usare Enzyme da subito:

```js
import test from 'ava';
import React from 'react';
import {shallow} from 'enzyme';

const Foo = ({children}) =>
	<div className="Foo">
		<span className="bar">bar</span>
		{children}
		<span className="bar">bar</span>
	</div>;

Foo.propTypes = {
	children: React.PropTypes.any
};

test('has a .Foo class name', t => {
	const wrapper = shallow(<Foo/>);
	t.true(wrapper.hasClass('Foo'));
});

test('renders two `.Bar`', t => {
	const wrapper = shallow(<Foo/>);
	t.is(wrapper.find('.bar').length, 2);
});

test('renders children when passed in', t => {
	const wrapper = shallow(
		<Foo>
			<div className="unique"/>
		</Foo>
	);
	t.true(wrapper.contains(<div className="unique"/>));
});
```

Enzyme ha anche degli helper `mount` e `render` per fare testing in un ambiente browser reale. Se vuoi utilizzare questi helpers avrai bisogno di configurare un ambiente di testing per AVA. Fai riferimento alla ricetta [browser testing](https://github.com/avajs/ava-docs/blob/master/it_IT/docs/recipes/browser-testing.md) su come fare.

Per vedere un esempio di AVA e Enzyme configurati in un ambiente browser di testing, guarda questo [progetto di esempio](https://github.com/adriantoine/ava-enzyme-demo).

Questo è un esempio base su come integrare Enzyme e AVA. Per maggiori informazioni su come usare Enzyme per il testing di componenti React fai riferimento alla [documentazione di Enzyme](http://airbnb.io/enzyme/).

## Usare helpers per JSX

Un altro approccio per il testing di componenti React è l'uso del pacchetto [`react-element-to-jsx-string`](https://github.com/algolia/react-element-to-jsx-string) per comparare alberi DOM tra loro come stringhe. La libreria [`jsx-test-helpers`](https://github.com/MoOx/jsx-test-helpers) è molto utile per il [rendering superficiale dei componenti](https://facebook.github.io/react/docs/test-utils.html#shallow-rendering) e per la conversione del codice JSX in stringhe di testo per usare le asserzioni AVA nel testing di componenti React.

```console
$ npm install --save-dev jsx-test-helpers
```

Esempio di utilizzo:

```js
import test from 'ava';
import React from 'react';
import {renderJSX, JSX} from 'jsx-test-helpers';

const Foo = ({children}) =>
	<div className="Foo">
		<span className="bar">bar</span>
		{children}
		<span className="bar">bar</span>
	</div>;

Foo.propTypes = {
	children: React.PropTypes.any
};

test('renders correct markup', t => {
	const actual = renderJSX(<Foo/>);
	const expected = JSX(
		<div className="Foo">
			<span className="bar">bar</span>
			<span className="bar">bar</span>
		</div>
	);
	t.is(actual, expected);
});

test('renders children when passed in', t => {
	const actual = renderJSX(
		<Foo>
			<div className="unique"/>
		</Foo>
	);
	const expected = JSX(
		<div className="Foo">
			<span className="bar">bar</span>
			<div className="unique"/>
			<span className="bar">bar</span>
		</div>
	);
	t.is(actual, expected);
});
```

Devi usare le variabili `actual` e `expected` come mostrato sopra a causa di un problema con [`power-assert` e la gestione di JSX](https://github.com/power-assert-js/power-assert/issues/34).

questo è un esempio base su come utilizzare `jsx-test-helpers` con AVA. Per esempi più avanzati con questa libreria guarda [questo file di test con note](https://github.com/MoOx/jsx-test-helpers/blob/master/src/__tests__/index.js).

[Questo semplice progetto](https://github.com/MoOx/jsx-test-helpers) mostra una configurazione minimale tra AVA e `jsx-test-helpers`.

## Utilizzare altre librerie di asserzione

In AVA puoi utilizzare ogni libreria di asserzione che vuoi e ci sono già alcune pronte per il testing di componenti React. Questa è una lista di librerie che funzionano bene con AVA:

- [`expect-jsx`](https://github.com/algolia/expect-jsx) ([Esempio](https://github.com/avajs/ava/issues/186#issuecomment-161317068))
- [`unexpected-react`](https://github.com/bruderstein/unexpected-react) ([Semplice progetto con un esempio di output](https://github.com/adriantoine/ava-unexpected-react-demo))