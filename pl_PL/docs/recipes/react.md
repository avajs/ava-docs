# Testowanie składników React

Tłumaczenia: [Español](https://github.com/avajs/ava-docs/blob/master/es_ES/docs/recipes/react.md), [Français](https://github.com/avajs/ava-docs/blob/master/fr_FR/docs/recipes/react.md)

## Konfigurowanie Babel

Gdy [włączysz Babel](https://github.com/avajs/babel), AVA automatycznie przedłuży twoją regularną konfigurację Babel (na poziomie projektu). Powinieneś móc używać React w plikach testowych bez dodatkowej konfiguracji.

Jeśli jednak chcesz to ustawić jawnie, dodaj ustawienie wstępne do opcji testowych w potoku Babel AVA, modyfikując `package.json` lub plik `ava.config.*`.

**`package.json`:**

```json
{
	"ava": {
		"babel": {
			"testOptions": {
				"presets": ["@babel/preset-react"]
			}
		}
	}
}
```

Możesz znaleźć więcej informacji w [`@ava/babel`](https://github.com/avajs/babel).

## Używanie [Enzyme](https://github.com/airbnb/enzyme)

Zobaczmy najpierw, jak używać AVA z jedną z najpopularniejszych bibliotek testowych React: [Enzyme](https://github.com/airbnb/enzyme).

Jeśli zamierzasz używać tylko [płytkiego renderowania komponentów](https://facebook.github.io/react/docs/test-utils.html#shallow-rendering), nie potrzebujesz żadnej dodatkowej konfiguracji.

Najpierw zainstaluj [wymagane pakiety Enzyme](https://github.com/airbnb/enzyme/#installation):

```console
$ npm install --save-dev enzyme react-addons-test-utils react-dom
```

Następnie możesz od razu użyć Enzyme:

```js
const test = require('ava');
const React = require('react');
const {shallow} = require('enzyme');

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

Enzyme także ma `mount` i `render` helper testować w rzeczywistym środowisku przeglądarki. Jeśli chcesz skorzystać z tych pomocników, musisz skonfigurować środowisko przeglądarki. Sprawdź [formuła na testowanie w przeglądarce](https://github.com/avajs/ava/blob/master/docs/recipes/browser-testing.md) jak to zrobić.

Aby zobaczyć przykład współpracy AVA z Enzyme skonfigurowanym do testowania w przeglądarce, spójrz na [ten przykładowy projekt](https://github.com/adriantoine/ava-enzyme-demo).

Jest to podstawowy przykład integracji enzymu z AVA. Aby uzyskać więcej informacji na temat używania Enzymu do testowania jednostkowego składnika React, zajrzyj do [dokumentacji Enzyme](http://airbnb.io/enzyme/).

## Używanie JSX helpers

Innym podejściem do testowania komponentu React jest użycie pakietu [`react-element-to-jsx-string`](https://github.com/algolia/react-element-to-jsx-string) aby porównać drzewa DOM jako ciągi znaków. [`jsx-test-helpers`](https://github.com/MoOx/jsx-test-helpers) to niezła obsługa biblioteki [renderowania płytkich komponentów](https://facebook.github.io/react/docs/test-utils.html#shallow-rendering) i konwertowania JSX na łańcuch w celu przetestowania komponentów React przy użyciu asercji AVA.

```console
$ npm install --save-dev jsx-test-helpers
```

Przykład użycia:

```js
const test = require('ava');
const React = require('react');
const {renderJSX, JSX} = require('jsx-test-helpers');

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

Zauważ, że musisz użyć zmiennych takich jak `actual` i `expected` ponieważ [`power-assert` nie obsługuje poprawnie JSX](https://github.com/power-assert-js/power-assert/issues/34).

To jest podstawowy przykład użycia `jsx-test-helpers` z AVA. Aby zobaczyć bardziej zaawansowane wykorzystanie tej biblioteki, spójrz na [ten testowy plik z adnotacjami](https://github.com/MoOx/jsx-test-helpers/blob/master/src/__tests__/index.js).

[Ten przykładowy projekt](https://github.com/MoOx/jsx-test-helpers) pokazuje podstawową i minimalną konfigurację AVA z `jsx-test-helpers`.

## Korzystanie z innych bibliotek asercji

W AVA możesz użyć dowolnej biblioteki asercji, a jest już kilka takich, które testują komponenty React. Oto lista bibliotek asercji dobrze współpracujących z AVA:

- [`expect-jsx`](https://github.com/algolia/expect-jsx) ([Przykład](https://github.com/avajs/ava/issues/186#issuecomment-161317068))
- [`unexpected-react`](https://github.com/bruderstein/unexpected-react) ([Przykładowy projekt z przykładem wyjścia](https://github.com/adriantoine/ava-unexpected-react-demo))
