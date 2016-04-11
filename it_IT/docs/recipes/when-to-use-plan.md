___
**Nota del traduttore**

Questa è la traduzione del file [when-to-use-plan.md](https://github.com/sindresorhus/ava/blob/master/docs/recipes/when-to-use-plan.md). Qui c'è il [link](https://github.com/sindresorhus/ava/compare/3201b1b4ff80ff75f0e1c288ca7da22f92c9b814...master#diff-0730bb7c2e8f9ea2438b52e419dd86c9) dove si confrontano le differenze tra commit di riferimento di questa traduzione e l'ultimo commit di AVA sul branch master (Se si clicca sul link, e non si vede il file `when-to-use-plan.md` nella lista dei file modificati, questa traduzione è aggiornata).
___

# Quando usare `t.plan()`

Traduzioni: [Español](https://github.com/sindresorhus/ava-docs/blob/master/es_ES/docs/recipes/when-to-use-plan.md), [Français](https://github.com/sindresorhus/ava-docs/blob/master/fr_FR/docs/recipes/when-to-use-plan.md), [日本語](https://github.com/sindresorhus/ava-docs/blob/master/ja_JP/docs/recipes/when-to-use-plan.md),  [Português](https://github.com/sindresorhus/ava-docs/blob/master/pt_BR/docs/recipes/when-to-use-plan.md), [Русский](https://github.com/sindresorhus/ava-docs/blob/master/ru_RU/docs/recipes/when-to-use-plan.md)

Una delle differenze sostanziali tra AVA e [`tap`](https://github.com/tapjs/node-tap)/[`tape`](https://github.com/substack/tape) è il comportamento della funzione `t.plan()`. In AVA, `t.plan()` è solamente usato per verificare che il numero previsto di asserzioni sia rispettato, ma non termina automaticamente il test.

## Uso superficiale di `t.plan()`

Molti utenti venendo da `tap`/`tape` sono abituati ad usare `t.plan()` abbondantemente in ogni test. In ogni caso, in AVA, non viene considerato una "buona pratica". Invece `t.plan()` dovrebbe essere usato in scenari dove potrebbe realmente aggiungere valore al test.

### Test sincroni senza ramificazioni

Non è necessario usare `t.plan()` nella maggior parte dei test sincroni.

```js
test(t => {
	// Sbagliato: non c'è alcuna ramificazione qui - t.plan() non è utile
	t.plan(2);

	t.is(1 + 1, 2);
	t.is(2 + 2, 4);
});
```

`t.plan()` non fornisce alcun valore aggiunto in questo caso, ma aggiunge solo difficoltà se volessi decidere di aggiungere o rimuovere un'asserzione.

### Promesse che ci si aspetta siano risolte

```js
test(t => {
	t.plan(1);

	return somePromise().then(result => {
		t.is(result, 'foo');
	});
});
```

Ad una prima occhiata questo test sembra usare giustamente `t.plan()` dato che un handler per una promessa asincrona è coinvolta. Ci sono però alcuni problemi con il test:\

1. Probabilmente `t.plan()` è stato usato per proteggersi contro la possibilità che `somePromise()` potrebbe essere rifiutata; Restituire una promessa rifiutata farebbe fallire il test in ogni caso.

2. Sarebbe meglio sfruttare la funzionalità `async`/`await`;

```js
test(async t => {
	t.is(await somePromise(), 'foo');
});
```

## Buon uso di `t.plan()`

Ci sono molti casi in cui vale la pena usare `t.plan()`.

### Promesse con un handler `.catch()`

```js
test(t => {
	t.plan(2);

	return shouldRejectWithFoo().catch(reason => {
		t.is(reason.message, 'Hello') // Valuta t.throws() se sei solamente interessato al messaggio d'errore
		t.is(reason.foo, 'bar');
	});
});
```

In questo caso `t.plan()` è usato per assicurarsi che il codice all'interno del blocco `catch` sia eseguito. In molti casi, dovresti considerare l'uso dell'asserzione `t.throws()`, ma è considerato un uso accettabile di `t.plan()` poichè `t.throws()` ti permette solamente di verificare la proprietà `message` dell'errore.

### Assicurarsi che un blocco catch venga eseguito

```js
test(t => {
	t.plan(2);

	try {
		shouldThrow();
	} catch (err) {
		t.is(err.message, 'Hello') // Valuta t.throws() se sei solamente interessato al messaggio d'errore
		t.is(err.foo, 'bar');
	}
});
```

Come già specificato nel blocco `try`/`catch` sopra, utilizzare l'asserzione `t.throws()` è generalmente una scelta migliore, anche se ti permette solamente di verificare la proprietà `message` dell'errore.

### Assicurati che callback multiple vengano eseguite

```js
test.cb(t => {
	t.plan(2);

	const callbackA = () => {
		t.pass();
		t.end();
	};

	const callbackB = () => t.pass();

	bThenA(callbackA, callbackB);
});
```

Il codice sopra verifica che `callbackB` sia chiamata prima (ed una sola volta), seguita poi da `callbackA`. Ogni altra combinazione non soddisferebbe la soglia impostata.

### Test con raminificazioni

In molti scenari, è una cattiva idea usare complicate ramificazioni nei tuoi test. Un'eccezione particolare riguarda i test che vengono generati automaticamente (per esempio da un file JSON). Qui sotto `t.plan()` è usato per garantire la conformità dell'input JSON:

```js
const testData = require('./fixtures/test-definitions.json');

testData.forEach(testDefinition => {
	test(t => {
		const result = functionUnderTest(testDefinition.input);

		// testDefinition dovrebbe avere solo uno tra `foo` o `bar` ma non entrambi
		t.plan(1);

		if (testDefinition.foo) {
			t.is(result.foo, testDefinition.foo);
		}

		if (testDefinition.bar) {
			t.is(result.bar, testDefinition.foo);
		}
	});
});
```

## Conclusioni

Ci sono molti usi validi per `t.plan()`, ma questo non vuole dire che può essere usato in modo indiscriminato. Una semplice regola da seguire è usarlo ogni volta che il tuo *test* non ha un codice con un flusso diretto o facile da comprendere. I testi con asserzioni dentro callback, blocchi `if`/`then`, blocchi `for`/`while` e (in certi casi) `try`/`catch` sono tutti buoni candidati per l'uso di `t.plan()`.
