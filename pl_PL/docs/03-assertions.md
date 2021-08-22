# Asercje

Tłumaczenia: [Français](https://github.com/avajs/ava-docs/blob/main/fr_FR/docs/03-assertions.md)

Asercje są mieszane w [obiekcie wykonawczym](./02-execution-context.md) dostarczonym do każdej implementacji testowej:

```js
test('unicorns are truthy', t => {
	t.truthy('unicorn'); // Assertion
});
```

Asercje są powiązane z ich testem, dzięki czemu można przypisać je do zmiennej lub przekazać:

```js
test('unicorns are truthy', t => {
	const truthy = t.truthy;
	truthy('unicorn');
});
```

Jeśli w jednym teście napotkamy wiele błędów asercji, AVA wyświetli tylko *pierwszy* z nich.

## Planowanie asercji

Plany asercji gwarantują, że testy zakończą się pomyślnie tylko wtedy, gdy wykonano określoną liczbę asercji. Pomogą ci wychwycić przypadki, w których testy kończą się zbyt wcześnie. Powodują również, że testy zakończą się niepowodzeniem, jeśli wykonanych zostanie zbyt wiele asercji, co może być przydatne, jeśli masz asercje w wywołaniach zwrotnych lub pętlach.

Jeśli nie określisz planu asercji, test nadal zakończy się niepowodzeniem, jeśli nie zostaną wykonane żadne asercje. Ustaw `failWithoutAssertions` opcje na `false` w AVA'owej [`package.json` konfiguracji](./06-configuration.md) aby wyłączyć to zachowanie.

Zauważ, że w przeciwieństwie do [`tap`](https://www.npmjs.com/package/tap) oraz [`tape`](https://www.npmjs.com/package/tape), AVA *nie* kończy automatycznie testu po osiągnięciu planowanej liczby asercji.

Poniższe przykłady zakończą się pozytywnym testem:

```js
test('resolves with 3', t => {
	t.plan(1);

	return Promise.resolve(3).then(n => {
		t.is(n, 3);
	});
});

test.cb('invokes callback', t => {
	t.plan(1);

	someAsyncFunction(() => {
		t.pass();
		t.end();
	});
});
```

Natomiast te nie:

```js
test('loops twice', t => {
	t.plan(2);

	for (let i = 0; i < 3; i++) {
		t.true(i < 3);
	}
}); // Fails, 3 assertions are executed which is too many

test('invokes callback synchronously', t => {
	t.plan(1);

	someAsyncFunction(() => {
		t.pass();
	});
}); // Fails, the test ends synchronously before the assertion is executed
```

## Pomijanie asercji

Każdą asercję można pominąć za pomocą modyfikatora `skip`. Pominięte asercje są nadal liczone, więc nie ma potrzeby zmiany planowanej liczby asercji.

```js
test('skip assertion', t => {
	t.plan(2);
	t.is.skip(foo(), 5); // No need to change your plan count when skipping
	t.is(1, 1);
});
```

## Wzmocnione komunikaty asercji

Włączając [Babel](./recipes/babel.md) włączy również [`power-assert`](https://github.com/power-assert-js/power-assert), co daje bardziej opisowe komunikaty asercji.

Weźmy ten przykład, używając standardowej bibiloteki Node'a [`assert`](https://nodejs.org/api/assert.html):

```js
const a = /foo/;
const b = 'bar';
const c = 'baz';
require('assert').ok(a.test(b) || b === c);
```

Jeśli wkleisz to do Node REPL, zwróci:

```
AssertionError: false == true
```

Z AVA'ową asercją `assert` jednakże, ten test:

```js
test('enhanced assertions', t => {
	const a = /foo/;
	const b = 'bar';
	const c = 'baz';
	t.assert(a.test(b) || b === c);
});
```

Pokaże:

```
6:   const c = 'baz';
7:   t.assert(a.test(b) || b === c);
8: });

Wartosc nie jest prawdziwa:

false

a.test(b) || b === c
=> false

b === c
=> false

c
=> 'baz'

b
=> 'bar'

a.test(b)
=> false

b
=> 'bar'

a
=> /foo/
```

## Niestandardowe asercje

Możesz użyć dowolnej biblioteki asercji zamiast lub oprócz wbudowanej, pod warunkiem, że zgłasza wyjątki w przypadku niepowodzenia asercji.

Nie zapewni to tak przyjemnego wrażenia, jak w przypadku [wbudowanych asercji](#built-in-assertions) jednak nie będziesz mógł użyć [planowania asercji](#planowanie asercji) ([zobacz #25](https://github.com/avajs/ava/issues/25)).

Będziesz musiał skonfigurować AVA, aby testy nie zakończyły się niepowodzeniem, jeśli nie zostaną wykonane żadne asercje, ponieważ AVA nie może stwierdzić, czy asercje niestandardowe przejdą. Ustaw `failWithoutAssertions` opcję na `false` w AVA'owej [`package.json` konfiguracji](./06-configuration.md).

```js
const assert = require('assert');

test('custom assertion', t => {
	assert(true);
});
```

## Wbudowane asercje

### `.pass(message?)`

Przekazywanie asercji.

### `.fail(message?)`

Nieudana asercja.

### `.assert(value, message?)`

Zapewnia, że `value` jest prawdziwe. To jest [`power-assert`](#enhanced-assertion-messages) włączone.

### `.truthy(value, message?)`

Zapewnia, że `value` jest prawdziwe.

### `.falsy(value, message?)`

Zapewnia, że `value` jest fałszywe.

### `.true(value, message?)`

Zapewnia, że `value` jest `true`.

### `.false(value, message?)`

Zapewnia, że `value` jest `false`.

### `.is(value, expected, message?)`

Zapewnia, że `value` jest takie samo jak `expected`. Jest to oparte na [`Object.is()`](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/Object/is).

### `.not(value, expected, message?)`

Zapewnia, że `value` nie jest takie samo jak `expected`. Jeset to oparte na [`Object.is()`](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/Object/is).

### `.deepEqual(value, expected, message?)`

Zapewnia, że `value` jest głęboko równe `expected`. Zobacz [Concordance](https://github.com/concordancejs/concordance) po więcej szczegółów. Działa z [elementami React oraz `react-test-renderer`](https://github.com/concordancejs/react).

### `.notDeepEqual(value, expected, message?)`

Zapewnia, że `value` nie jest głęboko równe `expected`. Odwrotność `.deepEqual()`.

### `.throws(fn, expectation?, message?)`

Zapewnia, że błąd jest rzucony. `fn` musi być funkcją, która powinna rzucić. Wyrzucona wartość *musi* być błędem. Jest zwracana, aby można było uruchomić więcej asercji przeciwko temu.

`expectation` może być obiektem o jednej lub więcej z następujących właściwości:

* `instanceOf`: konstruktor, zgłoszony błąd musi być instancją klasy
* `is`: wyrzucony błąd musi być ściśle równy `expectation.is`
* `message`: albo ciąg znaków, który jest porównywany z komunikatem o błędzie wyrzuconym, albo wyrażenie regularne, które jest dopasowane do tego komunikatu
* `name`: oczekiwana wartość `.name` wyrzuconego błędu
* `code`: oczekiwana wartość `.code` wyrzuconego błędu

`expectation` nie trzeba określać. Jeśli nie potrzebujesz, ale chcesz ustawić komunikat potwierdzający, musisz określić `null`.

Przykład:

```js
const fn = () => {
	throw new TypeError('🦄');
};

test('throws', t => {
	const error = t.throws(() => {
		fn();
	}, {instanceOf: TypeError});

	t.is(error.message, '🦄');
});
```

### `.throwsAsync(thrower, expectation?, message?)`

Zapewnia, że zgłaszany jest błąd. `thrower` może być funkcją asynchroniczną, która powinna zostać rzucona, lub obietnicą (promise), która powinna odrzucić. Asercja powinna być awaited.

Wyrzucona wartość *musi* być błędem. Jest zwracana, aby można było uruchomić więcej asercji przeciwko temu.

`expectation` może być obiektem o jednej lub więcej z następujących właściwości:

* `instanceOf`: konstruktor, zgłoszony błąd musi być instancją klasy
* `is`: wyrzucony błąd musi być ściśle równy `expectation.is`
* `message`: albo ciąg znaków, który jest porównywany z komunikatem o błędzie wyrzuconym, albo wyrażenie regularne, które jest dopasowane do tego komunikatu
* `name`: oczekiwana wartość `.name` wyrzuconego błędu
* `code`: oczekiwana wartość `.code` wyrzuconego błędu

`expectation` nie trzeba określać. Jeśli nie potrzebujesz, ale chcesz ustawić komunikat potwierdzający, musisz określić `null`.

Przykład:

```js
test('throws', async t => {
	await t.throwsAsync(async () => {
		throw new TypeError('🦄');
	}, {instanceOf: TypeError, message: '🦄'});
});
```

```js
const promise = Promise.reject(new TypeError('🦄'));

test('rejects', async t => {
	const error = await t.throwsAsync(promise);
	t.is(error.message, '🦄');
});
```

### `.notThrows(fn, message?)`

Zapewnia, że błąd nie jest zgłaszany. `fn` musi być funkcją, która nie powinna rzucać.

### `.notThrowsAsync(nonThrower, message?)`

Zapewnia, że błąd nie jest zgłaszany. `nonThrower` może być funkcją asynchroniczną, której nie należy rzucać, lub obietnicą (promise), która powinna zostać rozwiązana.

Tak jak asercja `.throwsAsync()`, musisz poczekać na zakończenie asercji:

```js
test('resolves', async t => {
	await t.notThrowsAsync(promise);
});
```

### `.regex(contents, regex, message?)`

Zapewnia, że `contents` pasuje do `regex`.

### `.notRegex(contents, regex, message?)`

Zapewnia, że `contents` nie pasuje do `regex`.

### `.snapshot(expected, message?)`
### `.snapshot(expected, options?, message?)`

Porównuje wartość `expected` z uprzednio zarejestrowanym snapshotem. Snapshoty są przechowywane dla każdego testu, więc upewnij się, że nadajesz swoim testom unikalne tytuły. Alternatywnie przekazaż obiekt `options` do wybrania określonego snapshota, na przykład `{id: 'my snapshot'}`.

Snapshot asercji nie można pominąć, gdy snapshoty są aktualizowane.

### `.try(title?, implementation | macro | macro[], ...args?)`

`.try()` pozwala ci *spróbować* asercji bez powodowania niepowodzenia testu.

Funkcja implementacji zachowuje się tak samo jak każda inna funkcja testowa. Możesz nawet używać makr. Pierwszy argument tytułu jest zawsze opcjonalny. Dodatkowe argumenty są przekazywane do funkcji implementacji lub makra.

`.try()` jest funkcją asynchroniczną. Musisz `await`. Wynikowy obiekt ma metody `commit()` oraz `discard()`. Musisz zdecydować, czy zatwierdzić, czy odrzucić wynik. Jeśli commitujesz nieudany wynik, test się nie powiedzie.

Możesz sprawdzić, czy próba się powiodła za pomocą właściwości `passed`. Wszelkie błędy asercji są dostępne za pośrednictwem właściwości `errors`. Tytuł próby jest dostępny za pośrednictwem właściwości `title`.

Logi z `t.log()` są dostępne poprzez właściwość `logs`. Możesz wybrać przechowywanie tych logów w ramach testu, przekazując `{retainLogs: true}` do metod `commit()` i `discard()`.

Funkcja implementacji otrzymuje własny [kontekst wykonania](./02-execution-context.md), podobnie jak funkcja testowa. Trzeba uważać, aby wykonywać asercje tylko w kontekście wykonania próby. Przynajmniej jedna asercja musi zostać potwierdzona dla twojej próby przejścia.

Możesz uruchomić wiele prób jednocześnie, w ramach jednego testu. Nie można jednak używać snapshotów.

Przykład:

```js
const twoRandomIntegers = () => {
	const rnd = Math.round(Math.random() * 100);
	const x = rnd % 10;
	const y = Math.floor(rnd / 10);
	return [x, y];
};

test('flaky macro', async t => {
	const firstTry = await t.try((tt, a, b) => {
		tt.is(a, b);
	}, ...randomIntegers());

	if (firstTry.passed) {
		firstTry.commit();
		return;
	}

	firstTry.discard();
	t.log(firstTry.errors);

	const secondTry = await t.try((tt, a, b) => {
		tt.is(a, b);
	}, ...randomIntegers());
	secondTry.commit();
});
```
