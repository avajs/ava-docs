# Fare debugging con WebStorm

Traduzioni:
[Français](https://github.com/avajs/ava-docs/blob/master/fr_FR/docs/recipes/debugging-with-webstorm.md),
[Italiano](https://github.com/avajs/ava-docs/blob/master/it_IT/docs/recipes/debugging-with-webstorm.md)

Dalla versione 2016.2, [WebStorm](https://www.jetbrains.com/webstorm/) e altri IDE JetBrains (IntelliJ IDEA Ultimate, PHPStorm, PyCharm Professional, e RubyMine con il plugin Node.js installato) ti permettono di fare debugging dei test AVA.

## Setup

Aggiungi una nuova *Configurazione Run/Debug per Node.js*: seleziona `Modifica Configurazioni...` dal menù a tendina in alto a destra, poi clicca su `+` e seleziona *Node.js*.

Nel campo `Javascript file` scrivi il `path` relativo all'installazione di AVA nella cartella `node_modules`: `node_modules/.bin/ava` per macOS e Linux oppure `node_modules/.bin/ava.cmd` per Windows.

Nel campo `Parametri applicazione` scrivi i `flag` CLI che vuoi usare per i file di test di cui vuoi fare il debug, ad esempio `--verbose test.js`.

Salva la configurazione.

## Debug

Imposta i breakpoints nel codice.

Clicca il bottone verde `Debug` vicino alla lista delle configurazioni in alto a destra. La *finestra di debug* comparirà ed una volta che il breakpoint verrà raggiunto, potrai ispezionare le variabile ed eseguire il codice riga per riga. Quando si fa debugging più file di testing, puoi anche saltare tra processi utilizzando il menù a tendina nel pannello *Frames*.
