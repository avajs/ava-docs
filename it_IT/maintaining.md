___
**Nota del traduttore**

Questa è la traduzione del file [readme.md](https://github.com/sindresorhus/ava/blob/master/readme.md). Qui c'è il [link](https://github.com/sindresorhus/ava/compare/f2c070987ecee3caf7613190acf2c8a90700e058...master#diff-f2c070987ecee3caf7613190acf2c8a90700e058) dove si confrontano le differenze tra commit di riferimento di questa traduzione e l'ultimo commit di AVA sul branch master (Se si clicca sul link, e non si vede il file `readme.md` nella lista dei file modificati, questa traduzione è aggiornata).
___
# Manutenzione [![Dependency Status](https://david-dm.org/sindresorhus/ava.svg)](https://david-dm.org/sindresorhus/ava) [![devDependency Status](https://david-dm.org/sindresorhus/ava/dev-status.svg)](https://david-dm.org/sindresorhus/ava#info=devDependencies)

## Condotta

**Sii gentie con tutti.**
Leggi ed aderisci al [codice di condotta](code-of-conduct.md).


## Testing

 - `npm test`: Esegue una verifica Lint sul codice ed esegue l'intera suite di test con code coverage
 - `npm run test-win`: Esegue i test in Windows.
 - `npm run coverage`: Genera un report di code coverage per l'ultima esecuzione (apre una finestra nel browser).
 - `tap test/fork.js --bail`:  Esegue uno specifico file di test ed esce non appena si verifica il primo errore (utile quando si cercano bug).


## Processo di rilascio

- Verifica le dipendenze.
- Assicurati che [Travis CI](https://travis-ci.org/sindresorhus/ava) e [AppVeyor](https://ci.appveyor.com/project/sindresorhus/ava/branch/master) siano verdi.
- Pubblica una nuova versione utilizzando [`np`](https://github.com/sindresorhus/np) con un numero di versione secondo le regole [semver](http://semver.org).
- Scrivi una [release note](https://github.com/sindresorhus/ava/releases/new) seguendo lo stile delle precedenti release note.


## Pull requests

- Una nuova funzionalità deve avere test e documentazione a supporto.
- Assicurati che le [linee guida per collaborare](contributing.md) siano rispettate.
- Almeno uno dei membri del team deve verificare (`LGTM` - "Looks good to me") una pull request prima di accettarla
- Fai lo squash dei commit quando si effettua un merge *[Esempio](https://github.com/sindresorhus/ava/commit/0675d3444da6958b54c7e5eada91034e516bc97c)*

## Analisi prestazioni (Profiling)

Come prima cosa devi istallare
 [`iron-node`](https://github.com/s-a/iron-node) e/o [`devtool`](https://github.com/Jam3/devtool) globalmente:

```
$ npm install --global iron-node devtool
```

Nella cartella principale del progetto dove si usa AVA eseguire il seguente comando:

```
$ iron-node node_modules/ava/profile.js <test-file>
```

O:

```
$ devtool node_modules/ava/profile.js <test-file>
```

Una volta che la finestra dei Dev Tools è pronta, attiva l'analisi (profiling) della memoria o CPU, poi premi <kbd>Cmd</kbd><kbd>R</kbd> per rieseguire i test.

Non appena i test sono terminati, termina la registrazione e ispeziona i risultati del profiler. Il flamegraph può essere visualizzato scegliendo `Chart` dal menù a tendina nella sezione (tab) `Profili` (altre opzioni sono `Tree (top down)` o `Heavy (bottom up)`).

Potresti anche voler controllare la pagina Impostazioni nei Dev Tools ed abilitare una o più opzioni nella sezione Profiling.

##### Risorse utili

 - [Un'introduzione al debug per Node.js con i `devtool`](http://mattdesl.svbtle.com/debugging-nodejs-in-chrome-devtools).
 - [Un video di introduzione al profiling della memoria e CPU con i Chrome DevTools](https://www.youtube.com/watch?v=KKwmdTByxLk).

 ## Valutazione comparativa (Benchmarking)

Prima di tutto otteniamo i dati per uno specifico branch o commit:

```
$ node bench/run
```

Una volta ottenuti i dati per due o tre diversi branch o commit, esegui:

```
$ node bench/compare
```

*Per esempio potresti ottenere i dati dal working tree fino all'ultimo commit*

![](https://cloud.githubusercontent.com/assets/4082216/12700805/bf18f730-c7bf-11e5-8a4f-fec0993c053f.png)

Ora puoi eseguire un sottoinsieme della suite:

```
$ node bench/run.js concurrent/sync.js serial/sync.js -- concurrent/sync.js -- serial/sync.js
```

Nota il separatore '--'. Il comando sopra è l'equivalente dei tre comandi seguenti combinati:

```
$ ava concurrent/sync.js serial/sync.js
$ ava concurrent/sync.js
$ ava serial/sync.js
```

Inoltre se vuoi fare il benchmark di una suite che dovrebbe fallire, devi aggiungere `--should-fail` per il gruppo scelto:

```
$ node bench/run.js concurrent/sync.js -- --should-fail other/failures.js
```

Il comando sopra equivale a due comandi di benchmark, dove il secondo gruppo di test ci si aspetta fallisca.

## Introduzione di nuovi membri

- Aggiungi l'utente al file 'readme.md' e 'package.json'.
- Aggiungi l'utente come collaboratore a tutti i repository AVA e i packages su npm.
- Condividi i dati di login dell'account Twitter in modo che l'utente sia libero di twittare o ritwittare cose rilevanti.
