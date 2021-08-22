# TypeScript

Tłumaczenia: [Español](https://github.com/avajs/ava-docs/blob/main/es_ES/docs/recipes/typescript.md), [Français](https://github.com/avajs/ava-docs/blob/main/fr_FR/docs/recipes/typescript.md), [Italiano](https://github.com/avajs/ava-docs/blob/main/it_IT/docs/recipes/typescript.md), [Русский](https://github.com/avajs/ava-docs/blob/main/ru_RU/docs/recipes/typescript.md), [简体中文](https://github.com/avajs/ava-docs/blob/main/zh_CN/docs/recipes/typescript.md)

AVA jest dostarczany w pakiecie z plikiem definicji TypeScript. Dzięki temu programiści mogą wykorzystywać TypeScript do pisania testów.

Jednak po wyjęciu z pudełka AVA nie ładuje plików testowych TypeScript. Podstawowe wsparcie jest dostępne za pośrednictwem pakietu [`@ava/typescript`]. Możesz także użyć AVA z [`ts-node`]. Czytaj dalej, aby poznać szczegóły.

W tym przewodniku założono, że już skonfigurowałeś TypeScript dla swojego projektu. Należy pamiętać, że definicja AVA została przetestowana z wersją 3.7.5.

## Włączanie obsługi TypeScript w AVA

Obecnie obsługa TypeScript AVA jest przeznaczona do pracy w projektach, które prekompilują TypeScript. Proszę zobaczyć [`@ava/typescript`] po instrukcje konfiguracji.

Czytaj dalej do końca, aby dowiedzieć się, jak używać [`ts-node`] z AVA.

## Pisanie testów

Stwórz plik `test.ts`.

```ts
import test from 'ava';

const fn = () => 'foo';

test('fn() returns foo', t => {
	t.is(fn(), 'foo');
});
```

## Używanie [macros](../01-writing-tests.md#reusing-test-logic-through-macros)

Makra mogą otrzymywać dodatkowe argumenty. AVA może wywnioskować te, aby upewnić się, że używasz makra poprawnie:

```ts
import test, {ExecutionContext} from 'ava';

const hasLength = (t: ExecutionContext, input: string, expected: number) => {
	t.is(input.length, expected);
};

test('bar has length 3', hasLength, 'bar', 3);
```

Aby móc przypisać właściwość `title` do makra musisz wpisać funkcję:

```ts
import test, {Macro} from 'ava';

const macro: Macro<[string, number]> = (t, input, expected) => {
	t.is(eval(input), expected);
};
macro.title = (providedTitle = '', input, expected) => `${providedTitle} ${input} = ${expected}`.trim();

test(macro, '2 + 2', 4);
test(macro, '2 * 3', 6);
test('providedTitle', macro, '3 * 3', 9);
```

Będziesz potrzebować innego typu, jeśli spodziewasz się, że twoje makro będzie używane z testem oddzwonienia:

```ts
import test, {CbMacro} from 'ava';

const macro: CbMacro<[]> = t => {
	t.pass();
	setTimeout(t.end, 100);
};

test.cb(macro);
```

## Typing [`t.context`](../01-writing-tests.md#test-context)

Domyślnie typ `t.context` będzie pustym obiektem (`{}`). AVA ujawnia interfejs `TestInterface<Context>` do którego możesz zastosować własny typ `t.context`. Może to pomóc w wykrywaniu błędów w czasie kompilacji:

```ts
import anyTest, {TestInterface} from 'ava';

const test = anyTest as TestInterface<{foo: string}>;

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

Możesz także wpisać kontekst podczas tworzenia makr:

```ts
import anyTest, {Macro, TestInterface} from 'ava';

interface Context {
	foo: string
}

const test = anyTest as TestInterface<Context>;

const macro: Macro<[string], Context> = (t, expected: string) => {
	t.is(t.context.foo, expected);
};

test.beforeEach(t => {
	t.context = {foo: 'bar'};
});

test('foo is bar', macro, 'bar');
```

Zauważ, że pomimo wykonania powyższego typu, podczas wykonywania `t.context` jest pustym obiektem, chyba że jest przypisany.

## Typing `throws` assertions

`t.throws()` i `t.throwsAsync()` asercje są typowane, aby zawsze zwracać błąd. Możesz dostosować klasę błędów za pomocą ogólnych:

```ts
import test from 'ava';

class CustomError extends Error {
	parent: Error

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

Zauważ, że pomimo typowania, asercja wraca `undefined` jeśli to zawiedzie. Typowanie asercji jako zwracających `Error | undefined` nie wydawało się pragmatycznym wyborem.

## Kompilacja w locie za pomocą `ts-node`

Jeśli [`@ava/typescript`] nie robi sztuczki, możesz użyć [`ts-node`]. Upewnij się, że jest zainstalowany, a następnie skonfiguruj AVA do rozpoznawania plików TypeScript i rejestracji [`ts-node`]:

`package.json`:

```json
{
	"ava": {
		"extensions": [
			"ts"
		],
		"require": [
			"ts-node/register"
		]
	}
}
```

Warto zauważyć, że przy tej konfiguracji testy zakończą się niepowodzeniem, jeśli wystąpią błędy kompilacji TypeScript. Jeśli chcesz przetestować, ignorując te błędy, możesz użyć `ts-node/register/transpile-only` zamiast `ts-node/register`.

### Korzystanie z mapowania ścieżek modułów

`ts-node` [nie obsługuje mapowania ścieżek modułów](https://github.com/TypeStrong/ts-node/issues/138), jednak możesz użyć [`tsconfig-paths`](https://github.com/dividab/tsconfig-paths#readme).

Po zainstalowaniu dodaj `tsconfig-paths/register` wejście do sekcji `require` configa AVA:

`package.json`:

```json
{
	"ava": {
		"extensions": [
			"ts"
		],
		"require": [
			"ts-node/register",
			"tsconfig-paths/register"
		]
	}
}
```

Następnie możesz zacząć używać aliasów modułów:

`tsconfig.json`:
```json
{
	"baseUrl": ".",
	"paths": {
		"@helpers/*": ["helpers/*"]
	}
}
```

Test:

```ts
import myHelper from '@helpers/myHelper';

// Rest of the file
```

[`@ava/typescript`]: https://github.com/avajs/typescript
[`ts-node`]: https://www.npmjs.com/package/ts-node
