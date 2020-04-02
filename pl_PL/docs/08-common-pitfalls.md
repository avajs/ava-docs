# Typowe pułapki

Tłumaczenia: [Français](https://github.com/avajs/ava-docs/blob/master/fr_FR/docs/08-common-pitfalls.md)

Jeśli używasz [ESLint](http://eslint.org/), możesz zainstalować [eslint-plugin-ava](https://github.com/avajs/eslint-plugin-ava). Pomoże Ci to poprawnie używać AVA i uniknąć typowych pułapek.

## AVA w Docker

Jeśli uruchamiasz AVA w Docker jako część CI, musisz naprawić odpowiednie zmienne środowiskowe. W szczególności dodanie `-e CI=true` w poleceniu `docker exec`. Zobacz [#751](https://github.com/avajs/ava/issues/751).

AVA używa [is-ci](https://github.com/watson/is-ci) aby zdecydować, czy jest w środowisku CI, czy nie używa [tych zmiennych](https://github.com/watson/ci-info/blob/master/index.js).

## AVA i powiązane limity klientów

Być może korzystasz z usługi, która umożliwia ograniczoną liczbę równoczesnych połączeń. Na przykład wiele firm korzystających z bazy danych jako usługi oferuje bezpłatny plan z ograniczeniem liczby klientów, którzy mogą z niego korzystać jednocześnie. AVA może przekroczyć te limity, ponieważ uruchamia wiele procesów, ale dobrze napisane usługi powinny w takich przypadkach emitować błąd lub przepustnicę. Jeśli ten, którego używasz - nie, testy się zawieszą.

Domyślnie AVA wykorzystuje tyle procesów, ile jest [rdzeni logicznych](https://superuser.com/questions/1105654/logical-vs-physical-cpu-performance) na twojej maszynie. Jest to ograniczone do dwóch w środowisku CI.

Użyj flagi `concurrency` aby ograniczyć liczbę uruchomionych procesów. Na przykład, jeśli Twój plan usług dopuszcza 5 klientów, powinieneś uruchomić AVA z `concurrency=5` lub mniejszą.

## Operacje asynchroniczne

Być może uruchamiasz operację asynchroniczną w teście i zastanawiasz się, dlaczego się nie kończy. Jeśli operacja asynchroniczna korzysta z obietnic, powinieneś zwrócić obietnicę (promise):

```js
test('fetches foo', t => {
	return fetch().then(data => {
		t.is(data, 'foo');
	});
});
```

Jeszcze lepiej, użyj `async` / `await`:

```js
test('fetches foo', async t => {
	const data = await fetch();
	t.is(data, 'foo');
});
```

Jeśli używasz callbacków, użyj [`test.cb`](./01-writing-tests.md#callback-support):

```js
test.cb('fetches foo', t => {
	fetch((err, data) => {
		t.is(data, 'foo');
		t.end();
	});
});
```

Alternatywnie, możesz promisify funkcję callback używając czegoś takiego, jak [`pify`](https://github.com/sindresorhus/pify):

```js
test('fetches foo', async t => {
	const data = await pify(fetch)();
	t.is(data, 'foo');
});
```

## Przypisywanie niewyłapanych wyjątków do testów

AVA [nie może wyśledzić niewyłapanych wyjątków](https://github.com/avajs/ava/issues/214) z powrotem do testu, który je uruchomił. Funkcje callback mogą prowadzić do nieprzechwyconych wyjątków, które mogą być trudne do debugowania. Rozważ promisifying i wykorzystanie `async`/`await`, jak w powyższym przykładzie. Powinno to pozwolić AVA na wychwycenie wyjątku i przypisanie go do poprawnego testu.

## Dlaczego komunikaty o wzmocnionej asercji nie są wyświetlane?

Upewnij się, że pierwszy parametr przekazany do testu ma nazwę `t`. Jest to wymóg [`power-assert`](https://github.com/power-assert-js/power-assert), biblioteka udostępniająca [wzmocnione wiadomości](./03-assertions.md#enhanced-assertion-messages).

```js
test('one is one', t => {
	t.assert(1 === 1);
});
```

Upewnij się również, aby włączyć [Babel](./recipes/babel.md).

## Udostępnianie zmiennych między testami asynchronicznymi

Domyślnie AVA wykonuje testy jednocześnie. Może to powodować problemy, jeśli testy są asynchroniczne i współużytkują zmienne.

Weź ten wymyślony przykład:

```js
const test = require('ava');

let count = 0;
const incr = async () => {
	await true;
	count = count + 1;
};

test.beforeEach('reset the count', () => {
	count = 0;
});

test('increment once', async t => {
	await incr();
	t.is(count, 1);
});

test('increment twice', async t => {
	await incr();
	await incr();
	t.is(count, 2);
});
```

Współbieżne testy pozwalają na szybsze wykonywanie testów asynchronicznych, ale jeśli polegają one na stanie współużytkowanym, może to prowadzić do nieoczekiwanych niepowodzeń testów. Jeśli nie można uniknąć stanu wspólnego, możesz wykonać testy szeregowo:

```js
const test = require('ava');

let count = 0;
const incr = async () => {
	await true;
	count = count + 1;
};

test.beforeEach('reset the count', () => {
	count = 0;
});

test.serial('increment once', async t => {
	await incr();
	t.is(count, 1);
});

test.serial('increment twice', async t => {
	await incr();
	await incr();
	t.is(count, 2);
});
```

---

Twojego problemu nie ma na liście? Prześlij pull request lub skomentuj [to issue](https://github.com/avajs/ava/issues/404).
