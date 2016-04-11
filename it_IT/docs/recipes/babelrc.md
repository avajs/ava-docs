___
**Nota del traduttore**

Questa è la traduzione del file [babelrc.md](https://github.com/sindresorhus/ava/blob/master/docs/recipes/babelrc.md). Qui c'è il [link](https://github.com/sindresorhus/ava/compare/3201b1b4ff80ff75f0e1c288ca7da22f92c9b814...master#diff-0730bb7c2e8f9ea2438b52e419dd86c9) dove si confrontano le differenze tra commit di riferimento di questa traduzione e l'ultimo commit di AVA sul branch master (Se si clicca sul link, e non si vede il file `babelrc.md` nella lista dei file modificati, questa traduzione è aggiornata).
___
# Configurare Babel

Traduzioni: [Français](https://github.com/sindresorhus/ava-docs/blob/master/fr_FR/docs/recipes/babelrc.md)

Ci sono più opzioni per configurare il mdoo in cui AVA transpila i tuoi test con Babel.

 - [Impostazione predefinita di AVA per la transpilazione](#impostazione-predefinita-per-la-transpilazione-in-ava)
 - [Personalizzare come AVA transpila i tuoi test](personalizzare-come-ava-transpila-i-tuoi-test)
 - [Transpilare file sorgente](#transpilare-file-sorgente)
 - [Transpilare i file di test e sorgente allo stesso modo](#transpilare-file-di-test-e-sorgenti-allo-stesso-modo)
 - [Estendere la configurazione per la transpilazione dei sorgenti](#extendere-la-configurazione-per-la-traspilazione-dei-sorgenti)
 - [Estendere un file di configurazione alternativo (es. non `.babelrc`)](#estendere-un-file-di-configurazione-alternativo)
 - [Note](#note)

## Impostazione predefinita per la transpilazione in AVA

AVA transpila i tuoi file di test (e sono questi) utilizzando le impostazioni Babel  [`es2015`](http://babeljs.io/docs/plugins/preset-es2015/) e [`stage-2`](http://babeljs.io/docs/plugins/preset-stage-2/). Questa è l'impostazione migliore per piccoli moduli in cui tu non voglia transpilare i tuoi sorgenti prima di pubblicare su `npm`.

## Personalizzare come AVA transpila i tuoi test

Puoi sovrascrivere la configurazione Babel predefinita in AVA da usare per transpilare i test nel `package.json`. Ad esempio, la configurazione seguente aggiunge il plugin Babel `rewire` ed utilizza l'impostazione [`stage-3`](http://babeljs.io/docs/plugins/preset-stage-3/)(che è un sottoinsieme di [`stage-2`](http://babeljs.io/docs/plugins/preset-stage-2/)).

```json
{
  "ava": {
    "babel": {
      "plugins": ["rewire"],
      "presets": ["es2015", "stage-3"]
    }
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
    "presets": ["es2015"]
  }
}
```

Considera che caricare `babel-register` in ogni processo ha un alto costo in termini di performance. Se hai molti file di test, puoi considerare di transpilare i tuoi file sorgente *prima* di fare eseguire i tuoi test. Questo non è però ideale, poichè potrebbe complicare l'utilizzo del watch mode in AVA, quindi suggeriamo il caricamento di `babel-register` fino a che il costo prestazionale sia sopportabile. Come impostare un passo di pre-compilazione non è lo scopo di questa guida, ma possiamo consigliare di dare un'occhiata ad uno qualsiasi dei vari [build systems che supportano Babel](http://babeljs.io/docs/setup/). È al momento presente una [issue](https://github.com/sindresorhus/ava/issues/577) dove si sta discutendo come migliorare questo aspetto in AVA.

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
    "presets": ["es2015", "react"]
  }
}
```

Nell'esempio sopra, sia i file di test che sorgente saranno transpilati utilizzando le impostazioni [`es2015`](http://babeljs.io/docs/plugins/preset-es2015/) e [`react`](http://babeljs.io/docs/plugins/preset-react/).

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
    "presets": ["es2015", "react"]
  }
}
```

Nell'esempio sopra, *i file sorgente* sono compilati utilizzando le impostazioni [`es2015`](http://babeljs.io/docs/plugins/preset-es2015/) e [`react`](http://babeljs.io/docs/plugins/preset-react/), *i file di test* utilizzano i suddetti plugin, con l'aggiunta de plugin `custom` specificato.

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

## Note

AVA aggiunge *sempre* qualche plugin Babel personalizzati quando transpila i tuoi plugin. Questi plugin personalizzati servono per diverse funzioni:

* Abilitare il supporto per i `power-assert`
* Riscrivere i path per i require delle dipendenze interne di AVA, tipo `babel-runtime` (importante se stai ancora usando `npm@2`)
* [`ava-throws-helper`](https://github.com/jamestalmage/babel-plugin-ava-throws-helper) permette ad AVA di [individuare e notificare](https://github.com/sindresorhus/ava/pull/742) usi impropri di asserzioni `t.throws`.
* Generare metadati sui test per determinare quali file devono essere eseguiti prima di altri (*in futuro*)
* Analisi statica delle dipendenza per la precompilazione (*in futuro*).
