# Flow

Tłumaczenie: [Français](https://github.com/avajs/ava-docs/blob/main/fr_FR/docs/recipes/flow.md)

Dopiero [1.4.1](https://github.com/avajs/ava/releases/tag/v1.4.1) do AVA dołączono plik definicji przepływu. Umożliwia to programistom wykorzystanie Flow do pisania testów.

**Potrzebujemy pomocy w opublikowaniu definicji typów poza AVA. Dołącz do nas https://github.com/avajs/flow-typed/issues/1 jeśli chcesz pomóc.**

W tym przewodniku założono, że skonfigurowałeś już Flow dla swojego projektu. Zauważ, że definicja AVA została przetestowana z wersją 0.95.1.
Zalecamy użycie AVA'owej [obsługi Babel](https://github.com/avajs/babel) do usuwania adnotacji i deklaracji typu Flow. AVA automatycznie stosuje konfigurację Babel twojego projektu, więc wszystko może działać bez zmian. Alternatywnie zainstaluj [`@babel/plugin-transform-flow-strip-types`](https://www.npmjs.com/package/@babel/plugin-transform-flow-strip-types) i dostosuj konfigurację AVA w pliku `package.json` (lub pliku `ava.config.*`) następująco.

**`package.json`:**

```json
{
	"ava": {
		"babel": {
			"testOptions": {
				"plugins": [
					"@babel/plugin-transform-flow-strip-types"
				]
			}
		}
	}
}
```

Zobacz nasz [`@ava/babel`](https://github.com/avajs/babel) po więcej szczegółów.

## Pisanie testów

Stwórz plik `test.js`.

```js
// @flow
const test = require('ava');

const getFoo = () => 'foo';

test('check getFoo', t => {
	t.is(getFoo(), 'foo');
});
```

## Typing [`t.context`](../01-writing-tests.md#test-context)

Domyślnie typ `t.context` będzie pustym obiektem (`{}`). AVA ujawnia interfejs `TestInterface<Context>` do którego możesz zastosować własny typ `t.context`. Może to pomóc w wykrywaniu błędów w czasie kompilacji:

```js
// @flow
const anyTest = require('ava');
const type {TestInterface} = require('ava');

const test: TestInterface<{foo: string}> = (anyTest: any);

test.beforeEach(t => {
	t.context = {foo: 'bar'};
});

test.beforeEach(t => {
	t.context.foo = 123; // error:  Type '123' is not assignable to type 'string'
});

test.serial.cb.failing('very long chains are properly typed', t => {
	t.context.fooo = 'a value'; // error: Property 'fooo' does not exist on type ''
});

test('an actual test', t => {
	t.deepEqual(t.context.foo.map(c => c), ['b', 'a', 'r']); // error: Property 'map' does not exist on type 'string'
});
```

Zauważ, że pomimo wykonania powyższego typu, podczas wykonywania `t.context` jest pustym obiektem, chyba że jest przypisany.

## Typing `throws` assertions

`t.throws()` oraz `t.throwsAsync()` asercje są typowane, aby zawsze zwracać błąd. Możesz dostosować klasę błędów używając generics:

```js
// @flow
const test = require('ava');

class CustomError extends Error {
	parent: Error;

	constructor(parent) {
		super(parent.message);
		this.parent = parent;
	}
}

function myFunc() {
	throw new CustomError(new TypeError('🙈'));
};

test('throws', t => {
	const err = t.throws<CustomError>(myFunc);
	t.is(err.parent.name, 'TypeError');
});

test('throwsAsync', async t => {
	const err = await t.throwsAsync<CustomError>(async () => myFunc());
	t.is(err.parent.name, 'TypeError');
});
```

Zauważ, że pomimo pisania, asercja wraca `undefined` jeśli zawiedzie. Wpisywanie asercji jako powracających `Error | undefined` nie wydawało się pragmatycznym wyborem.
