___
**Nota del traduttore**

Questa è la traduzione del file [debugging-with-webstorm.md](https://github.com/sindresorhus/ava/blob/master/docs/recipes/debugging-with-webstorm.md). Qui c'è il [link](https://github.com/avajs/ava/compare/c09462c3e515c41da8177a3d9ba5fb0f19759653...master#diff-1fb9cdb432e04d416229256c338f1a06) dove si confrontano le differenze tra commit di riferimento di questa traduzione e l'ultimo commit di AVA sul branch master (Se si clicca sul link, e non si vede il file `debugging-with-webstorm.md` nella lista dei file modificati, questa traduzione è aggiornata).
___
# Fare debugging con WebStorm

Traduzioni:
[Français](https://github.com/avajs/ava-docs/blob/master/fr_FR/docs/recipes/debugging-with-webstorm.md),
[Italiano](https://github.com/avajs/ava-docs/blob/master/it_IT/docs/recipes/debugging-with-webstorm.md)

Dalla versione 2016.2, [WebStorm](https://www.jetbrains.com/webstorm/) e altri IDE JetBrains (IntelliJ IDEA Ultimate, PHPStorm, PyCharm Professional, e RubyMine con il plugin Node.js installato) ti permettono di fare debugging dei test AVA.

## Setup con Node.js

Aggiungi una nuova *Configurazione Run/Debug per Node.js*: seleziona `Modifica Configurazioni...` dal menù a tendina in alto a destra, poi clicca su `+` e seleziona *Node.js*.

Nel campo `Javascript file` scrivi il `path` relativo all'installazione di AVA nella cartella `node_modules`: `node_modules/.bin/ava` per macOS e Linux oppure `node_modules/.bin/ava.cmd` per Windows.

Nel campo `Parametri applicazione` scrivi i `flag` CLI che vuoi usare per i file di test di cui vuoi fare il debug, ad esempio `--verbose test.js`.

Salva la configurazione.

## Setup con npm

Esegui `ava --init` nella cartella del tuo progetto per aggiungere AVA al tuo `package.json`.

Il file `package.json` sarà quindi simile al seguente:

```json
{
	"name": "awesome-package",
	"scripts": {
		"test": "ava"
	},
	"devDependencies": {
		"ava": "^0.20.0"
	}
}
```

Aggiungi una *nuova configurazione di debug per `npm`*: seleziona `Edit Configurations` dal menù a tendina in alto a destra, clicca `+` e seleziona *`npm`*.

Usa i seguenti parametri di configurazione:

- `package.json`: percorso per il file `package.json` del tuo progetto
- `Command`: `test`

Il tuo IDE eseguirà `npm run test` chiamando `node_modules/.bin/ava` e la configurazione da te inserita nel `package.json`.

Non dimenticare di selezionare l'interprete Node.js.

Salva la configurazione.

## Debug

Imposta i breakpoints nel codice.

Clicca il bottone verde `Debug` vicino alla lista delle configurazioni in alto a destra. La *finestra di debug* comparirà ed una volta che il breakpoint verrà raggiunto, potrai ispezionare le variabile ed eseguire il codice riga per riga. Quando si fa debugging più file di testing, puoi anche saltare tra processi utilizzando il menù a tendina nel pannello *Frames*.
