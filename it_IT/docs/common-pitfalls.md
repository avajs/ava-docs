___
**Nota del traduttore**

Questa è la traduzione del file [common-pitfalls.md](https://github.com/avajs/ava/blob/main/docs/common-pitfalls.md). Qui c'è il [link](https://github.com/avajs/ava/compare/c09462c3e515c41da8177a3d9ba5fb0f19759653...main#diff-7eb46230db3eba276054b9adbc6c82ca) dove si confrontano le differenze tra commit di riferimento di questa traduzione e l'ultimo commit di AVA sul branch master (Se si clicca sul link, e non si vede il file `common-pitfalls.md` nella lista dei file modificati, questa traduzione è aggiornata).
___
# Errori comuni

Traduzioni: [Français](https://github.com/avajs/ava-docs/blob/main/fr_FR/docs/08-common-pitfalls.md),
[Italiano](https://github.com/avajs/ava-docs/blob/main/it_IT/docs/common-pitfalls.md)

## Plugin ESLint

Se usi [ESLint](http://eslint.org/), puoi installare [eslint-plugin-ava](https://github.com/avajs/eslint-plugin-ava). Ti aiuterà ad usare correttamente AVA e ad evitare alcuni errori comuni.

## AVA in Docker

Se usi AVA in Docker nel tuo CI, hai bisogno di impostare le variabili d'ambiente correttamente. Nello specifico aggiungi `-e CI=true` nel comando `docker exec`. Per maggiori informazioni guarda [#751](https://github.com/avajs/ava/issues/751).

AVA usa [is-ci](https://github.com/watson/is-ci) per capise se è eseguito in un ambiente di CI o no, basandosi su [questi variabili](https://github.com/watson/is-ci/blob/master/index.js).

## AVA ed il limite di client connessi

Potresti utilizzare un servizio che permette un numero limitato di connessioni concorrenti. Ad esempio, molte aziende con prodotti Database-as-a-service offrono piani con un numero limitato di client connessi nello stesso momento. AVA può superare questi limiti poichè esegue processi multiple, ma questi servizi dovrebbero inviare un errore o incodare la richiesta in questi casi. Se il tuo servizio non si comporta in questo modo, i test potrebbero rimanere in sospeso.

Usa il flag `concurrency` per limitare il numero di processi da eseguire in AVA. Per esempio, se il piano del tuo servizio ti permette di eseguire 5 client alla volta, dovresti configurare AVA con `concurrency=5` o meno.

## Operazioni Asincrone

Talvolta test con operazioni asincrone potrebbero rimanere appesi e non terminare. Se utilizzi Promises per le operazioni asincrone, ricorda di restituire la Promise finale:


```js
test(t => {
	return fetch().then(data => {
		t.is(data, 'foo');
	});
});
```

Ancor meglio, usa `async` / `await`:

```js
test(async t => {
	const data = await fetch();
	t.is(data, 'foo');
});
```

Se invece stai usando le callbacks, usa [`test.cb`](https://github.com/avajs/ava#callback-support):

```js
test.cb(t => {
	fetch((err, data) => {
		t.is(data, 'foo');
		t.end();
	});
});
```

Come ultima alternativa, trasforma la callback in Promise con qualcosa come [`pify`](https://github.com/sindresorhus/pify):

```js
test(async t => {
	const data = await pify(fetch)();
	t.is(data, 'foo');
});
```

### Attribuzione di errori non catturati nei test

AVA [non può tracciare errori non catturati](https://github.com/avajs/ava/issues/214) e associarli al giusto test che l'ha lanciato. Spesso utilizzando callback nei test può indurre a errori non catturati che possono essere difficili da risolvere. Considera il processo di "promisify" e l'utilizzo di `async` / `await`, come nell'esempio sopra. Questo permette ad AVA di catturare gli errori e attribuirle correttamente al test che l'ha lanciato.

### Perchè i messaggi delle asserzioni avanzate non vengono mostrati?

Controlla che il primo parametro passato al tuo test sia chiamato `t`. Questo è un requisito di [`power-assert`](https://github.com/power-assert-js/power-assert), cioè la libreria usata per i messaggi avanzati.

```js
test(t => {
	t.is(1, 1);
});
```

---

Il problema che hai non è in questa lista? Invia una pull request o commenta in [questa issue](https://github.com/avajs/ava/issues/404). 
