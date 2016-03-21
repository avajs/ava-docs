___
**Nota del traduttore**

Questa è la traduzione del file [readme.md](https://github.com/sindresorhus/ava/blob/master/readme.md). Questo è il [link](https://github.com/sindresorhus/ava/compare/8d47119458e83d3899683ad3ea3a4c1c01b7dd49...master#diff-8d47119458e83d3899683ad3ea3a4c1c01b7dd49) con le differenza tra il ramo master di AVA ed il commit di quando è stata aggiornato questo file (Se si clicca sul link, e non si vede il file `readme.md` nella lista dei file modificati, questa è traduzione aggiornata).
___
# ![AVA](https://github.com/sindresorhus/ava/raw/master/media/header.png)

> Test runner futuristico

[![Build Status: Linux](https://travis-ci.org/sindresorhus/ava.svg?branch=master)](https://travis-ci.org/sindresorhus/ava) [![Build status: Windows](https://ci.appveyor.com/api/projects/status/igogxrcmhhm085co/branch/master?svg=true)](https://ci.appveyor.com/project/sindresorhus/ava/branch/master) [![Coverage Status](https://coveralls.io/repos/sindresorhus/ava/badge.svg?branch=master&service=github)](https://coveralls.io/github/sindresorhus/ava?branch=master) [![Gitter](https://badges.gitter.im/join chat.svg)](https://gitter.im/sindresorhus/ava)

Nonostante Javascript si possa eseguire in un singolo thread, le operazioni di IO (Input/Output) in Node.js si possono eseguire in parallelo data la sua natura asincrona. AVA trae vantaggio da questa proprietà ed esegue in modo concorrente i tuoi test, portando forti benefici a pesanti test in IO. Inoltre, i file di test sono eseguiti in parallelo in processi distinti, offrendoti un miglioramento ulteriore delle prestazioni oltre che un ambiente isolato dove far girare ogni file di test. Passando da Mocha ad AVA in Pageres si è migliorato il tempo di esecuzione da 31 secondi a 11 secondi. Avendo test eseguiti in modo concorrente si è obbligati a scrivere test atomici, cioè test che non dipendono da un unico stato globale o lo stato impostato da test precedenti, che è grandioso!

*Leggi la nostra [guida per collaboratori](contributing.md) se pensi di contribuire (issue/PRs/etc).*

Segui l'[account Twitter di AVA](https://twitter.com/ava__js) per le ultime notizie.

Traduzioni: [Español](https://github.com/sindresorhus/ava-docs/blob/master/es_ES/readme.md), [Français](https://github.com/sindresorhus/ava-docs/blob/master/fr_FR/readme.md), [日本語](https://github.com/sindresorhus/ava-docs/blob/master/ja_JP/readme.md), [Portugués](https://github.com/sindresorhus/ava-docs/blob/master/pt_BR/readme.md)


## Tabla de contenidos

- [Uso](#uso)
- [Uso della CLI](#cli)
- [Configurazione](#configurazione)
- [Documentazione](#documentazione)
- [API](#api)
- [Asserzioni](#asserzioni)
- [FAQ](#faq)
- [Ricette](#ricette)


## Perchè AVA?

- Minimale e veloce
- Sintassi semplice per i test
- Esegue i test in modo concorrente
- Obbliga a scrivere test atomici
- Non ha variabili globali implicite
- [Ambiente isolato per l'esecuzione di ogni file di test](#ambiente-isolato)
- [Scrivi i tuoi test usando ES2015](#supporto-a-es2015)
- [Supporto per Promesse](#supporto-per-promesse)
- [Supporto per Generatori](#supporto-per-generatori)
- [Supporto per funzioni asincrone](#support-per-funzioni-asincrone)
- [Supporto per Osservabili](#supporto-per-osservabili)
- [Messaggi di asserzione avanzati](#migliori-asserzioni)
- [Produzione TAP opzionale](#produzione-tap-opzionale)
- [Stack traces chiari](#stack-traces-chiari)


## Sintassi per test

```js
import test from 'ava';

test(t => {
	t.same([1, 2], [1, 2]);
});
```


## Uso

#### Aggiungi AVA al tuo progetto

Istalla AVA globalmente e poi eseguilo con `--init` per aggiungere AVA al tuo `package.json`:

```
$ npm install --global ava
$ ava --init
```

```json
{
	"name": "awesome-package",
	"scripts": {
		"test": "ava"
	},
	"devDependencies": {
		"ava": "^0.11.0"
	}
}
```

Ogni parametro passato dopo `--init` viene aggiunto al `package.json`.

#### Istallazione manuale

È anche possibile istallare AVA direttamente

```
$ npm install --save-dev ava
```

Dovrai configurare la sezione di `test` nel `package.json` per usare `ava` correttamente (vedi sopra).

#### Crea il tuo file di test

Crea un file `test.js` nella cartella principale del progetto con il seguente contenuto:


```js
import test from 'ava';

test('foo', t => {
	t.pass();
});

test('bar', async t => {

	const bar = Promise.resolve('bar');

	t.is(await bar, 'bar');
});
```

<img src="https://github.com/sindresorhus/ava/blob/master/screenshot.png" width="150" align="right">

#### Esegui il test

```
$ npm test
```

#### Osserva le modifiche ai file test

```
$ npm test -- --watch
```

AVA integra un sistema intelligente di watch. [Guarda più opzioni nella cartella ricette](docs/recipes/watch-mode.md)

## CLI

![](https://github.com/sindresorhus/ava/raw/master/screenshot-mini-reporter.gif)

```
$ ava --help

  Usage
    ava [<file|directory|glob> ...]

  Options
    --init           Add AVA to your project - (Aggiungi AVA al tuo progetto)
    --fail-fast      Stop after first test failure - (Fermati dopo il primo errore)
    --serial, -s     Run tests serially - (Esegui i test in serie)
    --require, -r    Module to preload (Can be repeated) - ( Moduli da precaricare (può essere ripetuto) )
    --tap, -t        Generate TAP output - (Genera l'output TAP)
    --verbose, -v    Enable verbose output - (Abilita la modalità verbosa)
    --no-cache       Disable the transpiler cache - (Disabilita la cache per il transpiler)
    --match, -m      Only run tests with matching title (Can be repeated) - (Esegui solamente i test con il titolo corrispondente ad un pattern (può essere ripetuto))
    --watch, -w      Re-run tests when tests and source files change - (Esegui nuovamente i test quando i file di test o sorgente cambiano)
    --source, -S     Pattern to match source files so tests can be re-run (Can be repeated) - (Pattern utilizzato per trovare file sorgente così che i test possano venire ri-eseguiti (può essere ripetuto) )

  Examples
    ava
    ava test.js test2.js
    ava test-*.js
    ava test
    ava --init
    ava --init foo.js

  Default patterns when no arguments (pattern predefiniti utilizzati quando nessun parametro viene passato):
  test.js test-*.js test/**/*.js
```

* Da notare che CLI utilizzerà la versione locale di AVA quando disponibile, anche quando eseguito globalmente.

Le cartelle sono ricorsive, tutti i file `*.js` vengono considerati automaticamente file di test. Cartelle con i seguenti nomi sono sempre ignorate: `fixtures`, `helpers` e `node_modules`. Allo stesso modo file il cui nome inizia per `_` vengono ignorati, consentendo di avere file di aiuto/supporto nella stessa cartella dei tuoi file.

## Configurazione

Tutte le opzioni per la CLI possono essere configurati nella sezione `ava` del tuo `package.json`. Questo permette di modificare il comportamento predefinito del comando `ava`, in modo da evitare di ripetere continuamente le stesse opzioni dalla riga di comando.

```json
{
  "ava": {
    "files": [
      "my-test-folder/*.js",
      "!**/not-this-file.js"
    ],
    "source": [
      "**/*.{js,jsx}",
      "!dist/**/*"
    ],
    "match": [
      "*oo",
      "!foo"
    ],
    "failFast": true,
    "tap": true,
    "require": [
      "babel-register"
    ],
    "babel": "inherit"
  }
}
```

Gli argomenti passati alla CLI avranno sempre la precedenza rispetto alla configurazione nel file `package.json`.

## Documentazione

I test vengono eseguiti in modo concorrente. Puoi definire sia test sincroni che asincroni. I test sono considerati sincroni a meno che una promessa o un oggetto osservabile vengano restituiti.

Noi raccomandiamo *particolarmente* l'uso di [funzioni asincrone](#supporto-per-funzioni-asincrone). Queste aiutano a scrivere codice asincrono in modo conciso e leggibile, restituendo implicitamente una promessa così non devi farlo tu.

Se non puoi usare promesse o oggetti osservabili, puoi sempre utilizzare la "modalità callback" definendo il tuo test con la seguente sintassi `test.cb([title], fn)`. I test definiti con questa modalità **dovranno** manualmente terminare con il comando `t.end()`. Questa modalità è principalmente pensata per testare API che utilizzano callback.

I test devono essere definiti in modo sincrono. Non possono essere definiti in istruzioni tipo `setTimeout`, `setInterval`, etc...

I file di test vengono eseguiti da dentro la loro cartella, quindi `process.cwd()` equivale sempre a `__dirname`. Puoi utilizzare percorsi relativi invece della funzione `path.join(__dirname, 'relative/path)`.

### Come creare test

Per creare un test devi utilizzare la funzione `test` importata da AVA, aggiungi un titolo e una callback. La funzione callback verrà eseguita quando il tuo test verrà eseguito. Come argomento della callback viene fornito un [contesto di esecuzione](#t) come primo ed unico argomento. Per convenzione questo parametro è chiamato `t`.

```js
test('name', t => {
	t.pass();
});
```

### Titoli

I titoli sono opzionali, il che vuol dire che puoi scrivere:


```js
test(t => {
	t.pass();
});
```

È comunque consigliato dare un titolo ai propri test se hai più di un test.

Se non hai fornito un titolo ad un test, ma la callback è una funzione con un nome, il nome della funzione verrà utilizzato come titolo del test:

```js
test(function name(t) {
	t.pass();
});
```

### Soglia per le asserzioni

Le soglie per le asserzione garantiscono che i test passino esclusivamente quando un numero specifico di asserzioni è stato eseguito. Possono aiutare a scoprire casi in cui i test terminano anticipatamente. Inoltre se un test eccede il numero soglia impostato di asserzioni allora viene considerato come fallito: questo è comodo in quei casi in cui si usano asserzioni in callback o cicli.

Da notare che, a differenza di [tap](https://www.npmjs.com/package/tap) e [tape](https://www.npmjs.com/package/tape), AVA non termina automaticamente il test quando il numero di asserzioni pianificate viene raggiunto.

Questi sono esempio di test che terminano con successo:

```js
test(t => {
	t.plan(1);

	return Promise.resolve(3).then(n => {
		t.is(n, 3);
	});
});

test.cb(t => {
	t.plan(1);

   someAsyncFunction(() => {
  		t.pass();
  		t.end();
 	});
});
```

Questi test invece no:

```js

// fallirà perché troppe asserzioni (3) vengono eseguite
test(t => {
    t.plan(2);

    for (let i = 0; i < 3; i++) {
        t.true(i < 3);
    }
});

// fallirà perché il test terminerà in modo sincrono prima che l'asserzione venga eseguita
test(t => {
    t.plan(1);

    someAsyncFunction(() => {
        t.pass();
    });
});
```

### Eseguire test in serie

I test vengono eseguiti in modo concorrente in automatico. A volte però avrai bisogno di scrivere test che non siano eseguiti in modo concorrente.

In questi rari casi si può utilizzare il modificatore `.serial`, che obbligherà l'esecuzione dei test seriali *prima* dell'esecuzione dei test concorrenti.

```js
test.serial(t => {
	t.pass();
});
```

Da notare che questo vale per i test all'interno di un particolare file. AVA eseguirà comunque file di test multipli nello stesso istante a meno che venga passato il parametro [`--serial` dalla CLI](#CLI).

### Eseguire test specifici

Durante lo sviluppo ti potrebbe essere utile eseguire solamente pochi test specifici. Puoi eseguire test specifici utilizzando il modificatore `.only`:

```js
test('will not be run', t => {
	t.fail();
})

test.only('will be run', t => {
	t.pass();
});
```

### Filtrare test da eseguire per titolo con un pattern

Il parametro `--match` permette di eseguire solamente i test il cui titolo corrisponde ad un pattern. Si possono utilizzare wildcard pattern. I pattern sono case insensitive. Per maggiori informazioni guarda la documentazione di [`matcher`](https://github.com/sindresorhus/matcher).

Esegui test il cui titolo termina per `foo`:

```
$ ava --match='*foo'
```

Esegui test il cui titolo inizia per `foo`:

```
$ ava --match='foo*'
```

Esegui test il cui titolo contiene `foo`:

```
$ ava --match='*foo*'
```

Esegui test il cui titolo è *esattamente* `foo` (sempre case insensitive):

```
$ ava --match='foo'
```

Esegui test il cui titolo non contiene `foo`:

```
$ ava --match='!*foo'
```

Esegui test il cui titolo inizia per `foo` e termina per `bar`:

```
$ ava --match='foo*bar'
```

Esegui test il cui titolo inizia per `foo` o termina per `bar`:

```
$ ava --match='foo*' --match='*bar'
```

Nota che il filtro per pattern ha la precedenza sul modificatore `.only`. Solamente i test con il titolo esplicito verranno considerati, quindi i test senza titolo o quelli che utilizzano i nome della callback come titolo verranno ignorati quando viene usato `--match`.

In questo esempio viene mostrato cosa succede quando si esegue AVA con un pattern `*oo*` e i test seguenti:

```js
test('foo will run', t => {
    t.pass();
});

test('moo will also run', t => {
    t.pass();
});

test.only('boo will run but not exclusively', t => {
    t.pass();
});

// non verrà eseguito perchè non ha un titolo
test(function (t) {
    t.fail();
});

// non verrà eseguito perchè non ha un titolo esplicito
test(function foo(t) {
    t.fail();
});
```

### Ignorare test

A volte può alcuni test falliscono senza una facile soluzione. In questi casi puoi dire ad AVA di ignorare questi test utilizzando il modificatore `.skip`. Verranno comunque mostrati nell'output (come ignorati) ma non verranno eseguiti.


```js
test.skip('will not be run', t => {
	t.fail();
});
```

La funzione callback è obbligatoria.

### Test da completare ("todo")

Puoi utilizzare il modificatore `.todo` quando vuoi lasciare un nota sull'intenzione di scrivere un test. Come i test ignorati anche questi verranno mostrati nell'output. L'unico requisito è un titolo; non è possibile definire una funzione di callback.

```js
test.todo('will think about writing this later');
```

### Before & after hooks

AVA ti permette di registrare delle funzioni hook da eseguire prima e dopo i tuoi test. Questo ti permette di creare e/o distruggere lo scenario per i tuoi test.

`test.before()` permette di registrare una funzione da eseguire prima del primo test nel tuo file di test. Allo stesso modo `test.after()` permette di eseguire una funzione dopo l'esecuzione dell'ultimo test.

`test.beforeEach()` permette di registrare una funzione da eseguire prima di ogni singolo test nel tuo file di test. Allo stesso modo `test.afterEach()` permette di eseguire una funzione dopo l'esecuzione di ciascun test.

Allo stesso modo di `test()` questi metodi possono avere un titolo opzionale e una funzione callback. Il titolo viene mostrato se l'hook fallisce l'esecuzione, mentre alla funzione callback viene passato il parametro [contesto di esecuzione](#t).

Gli hook `before` vengono sempre eseguiti prima degli hooks `beforeEach`. Gli hook `afterEach` vengono sempre eseguiti prima degli hook `after`. All'interno della loro categoria gli hook vengono eseguiti nell'ordine in cui sono definiti.

```js
test.before(t => {
    // eseguiti prima di tutti i test
});

test.before(t => {
    // eseguito dopo il before sopra, ma prima dei test
});

test.after('cleanup', t => {
    // eseguito al termine di tutti i test
});

test.beforeEach(t => {
    // eseguito prima di ciascun test
});

test.afterEach(t => {
    // eseguito dopo ciascun test
});

test(t => {
    // semplice test
});
```

Gli hook possono essere sia sincroni che asincroni, come i test. Per creare un hook asincrono restituisci una promessa o un oggetto osservabile, utilizza una funzione asincrona, o abilità la modalità callback usando `test.cb.before()`, `test.cb.beforeEach()`, etc...


```js
test.before(async t => {
	await promiseFn();
});

test.after(t => {
   return new Promise(/* ... */);
});

test.cb.beforeEach(t => {
	setTimeout(t.end);
});

test.afterEach.cb(t => {
	setTimeout(t.end);
});

```

Non dimenticare che `beforeEach` e `afterEach` vengono eseguiti appena prima e dopo un test è eseguito, e che i test vengono eseguiti in concorrenza. Se hai bisogno di impostare uno stato globale per ogni test (come impostare una *spia* in `console.log` [ad esempio](https://github.com/sindresorhus/ava/issues/560)), dovrai impostare l'esecuzione dei test in [modo seriale](#eseguire-test-in-serie).

Ricorda che AVA esegue ciascun file di test in un processo isolato. Potresti non aver bisogno di ripristinare lo stato originale dello stato nel'hook `after` poichè verrebbe chiamato non appena prima della chiusura del processo.

Gli hook `beforeEach` e `afterEach` possono condividere lo stesso contesto con il test:

```js
test.beforeEach(t => {
	t.context.data = generateUniqueData();
});

test(t => {
	t.is(t.context.data + 'bar', 'foobar');
});
```

Inizialmente `t.context` è un oggetto, ma può essere sovrascritto come mostrato di seguito:

```js
test.beforeEach(t => {
	t.context = 'unicorn';
});

test(t => {
	t.is(t.context, 'unicorn');
});
```

Il contesto condiviso *non* è disponibile negli hook `before` e `after`.

### Concatenare modificatori

Puoi concatenare i modificatori `.serial`, `.only` e `.skip` in ogni ordine, per `test`, `before`, `after`, `beforeEach` e `afterEach`.
Ad esempio:

```js
test.before.skip([title], testFn);
test.skip.after(....);
test.serial.only(...);
test.only.serial(...);
```

Questo ti permette di aggiungere temporaneamente `.skip` o `.only` alla fine di un test o hook senza dover fare altre modifiche.

### Personalizzare asserzioni

Puoi utilizzare ogni modulo di asserzioni invece o in aggiunta al modulo integrata, a patto che lanci un'eccezione quando un'asserzione fallisce.

L'uso di un modulo esterno non fornisce lo stesso tipo d'esperienza d'uso di [quella integrata](#asserzioni), e allo stesso modo la [soglia per le asserzioni](#soglia-per-le-asserzioni) non funzionerà ([issue #25](https://github.com/sindresorhus/ava/issues/25)).

```js
import assert from 'assert';

test(t => {
    assert(true);
});
```

### Supporto a ES2015

AVA fornisce un supporto integrato per ES2015 mediante [Babel 6](https://babeljs.io). Scrivi i tuoi test con ES2015, non c'è bisogno di altra impostazione da aggiungere. Puoi utilizzare qualunque versione di Babel nel tuo progetto. AVA utilizza una versione propria di Babel integrata preimpostata con [`es2015`](http://babeljs.io/docs/plugins/preset-es2015/) e [`stage-2`](http://babeljs.io/docs/plugins/preset-stage-2/), e con i plugin [`espower`](https://github.com/power-assert-js/babel-plugin-espower) e [`transform-runtime`](https://babeljs.io/docs/plugins/transform-runtime/).

La configurazione corrispondente in Babel per il setup di AVA è la seguente:

```json
{
  "presets": [
    "es2015",
    "stage-2",
  ],
  "plugins": [
    "espower",
    "transform-runtime"
  ]
}
```

Puoi personalizzare come AVA utilizzi il transpiler per i file di test mediante l'opzione `babel` nella [configurazione `package.json`](#configurazione) di AVA.
Per sovrascrivere le impostazioni predefinita puoi usare il seguente esempio:

```json
{
	"ava": {
		 "babel": {
			 "presets": [
					"es2015",
					"stage-0",
					"react"
			 ]
		 }
	},
}
```

In aggiunta puoi utilizzare la speciale keyword `"inherit"`. Questa fa sì che AVA demandi la configurazione di Babel al tuo [`.babelrc` o file `package.json`](https://babeljs.io/docs/usage/babelrc/). In questo modo il transpiler utilizzerà la stessa configurazione dei tuoi file sorgente per i tuoi file di test senza alcuna specifica ripetizione per AVA.

```json
{
   "babel": {
	   "presets": [
		   "es2015",
		   "stage-0",
		   "react"
	   ]
   },
   "ava": {
	   "babel": "inherit",
   },
}
```

Da notare che AVA applicherà *sempre* i plugin [`espower`] (https://github.com/power-assert-js/babel-plugin-espower) e [`transform-runtime`] (https://babeljs.io/docs/plugins/transform-runtime/).

### Support di TypeScript

AVA include i typings per TypeScript. La configurazione del transpiler dovrà però essere fatta manualmente. Quando imposterai nel file `tsconfig.json` `module` su `commonjs`, TypeScript troverà automaticamente la definizione dei tipi per AVA. Dovrai impostare `target` su `es2015` per poter utilizzare le promesse e le funzioni asincrone.

### Usare il transpiler per i moduli importati

AVA al momento utilizza il transpiler solamente per i test che richiedi di eseguire. *Non verrà utilizzato il transpiler per i moduli importati al di fuori dei test*. Benchè ci sia una ragione valida per questo comportamento, potrebbe non essere quel che ci si aspetta.

Come soluzione alternativa, utilizzando Babel, si può utilizzare il suo [require hook](https://babeljs.io/docs/usage/require/] per i moduli importati sul momento. Esegui AVA con `--require babel-register` (vedi [CLI](#CLI) ) o [aggiungi l'impostazione nel tuo `package.json`](#configurazione).

Puoi anche utilizzare il transpiler per i tuoi moduli in un processo separato e utilizzare i file prodotti invece dei sorgenti nei tuoi test.

### Supporto per Promesse

Se restituisci una promessa nel tuo test non hai bisogno di segnalare il termine del test esplicitamente perchè questo terminerà quando la promessa sarà risolta.

```js
test(t => {
	return somePromise().then(result => {
		t.is(result, 'unicorn');
	});
});
```

### Support per Generatori

AVA fornisce supporto nativo per i [generatori](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Statements/function*).

```js
test(function * (t) {
	const value = yield generatorFn();
	t.true(value);
});
```

### Supporto per funzioni asincrone

AVA fornisce supporto nativo per [funzioni asincrone](https://tc39.github.io/ecmascript-asyncawait/) *(async/await)*.

```js
test(async function (t) {
	const value = await promiseFn();
	t.true(value);
});

// async arrow function
test(async t => {
	const value = await promiseFn();
	t.true(value);
});
```

### Support per Oggetti Osservabili

AVA fornisce supporto nativo per [oggetti osservabili](https://github.com/zenparsing/es-observable). Se restituisci un oggetto osservabile da un test, AVA attenderà automaticamente il completamento di questo prima di terminare il test.

*Non c'è bisogno di usare la modalità "callback" o chiamare `t.end()`.*

```js
test(t => {
	t.plan(3);
	return Observable.of(1, 2, 3, 4, 5, 6)
		.filter(n => {
			// solo numeri pari
			return n % 2 === 0;
		})
		.map(() => t.pass());
});
```

### Supporto per Callback

In AVA è possibile utilizzare callback in stile Node.js (errore come primo parametro) mediante `t.end()` nella callback finale. AVA considera il primo argomento non-falso passato a `t.end` come un error. Da notare che `t.end` richiede la "modalità callback" che può essere abilitata utilizzando il modificatore `test.cb`.


```js
test.cb(t => {
	// t.end verificherà automaticamente il primo argomento per errori
	fs.readFile('data.txt', t.end);
});
```

### Produzione TAP opzionale

AVA può produrre output TAP mediante l'opzione `--tap` per l'utilizzo di ogni [TAP reporter](https://github.com/sindresorhus/awesome-tap#reporters).

```
$ ava --tap | tap-nyan
```

<img src="https://github.com/sindresorhus/ava/blob/master/media/tap-output.png" width="398">

### Stack trace chiari

AVA nasconde automaticamente righe non attinenti ai tuoi test nello stack trace, permettendoti di trovare la sorgente di ogni errore molto più rapidamente.

<img src="https://github.com/sindresorhus/ava/blob/master/media/stack-traces.png" width="300">


## API

### test([titolo], callback)
### test.serial([titolo], callback)
### test.cb([titolo], callback)
### test.only([titolo], callback)
### test.skip([titolo], callback)
### test.before([titolo], callback)
### test.after([titolo], callback)
### test.beforeEach([titolo], callback)
### test.afterEach([titolo], callback)

#### titolo

Tipo: `string`

Titolo del test.

#### callback(t)

Tipo: `function`

Deve contere il corpo del test.

##### t

Tipo: `oggetto`

Il contesto di esecuzione di uno specifico test. Ogni callback di un test riceve un oggetto distinto. Contiene le asserzioni insieme ai metodi  `.plan(limite)` e `.end()`. `t.context` può contenere lo stato condiviso dall'hook `beforeEach`.

###### t.plan(limite)

Imposta quante asserzioni ci sono in un test. Il test fallirà se il numero di asserzioni eseguite supera il numero qui impostato. Vedi [soglia per le asserzioni](#soglia-per-le-asserzioni).

###### t.end()

Termina il test. Funziona unicamente con `test.cb()`.

## Asserzioni

Le asserzioni sono contenute nel [contesto di esecuzione](#contesto-di-esecuzione) del test:

```js
test(t => {
	t.ok('unicorn'); // asserzione
});
```

Se più di un'asserzione fallisce all'interno di un test, AVA mostrerà solamente la *prima*.

### .pass([messaggio])

Fa passare l'asserzione.

### .fail([messaggio])

Fa fallire l'asserzione.

### .ok(valore, [messaggio])

Accerta che il `valore` sia non-falso.

### .notOk(valore, [messaggio])

Accerta che il `valore` sia falso.

### .true(valore, [messaggio])

Accerta che il `valore` sia `true`.

### .false(valore, [messaggio])

Accerta che il `valore` sia `false`.

### .is(valore, previsto, [messaggio])

Accerta che il `valore` si uguale a `previsto`.

### .not(valore, previsto, [messaggio])

Accerta che il `valore` non sia uguale a `previsto`.

### .same(valore, previsto, [messaggio])

Accerta che il `valore` sia profondamente uguale a `previsto`.

### .notSame(valore, previsto, [messaggio])

Accerta che il `valore` non sia profondamente uguale a `previsto`.

### .throws(function|promise, [errore, [message]])

Accerta che `function` lanci un errore o che  la `promise` sia rifiutata.

`errore` può essere un costruttore, espressione regolare, messaggio di errore o una funzione di validazione.

Restituisce l'errore lanciato dalla `function` o dal ragione del rifiuto della promessa `promise`.

### .notThrows(function|promise, [messaggio])

Accerta che `function` non lanci un `errore` o che la promessa `promise` sia risolta.

### .regex(contenuto, regex, [message])

Accerta che `contenuto` corrisponda all'espressione regolare `regex`.

### .ifError(errore, [messaggio])

Accerta che `errore` sia falso.

## Ignorare asserzioni

Ogni asserzione può essere ignorata utilizzando il modificatore `.skip`. Le assegnazioni ignorate sono comunque considerate nel conto della soglia, quindi non c'è alcuna necessità di cambiarla.

```js
test(t => {
  t.plan(2);
  t.skip.is(foo(), 5); // non c'è la necessità di cambiare il valore della soglia quando si ignora
  t.is(1, 1);
});
```

## Migliorare messaggi delle asserzioni

AVA integra il modulo [`power-assert`](https://github.com/power-assert-js/power-assert), dandoti messaggi più descrittivi per le asserzioni. Leggendo il tuo test questo modulo cerca di inferire più informazioni riguardo il codice.

Facciamo un esempio, utilizzando il modulo Node.js standard [assert](https://nodejs.org/api/assert.html):

```js
const a = /foo/;
const b = 'bar';
const c = 'baz';
require('assert').ok(a.test(b) || b === c);
```

Provando a scrivere l'esempio sopra nella console Node.js si ha:

```
AssertionError: false == true
```

In AVA invece, questo test:

```js
test(t => {
    const a = /foo/;
    const b = 'bar';
    const c = 'baz';
    t.ok(a.test(b) || b === c);
});
```

Stamperà:
```
t.ok(a.test(b) || b === c)
       |    |     |     |
       |    "bar" "bar" "baz"
       false
```

## Isolamento dei processi

Ogni test file viene eseguito in un processo Node.js separato. Questo ti permette di cambiare lo stato globale o sovrascrivere un'impostazione in un file di test, senza avere conseguenze in altri file di test. È anche un bel vantaggio dal punto di vista delle performance con i moderni processori multi-core, permettendo a più file di test di essere eseguiti in parallelo.

## Consigli

### File temporanei

Eseguire test in concorrenza comporta alcune complicazioni, ad esempio l'IO è una.

Generalmente, i test eseguiti in serie creano cartelle temporanee nella cartella di test corrente per poi eliminarla al termine. Questo approccio non funziona quando i testi sono eseguiti concorrentemente poichè i test potrebbero entrare in conflitto l'uno con l'altro. L'approccio migliore per questo tipo di problema è quello di utilizzare una nuova cartella temporanea per ciascun file di test. I moduli [`tempfile`](https://github.com/sindresorhus/tempfile) e [`temp-write`](https://github.com/sindresorhus/temp-write) possono aiutare in questo.

### Debugging

AVA esegue i test in concorrenza in automatico, il che lo rende sub-ottimale quando devi fare il debug di qualcosa. In questo caso esegui i test in serie con l'opzione `--serial`:

```
$ ava --serial
```

### Code coverage

Non è possibile utilizzare [`istanbul`](https://github.com/gotwarlost/istanbul) per il code coverage dato l'[uso di processi isolati](#processi-isolati) in AVA. In questo caso puoi usare [`nyc`](https://github.com/bcoe/nyc), poichè si tratta sostanzialmente di `istanbul` con il supporto per sub-processi.

Dalla versione `5.0.0` utilizza anche le source maps per la rappresentazione del code coverage per il tuo codice sorgente, a prescindere dai file prodotti dal transpiler. Assicurati che il file che stai testando abbia una source map inline oppure referenzi un file source map. Se utilizzi `babel-register` puoi specificare l'opzione `sourceMaps` come `inline` nella configurazione Babel.

## Domande frequenti

### Perchè non usare `mocha`, `tape`, `node-tap`?

Mocha richiede che tu usi funzioni globali come `describe` o `it` con l'interfaccia di default (usata dalla maggior parte delle persone). Non impone una struttura precisa ed esegue test in serie senza alcun isolamento a livello di processo, rendendolo lento.

Tape e tap sono buoni moduli. AVA ha preso ispirazione dalla loro sintassi. Anche loro eseguono test in serie. Il loro output [TAP](https://testanything.org) integrato non è molto pratico da usare e quindi si finisce sempre per usare un reporter TAP esterno.

Viceversa AVA impone una struttura precisa ed esegue i test in modo concorrente, ogni file di test in un processo separato. Il suo reporter integrato è facile da comprendere a prima vista, nonostante ciò AVA supporta anche l'output TAP mediante il parametro da CLI.

### Come posso utilizzare reporter personalizzati?

AVA supporta il formato TAP ed è quindi compatibile con ogni [TAP reporter](https://github.com/sindresorhus/awesome-tap#reporters). Usa il [parametro `--tap`](#produzione-tap-opzionale) per abilitare l'output TAP.

### Come si scrive e pronuncia AVA?

AVA, no Ava o ava. Si pronuncia [`/ˈeɪvə/` ay-və](https://github.com/sindresorhus/ava/blob/master/media/pronunciation.m4a?raw=true).

### Cos'è lo sfondo del logo in cima?

La [galassia di Andromeda](https://es.wikipedia.org/wiki/Galaxia_de_Andr%C3%B3meda).

### Qual è la differenza tra concorrenza e parallelismo?

[Concorrenza non è parallelismo, ma lo abilita.](http://stackoverflow.com/q/1050222)

## Ricette

- [Code coverage](docs/recipes/code-coverage.md)
- [Test di Endpoint](docs/recipes/endpoint-testing.md)
- [Quando usare `t.plan()`](docs/recipes/when-to-use-plan.md)
- [Browser testing](docs/recipes/browser-testing.md)
- [TypeScript](docs/recipes/typescript.md)


## Supporto

- [Stack Overflow](https://stackoverflow.com/questions/tagged/ava)
- [Gitter chat](https://gitter.im/sindresorhus/ava)
- [Twitter](https://twitter.com/ava__js)


## Guarda anche

- [sublime-ava](https://github.com/sindresorhus/sublime-ava) - Frammenti di codice per test con AVA
- [atom-ava](https://github.com/sindresorhus/atom-ava) - Frammenti di codice per test con AVA
- [vscode-ava](https://github.com/samverschueren/vscode-ava) - Frammenti di codice per test con AVA
- [eslint-plugin-ava](https://github.com/sindresorhus/eslint-plugin-ava) - Regole Lint per test AVA
- [gulp-ava](https://github.com/sindresorhus/gulp-ava) - Esegui i test con gulp
- [grunt-ava](https://github.com/sindresorhus/grunt-ava) - Esegui i test con grunt
- [fly-ava](https://github.com/pine613/fly-ava) - Esegui i test con fly
- [start-ava](https://github.com/start-runner/ava) - Esegui i test con start

[Più moduli](https://github.com/sindresorhus/awesome-ava#packages)

## Link

- [Compra adesivi di AVA](https://www.stickermule.com/user/1070705604/stickers)
- [Lista fantastica](https://github.com/sindresorhus/awesome-ava)


## Creato da

[![Sindre Sorhus](https://avatars.githubusercontent.com/u/170270?s=130)](http://sindresorhus.com) | [![Kevin Mårtensson](https://avatars.githubusercontent.com/u/709159?s=130)](https://github.com/kevva) | [![Vadim Demedes](https://avatars.githubusercontent.com/u/697676?s=130)](https://github.com/vdemedes) | [![James Talmage](https://avatars.githubusercontent.com/u/4082216?s=130)](https://github.com/jamestalmage) | [![Mark Wubben](https://avatars.githubusercontent.com/u/33538?s=130)](https://novemberborn.net)
---|---|---|---|---
[Sindre Sorhus](http://sindresorhus.com) | [Kevin Mårtensson](https://github.com/kevva) | [Vadim Demedes](https://github.com/vdemedes) | [James Talmage](https://github.com/jamestalmage) | [Mark Wubben](https://novemberborn.net)


<div align="center">
	<br>
	<br>
	<br>
	<img src="https://cdn.rawgit.com/sindresorhus/ava/fe1cea1ca3d2c8518c0cc39ec8be592beab90558/media/logo.svg" width="200" alt="AVA">
	<br>
	<br>
</div>
