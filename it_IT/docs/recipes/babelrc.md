___
**Nota del traduttore**

Questa è la traduzione del file [babelrc.md](https://github.com/avajs/ava/blob/main/docs/recipes/babelrc.md). Qui c'è il [link](https://github.com/avajs/ava/compare/c09462c3e515c41da8177a3d9ba5fb0f19759653...master#diff-0730bb7c2e8f9ea2438b52e419dd86c9) dove si confrontano le differenze tra commit di riferimento di questa traduzione e l'ultimo commit di AVA sul branch master (Se si clicca sul link, e non si vede il file `babelrc.md` nella lista dei file modificati, questa traduzione è aggiornata).
___
# Configurare Babel

Traduzioni: [Français](https://github.com/avajs/ava-docs/blob/master/fr_FR/docs/recipes/babelrc.md)

Ci sono più opzioni per configurare il mdoo in cui AVA transpila i tuoi test con Babel.

 - [Impostazione predefinita di AVA per la transpilazione](#impostazione-predefinita-per-la-transpilazione-in-ava)
 - [Personalizzare come AVA transpila i tuoi test](personalizzare-come-ava-transpila-i-tuoi-test)
 - [Transpilare file sorgente](#transpilare-file-sorgente)
 - [Transpilare i file di test e sorgente allo stesso modo](#transpilare-file-di-test-e-sorgenti-allo-stesso-modo)
 - [Estendere la configurazione per la transpilazione dei sorgenti](#extendere-la-configurazione-per-la-traspilazione-dei-sorgenti)
 - [Estendere un file di configurazione alternativo (es. non `.babelrc`)](#estendere-un-file-di-configurazione-alternativo)
 - [Note](#note)

## Impostazione predefinita per la transpilazione in AVA

AVA ti permette di usare nuove funzionalità JavaScript, come le [async functions](https://github.com/avajs/ava#async-function-support). Per usare queste nuove funzioni su vecchie versioni di Node.js AVA transpila i test ed i file helper con l'impostazione Babel [`@ava/stage-4`](https://github.com/avajs/babel-preset-stage-4). Questo è molto utile per progetti dove non si utilizza Babel per i file sorgenti, ma si vogliono usare le nuove funzionalità JavaScript per i test.

## Personalizzare come AVA transpila i tuoi test

Puoi sovrascrivere la configurazione Babel predefinita in AVA da usare per transpilare i test nel `package.json`. Ad esempio, la configurazione seguente aggiunge il plugin Babel `rewire` ed utilizza l'impostazione [`stage-3`](http://babeljs.io/docs/plugins/preset-stage-3/)(che è un sottoinsieme di [`stage-2`](http://babeljs.io/docs/plugins/preset-stage-2/)).

Puoi sovrascrivere la configurazione predefinita di Babel che AVA utilizza per la traspilazione dei test nel `package.json`. Per esempio, la configurazione sotto aggiunge il plugin Babel `rewire`, e aggiunge l'impostazione [`stage-3`](http://babeljs.io/docs/plugins/preset-stage-3/) a Babel.

You can override the default Babel configuration AVA uses for test transpilation in `package.json`. For example, the configuration below adds the Babel `rewire` plugin, and adds the Babel [`stage-3`](http://babeljs.io/docs/plugins/preset-stage-3/) preset.

```json
{
    "ava": {
        "babel": {
            "plugins": ["rewire"],
            "presets": ["@ava/stage-4", "stage-3"]
        }
    }
}
```

## Usare Polyfills per Babel

AVA ti permette di scrivere i test con la nuova sintassi JavaScript, anche su versioni vecchie di Node.js che non lo supportano nativamente. Questo non aggiunge o modifica il tuo ambiente corrente. Per esempio usando AVA non si aggiungono funzionalità come `Array.prototype.includes()` ad un ambiente Node.js 4.
Caricando il  [modulo Babel Polyfill](https://babeljs.io/docs/usage/polyfill/) puoi aggiungere queste funzionalità.
Considera che questo modulo modificherà l'ambiente che stai utilizzando, cambiando potenzialmente il comportamento del tuo software. Puoi abilitare `babel-polyfill` aggiungendolo all'opzione `require` di AVA:
```json
{
	"ava": {
		"require": [
			"babel-polyfill"
		]
	}
}
```

## Transpilare i file sorgente

Per transpilare i tuoi file sorgente, avrai bisogno di definire una [`configurazione babel`](http://babeljs.io/docs/usage/babelrc/) nel file `package.json` o un file `.babelrc`. Inoltre, dovrai esplicitamente dichiarare ad AVA di caricare [`babel-register`](http://babeljs.io/docs/usage/require/) in ogni processo, aggiungendolo nella sezione `require` del tuo file di configurazione AVA:

`package.json`

```json
{
  "ava": {
    "require": ["babel-register"]
  },
  "babel": {
    "presets": ["@ava/stage-4"]
  }
}
```

Considera che caricare `babel-register` in ogni processo ha un alto costo in termini di performance. Se hai molti file di test, puoi considerare di transpilare i tuoi file sorgente *prima* di fare eseguire i tuoi test. Questo non è però ideale, poichè potrebbe complicare l'utilizzo del watch mode in AVA, quindi suggeriamo il caricamento di `babel-register` fino a che il costo prestazionale sia sopportabile. Come impostare un passo di pre-compilazione non è lo scopo di questa guida, ma possiamo consigliare di dare un'occhiata ad uno qualsiasi dei vari [build systems che supportano Babel](http://babeljs.io/docs/setup/). È al momento presente una [issue](https://github.com/avajs/ava/issues/577) dove si sta discutendo come migliorare questo aspetto in AVA.

## Transpilare i file di test e sorgente allo stesso modo

Utilizzando l'impostazione `"inherit"` i tuoi test verranno transpilati allo stesso modo dei tuoi file sorgente (come configurato nel tuo file [`babelrc`](http://babeljs.io/docs/usage/babelrc/)). AVA aggiungere qualche [plugin interno](#note) in aggiunta quando verrano transpilati i file di test, senza procurare alcun cambio di comportamento ai tuoi file di test.

`package.json`:

```json
{
  "ava": {
    "require": "babel-register",
    "babel": "inherit"
  },
  "babel": {
    "presets": ["@ava/stage-4", "react"]
  }
}
```

Nell'esempio sopra, sia i file di test che sorgente saranno transpilati utilizzando le impostazioni [`@ava/stage-4`](https://github.com/avajs/babel-preset-stage-4) e [`react`](http://babeljs.io/docs/plugins/preset-react/).

AVA controllerà solamente l'esistenza del file `.babelrc` nella stessa cartella del file `package.json`. Se questo file non venisse trovato allora si assume che la configurazione Babel è inclusa nel file `package.json`.

## Estendere la configurazione per la transpilazione dei sorgenti

Quando configuri le impostazioni Babel per i tuoi test, puoi impostare l'opzione `babelrc` su `true`. Questo ti permetterà di unire i plugin selezionati con quelli configurati nel tuo file [`babelrc`](http://babeljs.io/docs/usage/babelrc/).

`package.json`:

```json
{
  "ava": {
    "require": "babel-register",
    "babel": {
      "babelrc": true,
      "plugins": ["custom-plugin-name"],
      "presets": ["custom-preset"]
    }
  },
  "babel": {
    "presets": ["@ava/stage-4", "react"]
  }
}
```

Nell'esempio sopra, *i file sorgente* sono compilati utilizzando le impostazioni [`@ava/stage-4`](https://github.com/avajs/babel-preset-stage-4) e [`react`](http://babeljs.io/docs/plugins/preset-react/), *i file di test* utilizzano i suddetti plugin, con l'aggiunta de plugin `custom` specificato.

AVA controllerà solamente l'esistenza del file `.babelrc` nella stessa cartella del file `package.json`. Se questo file non venisse trovato allora si assume che la configurazione Babel è inclusa nel file `package.json`.

## Estendere un file di configurazione alternativo

Se, per qualche motivo, la tua configurazione Babel non si trova in una dei file predefiniti ([`.babelrc` o `package.json`](http://babeljs.io/docs/usage/babelrc/), puoi impostare nell'opzione `extends` la configurazione alternativa che intendi utilizzare per i tuoi test.

`package.json`:

```json
{
  "ava": {
    "require": "babel-register",
    "babel": {
      "extends": "./babel-test-config.json",
      "plugins": ["custom-plugin-name"],
      "presets": ["custom-preset"]
    }
  }
}
```

L'esempio sopra utilizza `babel-test-config.json` come configurazione per il transpiling dei *file sorgente* e come configurazione di base per i *file di test*. Per i *file di test* la configurazione verrà comunque estesa con i plugin e le impostazioni specificati.