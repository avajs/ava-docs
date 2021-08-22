___
**Nota del traduttore**

Questa è la traduzione del file [debugging-with-vscode.md](https://github.com/sindresorhus/ava/blob/master/docs/recipes/debugging-with-vscode.md). Qui c'è il [link](https://github.com/avajs/ava/compare/c09462c3e515c41da8177a3d9ba5fb0f19759653...main#diff-a3927068f3a0ffbbdf1b02fbd401b146) dove si confrontano le differenze tra commit di riferimento di questa traduzione e l'ultimo commit di AVA sul branch master (Se si clicca sul link, e non si vede il file `debugging-with-vscode.md` nella lista dei file modificati, questa traduzione è aggiornata).
___
# Fare debugging dei test con Visual Studio Code

Translations: [Français](https://github.com/avajs/ava-docs/blob/main/fr_FR/docs/recipes/debugging-with-vscode.md)
[Italiano](https://github.com/avajs/ava-docs/blob/main/it_IT/docs/recipes/debugging-with-vscode.md)

## Setup

Sulla barra a lato di VSCode clicca su `Debug`.

Aggiungi una nuova configurazione cliccando sul campo a fianco del tasto verde `Debug`, poi clicca su `Add configuration`. Si aprirà un file `launch.json` con tutte le impostazioni per il debug.

Aggiungi il seguente all'oggetto `configurations`:

```json
{
	"type": "node",
	"request": "launch",
	"name": "Avvia AVA test",
	"program": "${workspaceRoot}/node_modules/ava/profile.js",
	"args": [
	  "${file}"
	]
}
```

Dopo aver aggiunto la configurazione salva il file.

## Debug

> **Nota:** Il file su cui vuoi fare il debug deve essere aperto e attivo in VSCode

> **Nota:** I breakpoint in VSCode possono non funzionare correttamente a volte (in particolare quando si tratta di codice async). L'istruzione `debugger;` funziona sempre correttamente.

Aggiungi il breakpoint nel codice **o** aggiungere l'istruzione `debugger;` nel punto in cui si vuole fermare l'esecuzione.

Clicca il tasto verde `Debug` accanto alla lista delle configurazioni, in alto a sinistra del pannello `Debug` in VSCode. Una volta che l'esecuzione si fermerà al punto in cui si è impostato il breakpoint sarà possibile guardare i valori delle variabili ed eseguire il codice riga per riga.