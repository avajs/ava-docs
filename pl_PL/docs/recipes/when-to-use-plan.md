# Kiedy użyć `t.plan()`

Tłumaczenia: [Español](https://github.com/avajs/ava-docs/blob/main/es_ES/docs/recipes/when-to-use-plan.md), [Français](https://github.com/avajs/ava-docs/blob/main/fr_FR/docs/recipes/when-to-use-plan.md), [Italiano](https://github.com/avajs/ava-docs/blob/main/it_IT/docs/recipes/when-to-use-plan.md), [日本語](https://github.com/avajs/ava-docs/blob/main/ja_JP/docs/recipes/when-to-use-plan.md),  [Português](https://github.com/avajs/ava-docs/blob/main/pt_BR/docs/recipes/when-to-use-plan.md), [Русский](https://github.com/avajs/ava-docs/blob/main/ru_RU/docs/recipes/when-to-use-plan.md), [简体中文](https://github.com/avajs/ava-docs/blob/main/zh_CN/docs/recipes/when-to-use-plan.md)

Jedna główna różnica między AVA a [`tap`](https://github.com/tapjs/node-tap)/[`tape`](https://github.com/substack/tape) to zachowanie `t.plan()`. W AVA, `t.plan()` służy tylko do zapewnienia, że wywoływana jest oczekiwana liczba asercji; nie kończy automatycznie testu.

## Słabe zastosowania `t.plan()`

Wielu użytkowników przechodzi z `tap`/`tape` są przyzwyczajeni do korzystania `t.plan()` często w każdym teście. Jednak w AVA nie uważamy tego za "najlepszą praktykę". Zamiast tego wierzymy że `t.plan()` powinien być stosowany tylko w sytuacjach, w których zapewnia pewną wartość.

### Testy synchronizacji bez rozgałęzień

`t.plan()` jest niepotrzebny w większości testów synchronizacji.

```js
test('simple sums', t => {
	// BAD: there is no branching here - t.plan() is pointless
	t.plan(2);

	t.is(1 + 1, 2);
	t.is(2 + 2, 4);
});
```

`t.plan()` nie zapewnia tutaj żadnej wartości i stwarza dodatkowe obowiązki, jeśli kiedykolwiek zdecydujesz się dodać lub usunąć asercje.

### Obietnice (promises), które powinny zostać spełnione

```js
test('gives foo', t => {
	t.plan(1);

	return somePromise().then(result => {
		t.is(result, 'foo');
	});
});
```

Na pierwszy rzut oka wydaje się, że testy te dobrze wykorzystują `t.plan()` ponieważ zaangażowana jest obsługa asynchronicznych obietnic. Istnieje jednak kilka problemów z testem:

1. `t.plan()` jest prawdopodobnie używany tutaj w celu ochrony przed taką możliwością `somePromise()` może zostać odrzucone; Ale odesłanie odrzuconej obietnicy i tak nie powiedzie się.

2. Lepiej byłoby skorzystać z `async`/`await`

```js
test('gives foo', async t => {
	t.is(await somePromise(), 'foo');
});
```

### Promises z blokiem `.catch()`

```js
test('rejects with foo', t => {
	t.plan(2);

	return shouldRejectWithFoo().catch(reason => {
		t.is(reason.message, 'Hello');
		t.is(reason.foo, 'bar');
	});
});
```

Tutaj użycie `t.plan()` stara się zapewnić wykonanie kodu w bloku 'catch'.
Zamiast tego powinieneś skorzystać `t.throwsAsync` i `async`/`await`, ponieważ prowadzi to do bardziej płaskiego kodu, który łatwiej jest zrozumieć:

```js
test('rejects with foo', async t => {
	const reason = await t.throwsAsync(shouldRejectWithFoo());
	t.is(reason.message, 'Hello');
	t.is(reason.foo, 'bar');
});
```

### Zapewnienie, że nastąpi zdarzenie catch

```js
test('throws', t => {
	t.plan(2);

	try {
		shouldThrow();
	} catch (err) {
		t.is(err.message, 'Hello');
		t.is(err.foo, 'bar');
	}
});
```

Jak stwierdzono w poprzednim przykładzie, używając asercji `t.throws()` z `async`/`await` jest lepszym wyborem.

## Dobre wykorzystanie `t.plan()`

`t.plan()` zapewnia wartość w następujących przypadkach.

### Zapewnienie faktycznego wywoływania wielu callbacków

```js
test.cb('invokes callbacks', t => {
	t.plan(2);

	const callbackA = () => {
		t.pass();
		t.end();
	};

	const callbackB = () => t.pass();

	bThenA(callbackA, callbackB);
});
```

Powyższe zapewnia że `callbackB` wywołuje się pierwszy (i tylko raz)), śledzony przez `callbackA`. Żadna inna kombinacja nie spełnia planu.

### Testy z instrukcjami rozgałęziającymi

W większości przypadków używanie złożonych rozgałęzień w testach jest złym pomysłem. Godnym uwagi wyjątkiem są testy generowane automatycznie (być może z dokumentu JSON). Poniżej `t.plan ()` służy do zapewnienia poprawności wejścia JSON:

```js
const testData = require('./fixtures/test-definitions.json');

for (const testDefinition of testData) {
	test('foo or bar', t => {
		const result = functionUnderTest(testDefinition.input);

		// testDefinition should have an expectation for `foo` or `bar` but not both
		t.plan(1);

		if (testDefinition.foo) {
			t.is(result.foo, testDefinition.foo);
		}

		if (testDefinition.bar) {
			t.is(result.bar, testDefinition.foo);
		}
	});
}
```

## Wniosek

`t.plan()` ma wiele uzasadnionych zastosowań, ale nie należy ich używać bez rozróżnienia. Dobrą zasadą jest używanie go za każdym razem, gdy *test* nie ma prostego, łatwego do uzasadnienia przepływu kodu. Testy z asercjami wewnątrz wywołań zwrotnych, deklaracje `if`/`then`, pętle `for`/`while`, i (w niektórych przypadkach) bloki `try`/`catch`, są dobrymi kandydatami na `t.plan()`.
