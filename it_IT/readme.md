___
**Nota del traduttore**

Questa è la traduzione del file [readme.md](https://github.com/avajs/ava/blob/master/readme.md). Qui c'è il [link](https://github.com/avajs/ava/compare/c09462c3e515c41da8177a3d9ba5fb0f19759653...master#diff-0730bb7c2e8f9ea2438b52e419dd86c9) dove si confrontano le differenze tra commit di riferimento di questa traduzione e l'ultimo commit di AVA sul branch master (Se si clicca sul link, e non si vede il file `readme.md` nella lista dei file modificati, questa traduzione è aggiornata).
___
# [![AVA](https://github.com/avajs/ava/raw/master/media/header.png)](https://ava.li)

> Test runner futuristico

[![Build Status: Linux](https://travis-ci.org/avajs/ava.svg?branch=master)](https://travis-ci.org/avajs/ava) [![Build status: Windows](https://ci.appveyor.com/api/projects/status/e7v91mu2m5x48ehx/branch/master?svg=true)](https://ci.appveyor.com/project/ava/ava/branch/master) [![Coverage Status](https://coveralls.io/repos/github/avajs/ava/badge.svg?branch=master)](https://coveralls.io/github/avajs/ava?branch=master) [![Dependency Status](https://dependencyci.com/github/avajs/ava/badge)](https://dependencyci.com/github/avajs/ava) [![XO code style](https://img.shields.io/badge/code_style-XO-5ed9c7.svg)](https://github.com/sindresorhus/xo) [![Gitter](https://badges.gitter.im/join_chat.svg)](https://gitter.im/avajs/ava)

Nonostante Javascript si possa eseguire in un singolo thread, le operazioni di IO (Input/Output) in Node.js si possono eseguire in parallelo data la sua natura asincrona. AVA trae vantaggio da questa proprietà ed esegue in modo concorrente i tuoi test, ottenendo molti benefici per test pesanti in IO. Inoltre, i file di test sono eseguiti in parallelo in processi distinti, offrendoti un miglioramento ulteriore delle prestazioni, oltre che un ambiente isolato dove far girare ogni file di test. [Passando](https://github.com/sindresorhus/pageres/commit/663be15acb3dd2eb0f71b1956ef28c2cd3fdeed0) da Mocha ad AVA in Pageres si è migliorato il tempo di esecuzione da 31 secondi a 11 secondi. Con test eseguiti in modo concorrente si è obbligati a scrivere test atomici, cioè test che non dipendono da un unico stato globale o lo stato impostato da test precedenti, che è grandioso!

![](https://github.com/avajs/ava/raw/master/media/mini-reporter.gif)

*Leggi la nostra [guida per collaboratori](contributing.md) se pensi di contribuire (issue/PRs/etc).*

Segui l'[account Twitter di AVA](https://twitter.com/ava__js) per le ultime notizie.

Translations: [Español](https://github.com/avajs/ava-docs/blob/master/es_ES/readme.md), [Français](https://github.com/avajs/ava-docs/blob/master/fr_FR/readme.md), [Italiano](https://github.com/avajs/ava-docs/blob/master/it_IT/readme.md), [日本語](https://github.com/avajs/ava-docs/blob/master/ja_JP/readme.md), [한국어](https://github.com/avajs/ava-docs/blob/master/ko_KR/readme.md), [Português](https://github.com/avajs/ava-docs/blob/master/pt_BR/readme.md), [Русский](https://github.com/avajs/ava-docs/blob/master/ru_RU/readme.md), [简体中文](https://github.com/avajs/ava-docs/blob/master/zh_CN/readme.md)


## Indice dei contenuti

- [Uso](#uso)
- [Uso della CLI](#cli)
- [Debugging](#debugging)
- [Reporters](#reporters)
- [Configurazione](#configurazione)
- [Documentazione](#documentazione)
- [API](#api)
- [Asserzioni](#asserzioni)
- [Snapshot testing](#snapshot-testing)
- [Consigli](#consigli)
- [Domande frequenti](#domande-frequenti)
- [Ricette](#ricette)


## Perchè AVA?

- Minimale e veloce
- Sintassi semplice per i test
- Esegue i test in modo concorrente
- Obbliga a scrivere test atomici
- Non ha variabili globali implicite
- Include i type definition per TypeScript & Flow
- [Asserzioni magiche](#asserzioni-magiche)
- [Ambiente isolato per l'esecuzione di ogni file di test](#isolamento-dei-processi)
- [Scrivi i tuoi test usando ES2017](#supporto-a-es2017)
- [Supporto per Promesse](#supporto-per-promesse)
- [Supporto per Generatori](#supporto-per-generatori)
- [Supporto per funzioni asincrone](#supporto-per-funzioni-asincrone)
- [Supporto per Oggetti Osservabili](#supporto-per-oggetti-osservabili)
- [Messaggi di asserzione avanzati](#migliorare-messaggi-delle-asserzioni)
- [Reporter TAP](#reporter-tap)
- [Migrazione automatica da altri test runner](https://github.com/avajs/ava-codemods#migrating-to-ava)


## Sintassi per test

```js
import test from 'ava';

test(t => {
	t.same([1, 2], [1, 2]);
});
```


## Uso

#### Aggiungi AVA al tuo progetto

Installa AVA globalmente e poi eseguilo con `--init` per aggiungere AVA al tuo `package.json`. [Yarn](https://yarnpkg.com/) fornisce al momento prestazioni migliori rispetto a npm per l'installazione. Considera l'uso di [Yarn](https://yarnpkg.com/en/docs/install) se l'installazione dovesse essere troppo lenta.

```console
$ yarn global add ava
$ ava --init
```

Se preferisci usare npm invece:

```console
$ npm install --global ava
$ ava --init
```

Modifica il tuo file `package.json` come il seguente:

```json
{
	"name": "awesome-package",
	"scripts": {
		"test": "ava"
	},
	"devDependencies": {
		"ava": "^0.19.0"
	}
}
```

Ogni parametro passato dopo `--init` viene aggiunto alla configurazione in `package.json`.

#### Installazione manuale

È anche possibile installare AVA direttamente:

```console
$ yarn add --dev ava
```

Se preferisci usare npm invece:

```console
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
    --verbose, -v    Enable verbose output - (Abilita la modalità prolissa)
    --no-cache       Disable the transpiler cache - (Disabilita la cache per il transpiler)
    --no-power-assert       Disable Power Assert - (Disabilita i power assert)
    --color                 Force color output - (Forza i colori nell'output)
    --no-color              Disable color output - (Disabilita i colori nell'output)
    --match, -m      Only run tests with matching title (Can be repeated) - (Esegui solamente i test con il titolo corrispondente ad un pattern (può essere ripetuto))
    --watch, -w      Re-run tests when tests and source files change - (Esegui nuovamente i test quando i file di test o sorgente cambiano)
    --timeout, -T           Set global timeout - (Imposta un timeout globale)
    --concurrency, -c       Maximum number of test files running at the same time (EXPERIMENTAL) - (Imposta il limita massimo di test che possono essere eseguiti in concorrenza (ANCORA SPERIMENTALE))
    --update-snapshots, -u  Update snapshots - (Aggiorna snapshots)

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

Quando viene usato `npm test` è possibile passare parametri posizionali `npm test test2.js`, ma i `flags` devono essere passati in questo modo `npm test -- --verbose`.

## Debugging

AVA esegue i test in processi figli del processo principale (child processes), perciò per fare il debugging dei test avrai bisogno di questo trucco:

```console
$ node --inspect node_modules/ava/profile.js some/test/file.js
```

### Consigli specifici per il debugging

- [Chrome DevTools](https://github.com/avajs/ava-docs/blob/master/it_IT/docs/recipes/debugging-with-chrome-devtools.md)
- [WebStorm](https://github.com/avajs/ava-docs/blob/master/it_IT/docs/recipes/debugging-with-webstorm.md)
- [Visual Studio Code](https://github.com/avajs/ava-docs/blob/master/it_IT/docs/recipes/debugging-with-vscode.md)

## Reporters

### Mini-reporter

Il reporter predefinito è il mini-reporter

<img src="https://github.com/avajs/ava/raw/master/media/mini-reporter.gif" width="460">

### Reporter completo

Per abilitare il reporter più completo e prolisso, usa il `flag` `--verbose`. Questo reporter viene utilizzato automaticamente in ambienti CI a meno che il [reporter TAP](#reporter-tap) non venga abilitato.

<img src="https://github.com/avajs/ava/raw/master/media/verbose-reporter.png" width="294">

### Reporter TAP

AVA suporta il formato TAP ed è compatibile con [ogni reporter TAP](https://github.com/sindresorhus/awesome-tap#reporters). Per abilitare l'output TAP utilizza il `flag` `--tap`.

```console
$ ava --tap | tap-nyan
```

<img src="https://github.com/avajs/ava/raw/master/media/tap-reporter.png" width="420">

Attenzione che il reporter TAP non è disponibile quando AVA viene eseguito in [watch mode](#osserva-le-modifiche-ai-file-test).

### Asserzioni magiche

AVA aggiunge parziali di codice mostrando le differenze tra il codice attuale e quello previsto in modo chiaro. Se i valori nelle asserzioni sono oggetti o Array, solamente la differenza verrà mostrata, al fine di rimuovere il rumore e focalizzarsi sul problema trovato. Il codice mostrato avrà anche lo style della sintassi! Se stai comparando stringhe di testo, che siano su riga singola o multipla, AVA mostrerà un tipo di output diverso, evidenziando la parte aggiunta o mancante nella stringa.

![](https://github.com/avajs/ava/raw/master/media/magic-assert-combined.png)

### Stack trace puliti

AVA rimuove automaticamente righe di codice non importanti dagli stack trace, facilitando e velocizzando la ricerca della fonte del problema, come mostrato sopra.

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
    "concurrency": 5,
    "failFast": true,
    "failWithoutAssertions": false,
    "tap": true,
    "powerAssert": false,
    "require": [
      "babel-register"
    ],
    "babel": "inherit"
  }
}
```

Gli argomenti passati alla CLI avranno sempre la precedenza rispetto alla configurazione nel file `package.json`.

Vedi la sezione [supporto a ES2017](#support-a-es2017) per dettagli sull'opzione `babel`.

## Documentazione

I test vengono eseguiti in modo concorrente. Puoi definire sia test sincroni che asincroni. I test sono considerati sincroni a meno che una promessa o un oggetto osservabile vengano restituiti.

Noi raccomandiamo *particolarmente* l'uso di [funzioni asincrone](#supporto-per-funzioni-asincrone). Queste aiutano a scrivere codice asincrono in modo conciso e leggibile, restituendo implicitamente una promessa così non devi farlo tu.

Se non puoi usare promesse o oggetti osservabili, puoi sempre utilizzare la "modalità callback" definendo il tuo test con la seguente sintassi `test.cb([title], fn)`. I test definiti con questa modalità **dovranno** manualmente terminare con il comando `t.end()`. Questa modalità è principalmente pensata per testare API che utilizzano callback. Ad ogni modo raccomandiamo di usare strumenti come [promisifying](https://github.com/sindresorhus/pify) per trasformare API da callback a Promesse, poichè i test diventano più facile da gestire e mantenere.

I test devono essere definiti in modo sincrono. Non possono essere definiti in istruzioni tipo `setTimeout`, `setInterval`, etc...

Ava prova ad eseguire i file di test impostando la cartella corrente nel percorso che contiene il file `package.json`.

### Come creare test

Per creare un test devi utilizzare la funzione `test` importata da AVA, aggiungi un titolo (opzionale) e una callback. La funzione callback verrà eseguita quando il tuo test verrà eseguito. Come argomento della callback viene fornito un [contesto di esecuzione](#t) come primo ed unico argomento. Per convenzione questo parametro è chiamato `t`.

**Nota**: per utilizzare i [messaggi di asserzione avanzati](#migliorare-messaggi-delle-asserzioni) correttamente, il primo argomento **deve** essere oblligatoriamente `t`.

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

Le soglie per le asserzioni garantiscono che i test passino esclusivamente quando un numero specifico di asserzioni è stato eseguito. Possono aiutare a scoprire casi in cui i test terminano anticipatamente. Inoltre se un test eccede il numero soglia impostato di asserzioni allora viene considerato come fallito: questo è comodo in quei casi in cui si usano asserzioni in callback o cicli.

Se non viene specificato un piano di asserzioni, il test fallirà se non vengono eseguiti test. Configura l'opzione `failWithoutAssertions` a `false` nella [configurazione AVA nel `package.json`](#configurazione) per disabilitare questo comportamento.

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

I test vengono eseguiti in modo concorrente in automatico. A volte però avrai bisogno di scrivere test che non siano eseguiti in modo concorrente. In questi rari casi si può utilizzare il modificatore `.serial`, che obbligherà l'esecuzione dei test seriali *prima* dell'esecuzione dei test concorrenti.

```js
test.serial(t => {
	t.pass();
});
```

Da notare che questo vale per i test all'interno di un particolare file. AVA eseguirà comunque file di test multipli nello stesso istante a meno che venga passato il parametro [`--serial` dalla CLI](#cli).

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
$ ava --match='!*foo*'
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

A volte può succedere che alcuni test falliscano senza una facile soluzione. In questi casi puoi dire ad AVA di ignorare questi test utilizzando il modificatore `.skip`. Verranno comunque mostrati nell'output (come ignorati) ma non verranno eseguiti.


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

### Test markating come `failing`

È possibile usare il modificatore `.failing` per documentare problemi con il tuo codice che necessitano attenzione. Test marcati come `failing` vengono eseguiti come normali test, ma AVA si aspetta che questi falliscano, quindi l'esecuzione non verrà interotta se questi falliscono. Se un test segnato come `failing` dovesse passare l'esecuzione in questo caso verrà interrotta ed un errore verrà stampato, spiegandone il motivo e suggerendo di rimuovere il modificatore `.failing`.

Questa funzionalità permette di fare il merge di test `.failing` prima ancora che il fix venga incluso senza interrompere ambienti di CI. Pu`o anche essere utilizzata questa funzionalità per dimostrare un bug tramite un commit, anche se non si sa come risolvere il problema/bug riportato.

```js
// Vedi: github.com/user/repo/issues/1234
test.failing('demonstrate some bug', t => {
	t.fail(); // Il test conterà come passato con successo
});
```

### Before & after hooks

AVA ti permette di registrare delle funzioni hook da eseguire prima e dopo i tuoi test. Questo ti permette di creare e/o distruggere lo scenario per i tuoi test.

`test.before()` permette di registrare una funzione da eseguire prima del primo test nel tuo file di test. Allo stesso modo `test.after()` permette di eseguire una funzione dopo l'esecuzione dell'ultimo test. Usa l'hook `test.after.always()` per registrare una funzione che verrà **sempre** eseguita una volta che i tuoi test e gli altri hook sono stati completati. Gli hook `.always()` sono eseguiti in tutti i casi, sia di successo o fallimenti durante l'esecuzione, e sono quindi il candidato ideale per ripulire l'ambiente al termine dell'esecuzione. Esistono due eccezioni a questo tipo di comportamento: se si usa l'opzione `--fail-fast` AVA interromperà l'esecuzione dei test al primo fallimento incontrato, senza eseguire alcun hook successivo, quindi neanche `.always()`. Errori non catturati (*Uncaught exceptions*) producono un crash dell'esecuzione dei test, prevenendo l'esecuzione degli hook `.always()`.

`test.beforeEach()` permette di registrare una funzione da eseguire prima di ogni singolo test nel tuo file di test. Allo stesso modo `test.afterEach()` permette di eseguire una funzione dopo l'esecuzione di ciascun test. Usa l'hook `test.afterEach.always()` per registrare un hook da chiamare anche quando altri test o hook falliscono. Gli hook `.always()` sono ideali per ripulire l'ambiente al termine dell'esecuzione.

**Nota**: Se l'opzione `--fail-fast` viene utilizzata, AVA interromperà l'esecuzione immediatamente dopo il primo fallimento e l'hook `.always()` **non** verrà eseguito.

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

test.after.always('guaranteed cleanup', t => {
	// eseguito in ogni caso, anche se un test fallisce
});

test.beforeEach(t => {
    // eseguito prima di ciascun test
});

test.afterEach(t => {
    // eseguito dopo ciascun test
});

test.afterEach.always(t => {
	// eseguito dopo ogni test in ogni caso, che fallisca o passi con successo
});

test(t => {
    // semplice test
});
```

Gli hook possono essere sia sincroni che asincroni, come i test. Per creare un hook asincrono restituisci una promessa o un oggetto osservabile, utilizza una funzione asincrona, o abilita la modalità callback usando `test.cb.before()`, `test.cb.beforeEach()`, etc...


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

Non dimenticare che `beforeEach` e `afterEach` vengono eseguiti appena prima e dopo l'esecuzione di ogni test, e che i test vengono eseguiti in concorrenza. Se hai bisogno di impostare uno stato globale per ogni test (come impostare una *spia* in `console.log` [ad esempio](https://github.com/avajs/ava/issues/560)), dovrai impostare l'esecuzione dei test in [modo seriale](#eseguire-test-in-serie).

Ricorda che AVA esegue ciascun file di test in un processo isolato. Potresti non aver bisogno di ripristinare lo stato originale dello stato nel'hook `after` poichè verrebbe chiamato non appena prima della chiusura del processo.

#### Contesto dei test

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

### Macro per i test

È possibile passare parametri aggiuntivi quando si dichiara un test. Questa funzionalità è utile per creare macro riutilizzabili in più test.

```js
function macro(t, input, expected) {
	t.is(eval(input), expected);
}

test('2 + 2 = 4', macro, '2 + 2', 4);
test('2 * 3 = 6', macro, '2 * 3', 6);
```

È possibile costruire un titolo di un test in modo programmatico assegnando una funzione `title` alla macro:

```js
function macro(t, input, expected) {
	t.is(eval(input), expected);
}

macro.title = (providedTitle, input, expected) => `${providedTitle} ${input} = ${expected}`.trim();

test(macro, '2 + 2', 4);
test(macro, '2 * 3', 6);
test('providedTitle', macro, '3 * 3', 9);
```

Il parametro `providedTitle` ha una stringa vuota come valore predefinito, qualora l'utente non fornisse alcun titolo. Questa funzionalità permette la concatenazione di stringhe senza preoccuparsi di valori `null` o `undefined`. Vale comunque la pena fare attenzione al fatto che una stringa vuota ha un valore "falso" in JavaScript, si può quindi sfruttare questo per scrivere espressioni come `if(providedTitle) {...}`.

È anche possibile passare una lista (Array) di funzioni macro ad un test:

```js
const safeEval = require('safe-eval');

function evalMacro(t, input, expected) {
	t.is(eval(input), expected);
}

function safeEvalMacro(t, input, expected) {
	t.is(safeEval(input), expected);
}

test([evalMacro, safeEvalMacro], '2 + 2', 4);
test([evalMacro, safeEvalMacro], '2 * 3', 6);
```

L'uso delle macro è incoraggiato rispetto alla costruzione di generatori di codice di test ([ecco un esempio](https://github.com/avajs/ava-codemods/blob/47073b5b58aa6f3fb24f98757be5d3f56218d160/test/ok-to-truthy.js#L7-L9) di codice da sostituire con una macro). Le macro sono progettate per effettuare analisi statica del tuo codice, con benefici in termini di performance, migliore integrazione nell'IDE e nelle regole del linter.

### Personalizzare asserzioni

Puoi utilizzare ogni modulo di asserzioni invece o in aggiunta al modulo integrato, a patto che lanci un'eccezione quando un'asserzione fallisce.

L'uso di un modulo esterno non fornisce lo stesso tipo d'esperienza d'uso di [quella integrata](#asserzioni), e allo stesso modo la [soglia per le asserzioni](#soglia-per-le-asserzioni) non funzionerà ([issue #25](https://github.com/avajs/ava/issues/25)).

Dovrai quindi configurare AVA per not far fallire i test qualora non fossero presenti asserzioni, poichè AVA non può predire se un asserzione personalizzata sia passata o meno. Configura l'opzione `failWithoutAssertions` a `false` nella [configurazione di AVA nel `package.json`](#configurazione).

```js
import assert from 'assert';

test(t => {
    assert(true);
});
```

### Supporto a ES2017

AVA fornisce un supporto integrato per ES2017 mediante [Babel 6](https://babeljs.io). Scrivi i tuoi test in ES2017, non c'è bisogno di altra impostazione da aggiungere. Puoi utilizzare qualunque versione di Babel nel tuo progetto. AVA utilizza una versione propria di Babel integrata preimpostata con [`@ava/stage-4`](https://github.com/avajs/babel-preset-stage-4) e [`@ava/transform-test-files`](https://github.com/avajs/babel-preset-transform-test-files).

La configurazione corrispondente in Babel per il setup di AVA è la seguente:

```json
{
  "presets": [
    "@ava/stage-4",
    "@ava/transform-test-files"
  ]
}
```

Puoi personalizzare come AVA utilizzi il transpiler per i file di test mediante l'opzione `babel` nella [configurazione `package.json`](#configurazione) di AVA.
Per sovrascrivere le impostazioni predefinite puoi usare il seguente esempio:

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
	   "babel": "inherit"
   },
}
```

Da notare che AVA applicherà *sempre* i plugin [`espower`] (https://github.com/power-assert-js/babel-plugin-espower) e [`transform-runtime`] (https://babeljs.io/docs/plugins/transform-runtime/).

### Support di TypeScript

AVA include i typings per TypeScript. La configurazione del transpiler dovrà però essere fatta manualmente. Quando imposterai nel file `tsconfig.json` `module` su `commonjs`, TypeScript troverà automaticamente la definizione dei tipi per AVA. Dovrai impostare `target` su `es2015` per poter utilizzare le promesse e le funzioni asincrone.

Per maggiori informazioni vedi la ricetta AVA per [TypeScript](https://github.com/avajs/ava-docs/blob/master/it_IT/docs/recipes/typescript.md).

### Usare il transpiler per i moduli importati

AVA al momento utilizza il transpiler solamente per i test che richiedi di eseguire, e i file helper per i test (in particolare quei file il cui nome inizia per `_` o che si trovano nella cartella `helpers`). *Non verrà utilizzato il transpiler per i moduli importati al di fuori dei test*. Benchè ci sia una ragione valida per questo comportamento, potrebbe non essere quel che ci si aspetta.

Come soluzione alternativa, utilizzando Babel, si può utilizzare il suo [require hook](https://babeljs.io/docs/usage/require/) per i moduli importati sul momento. Per aggiungerlo [configura l'impostazione nel tuo `package.json`](#configurazione).

Puoi anche utilizzare il transpiler per i tuoi moduli in un processo separato e utilizzare i file prodotti invece dei sorgenti nei tuoi test. Per un esempio vedi [qui](https://github.com/avajs/ava-docs/blob/master/it_IT/docs/recipes/precompiling-with-webpack.md).

### Supporto per Promesse

Se restituisci una promessa nel tuo test non hai bisogno di segnalare il termine del test esplicitamente perchè questo terminerà quando la promessa sarà risolta.

```js
test(t => {
	return somePromise().then(result => {
		t.is(result, 'unicorn');
	});
});
```

### Supporto per Generatori

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

### Supporto per Oggetti Osservabili

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

### Timeout globale

È possibile impostare un timeout globale con l'opzione `--timeout`.
Il timeout in AVA ha un funzionamento diverso che negli altri test framework.
AVA reimposta a 0 il timer dopo l'esecuzione di ogni test, forzando i test a terminare se non vengono ricevuti altri risultati di test entro il timeout specificato.

È possibile impostare i timeout con un formato leggibile:

```console
$ ava --timeout=10s # 10 secondi
$ ava --timeout=2m # 2 minuti
# ava --timeout=100 # 100 millisecondi
```

## API

### `test([titolo], implementazione)`
### `test.serial([titolo], implementazione)`
### `test.cb([titolo], implementazione)`
### `test.only([titolo], implementazione)`
### `test.skip([titolo], implementazione)`
### `test.todo(titolo)`
### `test.failing([titolo], implementazione)`
### `test.before([titolo], implementazione)`
### `test.after([titolo], implementazione)`
### `test.beforeEach([titolo], implementazione)`
### `test.afterEach([titolo], implementazione)`

#### titolo

Tipo: `string`

Titolo del test.

#### implementazione(t)

Tipo: `function`

Deve contere il corpo del test.

##### t

Tipo: `oggetto`

Il contesto di esecuzione di uno specifico test. Ogni callback di un test riceve un oggetto distinto. Contiene le asserzioni insieme ai metodi  `.plan(limite)` e `.end()`. `t.context` può contenere lo stato condiviso dall'hook `beforeEach`. `t.title` restituisce il titolo del test.

###### t.plan(limite)

Imposta quante asserzioni ci sono in un test. Il test fallirà se il numero di asserzioni eseguite supera il numero qui impostato. Vedi [soglia per le asserzioni](#soglia-per-le-asserzioni).

###### t.end()

Termina il test. Funziona unicamente con `test.cb()`.

## Asserzioni

Le asserzioni sono contenute nel [contesto di esecuzione](#t) del test:

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

Accerta che il `valore` sia uguale a `previsto`. L'implementazione è basata su [`Object.is()`](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/Object/is).

### .not(valore, previsto, [messaggio])

Accerta che il `valore` non sia uguale a `previsto`. L'implementazione è basata su [`Object.is()`](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/Object/is).

### .deepEqual(valore, previsto, [messaggio])

Accerta che il `valore` sia profondamente uguale a `previsto`. Fare riferimento a [Concordance](https://github.com/concordancejs/concordance) per maggiori dettagli. Funziona anche con [elementi React e `react-test-renderer`](https://github.com/concordancejs/react).

### .notDeepEqual(valore, previsto, [messaggio])

Accerta che il `valore` non sia profondamente uguale a `previsto`. L'opposto di `.deepEqual()`.

### .throws(function|promise, [errore, [message]])

Accerta che `function` lanci un errore o che  la `promise` sia rifiutata.

`errore` può essere un costruttore, messaggio di errore, un'espressione regolare (*regex*) eseguita sul messaggio di errore o una funzione di validazione.

Restituisce l'errore lanciato dalla `function` o dalla ragione del rifiuto della promessa `promise`.

Ad esempio:

```js
const fn = () => {
	throw new TypeError('🦄');
};

test('throws', t => {
	const error = t.throws(() => {
		fn();
	}, TypeError);

	t.is(error.message, '🦄');
});
```

```js
const promise = Promise.reject(new TypeError('🦄'));

test('rejects', async t => {
	const error = await t.throws(promise);
	t.is(error.message, '🦄');
});
```

Quando si esegue un test con una promessa bisogna attendere che l'asserzione sia completata:

```js
test('rejects', async t => {
	await t.throws(promise);
});
```

### .notThrows(function|promise, [messaggio])

Accerta che `function` non lanci un `errore` o che la promessa `promise` non venga rifiutata con un errore.

Come l'asserzione `.throws()`, quando si testa una promessa bisogna attendere che l'asserzione sia completata:

```js
test('rejects', async t => {
	await t.notThrows(promise);
});
```

### .regex(contenuto, regex, [messaggio])

Accerta che `contenuto` corrisponda all'espressione regolare `regex`.

### .notRegex(contenuto, regex, [messaggio])

Accerta che `contenuto` non corrisponda all'espressione regolare `regex`.

### .ifError(errore, [messaggio])

Accerta che `errore` sia falso.

### .snapshot(previsto, [messaggio])
### .snapshot(previsto, [opzioni], [messaggio])

confronta il valore `previsto` con uno snapshot precedentemente registrato. Gli snapshot sono salvati per ogni test, quindi assicurati di assegnare un titolo univoco ad ogni test. In alternativa è possibile passare un oggetto `opzione` per selezionare uno snapshot specifico, ad esempio `{id: 'my snapshot'}`.

## Snapshot testing

AVA offre il supporto per lo snapshot testing, [allo stesso modo di Jest](https://facebook.github.io/jest/docs/snapshot-testing.html), mediante l'interfaccia [Asserzioni](#asserzioni). Puoi fare lo snapshot di ogni valore come di elementi React:

```js
// Il tuo componente
const HelloWorld = () => <h1>Hello World...!</h1>;
export default HelloWorld;
```

```js
// Your test
import test from 'ava';
import render from 'react-test-renderer';
import HelloWorld from '.';

test('HelloWorld component', t => {
	const tree = render.create(<HelloWorld/>).toJSON();
	t.snapshot(tree);
});
```

[Fai una prova con questo progetto esempio.](https://github.com/avajs/ava-snapshot-example)

Gli snapshot vengono salvati insieme ai file di test. Se i tuoi test sono in una cartella `test` o `tests` allora gli snapshots saranno salvati in una cartella `snapshots`. Se i tuoi test sono in una cartella `__tests__` allora gli snapshot saranno salvati in una cartella `__snapshots__`.

Qualora avessi un file `~/project/test/main.js` con dentro asserzioni per snapshot, AVA creerà due file:

* `~/project/test/snapshots/main.js.snap`

* `~/project/test/snapshots/main.js.md`

dove il primo file contiene gli snapshot registrati che verranno usati per confronti futuri. Il secondo file contiene invece il *report degli snapshot*. Quando gli snapshot vengono aggiornati questo secondo file verrà aggiornato. Qualcosa venisse salvato sul sistema di versionamento del codice è possibile confrontare le diverse versioni per vedere le modifiche agli snapshot.

AVA mostrerà il perchè un'asserzione sugli snapshot è fallita come segue:

<img src="https://github.com/avajs/ava/raw/master/media/snapshot-testing.png" width="1048">

Potrai quindi verificarlo sul tuo codice. Se le modifiche sono state intenzionali puoi usare il `flag` `--update-snapshot` (o `-u`) per aggiornare gli snapshot:

```console
$ ava --update-snapshots
```

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

```console
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

### Code coverage

Non è possibile utilizzare [`istanbul`](https://github.com/gotwarlost/istanbul) per il code coverage dato l'[uso di processi isolati](#isolamento-dei-processi) in AVA. In questo caso puoi usare [`nyc`](https://github.com/bcoe/nyc), poichè si tratta sostanzialmente di `istanbul` con il supporto per sub-processi.

Dalla versione `5.0.0` utilizza anche le source maps per la rappresentazione del code coverage per il tuo codice sorgente, a prescindere dai file prodotti dal transpiler. Assicurati che il file che stai testando abbia una source map inline oppure referenzi un file source map. Se utilizzi `babel-register` puoi specificare l'opzione `sourceMaps` come `inline` nella configurazione Babel.

### Errori comuni

Abbiamo raccolto una lista comprensiva di [errori comuni](https://github.com/avajs/ava-docs/blob/master/it_IT/docs/common-pitfalls.md) che puoi incontrare nell'uso di AVA. Se dovessi incontrare qualsiasi problema e pensi che questo sia piuttosto comune, lascia un commento su questa [issue](https://github.com/avajs/ava/issues/404).

## Domande frequenti

### Perchè non usare `mocha`, `tape`, `node-tap`?

Mocha richiede che tu usi funzioni globali come `describe` o `it` con l'interfaccia di default (usata dalla maggior parte delle persone). Non impone una struttura precisa ed esegue test in serie senza alcun isolamento a livello di processo, rendendolo lento.

Tape e tap sono buoni moduli. AVA ha preso ispirazione dalla loro sintassi. Anche loro eseguono test in serie. Il loro output [TAP](https://testanything.org) integrato non è molto pratico da usare e quindi si finisce sempre per usare un reporter TAP esterno.

Viceversa AVA impone una struttura precisa ed esegue i test in modo concorrente, ogni file di test in un processo separato. Il suo reporter integrato è facile da comprendere a prima vista, nonostante ciò AVA supporta anche l'output TAP mediante il parametro da CLI.

### Come posso utilizzare reporter personalizzati?

AVA supporta il formato TAP ed è quindi compatibile con ogni [TAP reporter](https://github.com/sindresorhus/awesome-tap#reporters). Usa il [parametro `--tap`](#produzione-tap-opzionale) per abilitare l'output TAP.

### Come si scrive e pronuncia AVA?

AVA, no Ava o ava. Si pronuncia [`/ˈeɪvə/` ay-və](https://github.com/avajs/ava/blob/master/media/pronunciation.m4a?raw=true).

### Cos'è lo sfondo del logo in cima?

La [galassia di Andromeda](https://it.wikipedia.org/wiki/Galassia_di_Andromeda).

### Qual è la differenza tra concorrenza e parallelismo?

[Concorrenza non è parallelismo, ma lo abilita.](http://stackoverflow.com/q/1050222)

## Ricette

- [Code coverage](https://github.com/avajs/ava-docs/blob/master/it_IT/docs/recipes/code-coverage.md)
- [Watch Mode](https://github.com/avajs/ava-docs/blob/master/it_IT/docs/recipes/watch-mode.md)
- [Test di Endpoint](https://github.com/avajs/ava-docs/blob/master/it_IT/docs/recipes/endpoint-testing.md)
- [Quando usare `t.plan()`](https://github.com/avajs/ava-docs/blob/master/it_IT/docs/recipes/when-to-use-plan.md)
- [Browser testing](https://github.com/avajs/ava-docs/blob/master/it_IT/docs/recipes/browser-testing.md)
- [TypeScript](https://github.com/avajs/ava-docs/blob/master/it_IT/docs/recipes/typescript.md)
- [Configurare Babel](https://github.com/avajs/ava-docs/blob/master/it_IT/docs/recipes/babelrc.md)
- [Testare componenti React](https://github.com/avajs/ava-docs/blob/master/it_IT/docs/recipes/react.md)
- [Testare componenti Vue.js](https://github.com/avajs/ava-docs/blob/master/it_IT/docs/recipes/vue.md)
- [JSPM and SystemJS](https://github.com/avajs/ava-docs/blob/master/it_IT/docs/recipes/jspm-systemjs.md)
- [Fare il debugging di test con Chrome DevTools](https://github.com/avajs/ava-docs/blob/master/it_IT/docs/recipes/debugging-with-chrome-devtools.md)
- [Fare il debugging di test con WebStorm](https://github.com/avajs/ava-docs/blob/master/it_IT/docs/recipes/debugging-with-webstorm.md)
- [Fare il debugging di test con Visual Studio Code](https://github.com/avajs/ava-docs/blob/master/it_IT/docs/recipes/debugging-with-vscode.md)
- [Precompilare file sorgente con webpack](https://github.com/avajs/ava-docs/blob/master/it_IT/docs/recipes/precompiling-with-webpack.md)
- [Test di integrazione con MongoDB in isolamento](https://github.com/avajs/ava-docs/blob/master/it_IT/docs/recipes/isolated-mongodb-integration-tests.md)

## Supporto

- [Stack Overflow](https://stackoverflow.com/questions/tagged/ava)
- [Gitter chat](https://gitter.im/avajs/ava)
- [Twitter](https://twitter.com/ava__js)


## Guarda anche

- [eslint-plugin-ava](https://github.com/avajs/eslint-plugin-ava) - Regole Lint per test AVA
- [sublime-ava](https://github.com/avajs/sublime-ava) - Frammenti di codice per test con AVA
- [atom-ava](https://github.com/avajs/atom-ava) - Frammenti di codice per test con AVA
- [vscode-ava](https://github.com/samverschueren/vscode-ava) - Frammenti di codice per test con AVA
- [gulp-ava](https://github.com/avajs/gulp-ava) - Esegui i test con gulp
- [grunt-ava](https://github.com/avajs/grunt-ava) - Esegui i test con grunt
- [Vedi qui per altri moduli](https://github.com/avajs/awesome-ava#packages)

## Link

- [Compra adesivi di AVA](https://www.stickermule.com/user/1070705604/stickers)
- [Lista fantastica dedicata ad AVA](https://github.com/avajs/awesome-ava)
- [AVA Casts](http://avacasts.com)

## Creato da

[![Sindre Sorhus](https://avatars.githubusercontent.com/u/170270?s=130)](http://sindresorhus.com) | [![Vadim Demedes](https://avatars.githubusercontent.com/u/697676?s=130)](https://github.com/vadimdemedes) | [![James Talmage](https://avatars.githubusercontent.com/u/4082216?s=130)](https://github.com/jamestalmage) | [![Mark Wubben](https://avatars.githubusercontent.com/u/33538?s=130)](https://novemberborn.net) | [![Juan Soto](https://avatars.githubusercontent.com/u/8217766?s=130)](https://juansoto.me) | [![Jeroen Engels](https://avatars.githubusercontent.com/u/3869412?s=130)](https://github.com/jfmengels)
---|---|---|---|---|---
[Sindre Sorhus](http://sindresorhus.com) | [Vadim Demedes](https://github.com/vadimdemedes) | [James Talmage](https://github.com/jamestalmage) | [Mark Wubben](https://novemberborn.net) | [Juan Soto](http://juansoto.me) | [Jeroen Engels](https://github.com/jfmengels)

### Ex Membri

- [Kevin Mårtensson](https://github.com/kevva)


<div align="center">
	<br>
	<br>
	<br>
    <a href="https://ava.li">
	  <img src="https://cdn.rawgit.com/avajs/ava/fe1cea1ca3d2c8518c0cc39ec8be592beab90558/media/logo.svg" width="200" alt="AVA">
    </a>
	<br>
	<br>
</div>
