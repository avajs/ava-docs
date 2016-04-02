___
**Nota del traduttore**

Questa è la traduzione del file [watch-mode.md](https://github.com/sindresorhus/ava/blob/master/docs/recipes/watch-mode.md). Qui c'è il [link](https://github.com/sindresorhus/ava/compare/f2c070987ecee3caf7613190acf2c8a90700e058...master#diff-0730bb7c2e8f9ea2438b52e419dd86c9) dove si confrontano le differenze tra commit di riferimento di questa traduzione e l'ultimo commit di AVA sul branch master (Se si clicca sul link, e non si vede il file `watch-mode.md` nella lista dei file modificati, questa traduzione è aggiornata).
___
# Watch mode

Traduzioni: [Français](https://github.com/sindresorhus/ava-docs/blob/master/fr_FR/docs/recipes/watch-mode.md), [Русский](https://github.com/sindresorhus/ava-docs/blob/master/ru_RU/docs/recipes/watch-mode.md)

AVA integra un sistema intelligente di watch. Supervisiona le modifiche ai file ed esegue solamente i test che ne sono affetti.

## Esegui i test con la modalità watch

Puoi abilitare la modalità watch usando il parametro `--watch` o `-w`. Quindi se hai AVA installato globalmente:

```console
$ ava --watch
```

Se hai configurato AVA nel tuo `package.json` così:

```json
{
  "scripts": {
    "test": "ava"
  }
}
```

Puoi eseguire:

```console
$ npm test -- --watch
```

Puoi anche impostare uno specifico script:

```json
{
  "scripts": {
    "test": "ava",
    "test:watch": "ava --watch"
  }
}
```

E poi esegui:

```console
$ npm run test:watch
```

## Requisiti

AVA utilizza [`chokidar`] come watcher per i file. È configurato come dipendenza opzionale perchè in taluni scenari `chokidar` non può essere istallato. La modalità watch non è disponibile se `chokidar` non viene/può essere installato, verrà quiandi mostrato il seguente messaggio:

> The optional dependency chokidar failed to install and is required for --watch. Chokidar is likely not supported on your platform.
> La dipendenza opzionale chokidar ha fallito l'istallazione ed è stato richiesto da --watch. Probabilmente Chokidar non è ancora supportato dalla tua piattaforma.

Fai riferimento alla [documentazione di `chokidar`][`chokidar`] per trovare soluzioni a questo problema.

## File sorgente e di test

AVA distingue tra i *file sorgente* e i *file di test*. Come puoi immaginare i *file di test* contengono i tuoi test. I *file sorgente* sono tutti quei file che sono richiesti per eseguire i test, che siano file sorgente o file fixtures.

AVA controlla automaticamente le modifiche nei file di test, `package.json`, ed ogni file `.js`. Ignorerà invece file in [specifiche cartelle]
(https://github.com/novemberborn/ignore-by-default/blob/master/index.js) come predefinito nel modulo [`ignore-by-default`].

Puoi configurare il pattern per i file sorgente utilizzando il [parametro CLI `--source`] oppure nella sezione `ava` nel file `package.json`. Nota che se specifichi un pattern negativo le cartelle normalmente ignorate da [`ignore-by-default`] non saranno più ignorate, quindi vorrai aggiungere anche queste nella tua configurazione.

Se i tuoi test devono scrivere su disco potrebbero entrare in conflitto con il watcher, che farà ri-eseguire i tuoi test. Se questo avvenisse dovrai usare il parametro `--source`.

## Tracciare le dipendenza

AVA traccia quale da file sorgente ogni tuo test dipende. Se cambi questa dipendenza solamente il file di test che dipende verrà rieseguito. AVA ri-eseguirà tutti i test se non può determinare quale file di test dipende dal file sorgente modificato.

Il tracciamento delle dipendenze funziona per i moduli richiesti. Estensioni personalizzate e transpiler sono supportati, a patto che vengano caricati utilizzando il [parametro CLI `--require`] e non dall'interno dei tuoi file di test. I file caricati tramite il modulo `fs` non verranno tracciati.

## Riesecuzione manuale dei test

Puoi rapidamente rieseguire tutti i testi digitando <kbd>r</kbd> sulla linea di comando.

## Debugging

Qualche volta la modalità watch può comportarsi stranamente rieseguendo tutti i test quando invece pensavi che un unico test sarebbe stato eseguito. Per capirne il motivo puoi abilitare la modalità debug:

```console
$ DEBUG=ava:watcher npm test -- --watch
```

Su Windows scrivi:

```console
$ set DEBUG=ava:watcher
$ npm test -- --watch
```

## Aiutaci a migliorare la modalità watch

La modalità watch è una funzionalità relativamente nuova e ci potrebbero essere ancora alcuni difetti. Per favore [notifica](https://github.com/sindresorhus/ava/issues) qualsiasi problema che riscontri. Grazie!

[`chokidar`]: https://github.com/paulmillr/chokidar
[`ignore-by-default`]: https://github.com/novemberborn/ignore-by-default
[parametro CLI `--require`]: https://github.com/sindresorhus/ava-docs/blob/master/it_IT/readme.md#cli
[parametro CLI `--source`]: https://github.com/sindresorhus/ava-docs/blob/master/it_IT/readme.md#cli
