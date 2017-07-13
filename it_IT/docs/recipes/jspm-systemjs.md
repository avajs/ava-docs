___
**Nota del traduttore**

Questa è la traduzione del file [jspm-systemjs.md](https://github.com/sindresorhus/ava/blob/master/docs/recipes/jspm-systemjs.md). Qui c'è il [link](https://github.com/avajs/ava/compare/c09462c3e515c41da8177a3d9ba5fb0f19759653...master#diff-e1128fe3361ff27e14e2ebec6c6ff362) dove si confrontano le differenze tra commit di riferimento di questa traduzione e l'ultimo commit di AVA sul branch master (Se si clicca sul link, e non si vede il file `jspm-systemjs.md` nella lista dei file modificati, questa traduzione è aggiornata).
___
# JSPM e SystemJS per ES2015

Traduzioni: [Français](https://github.com/avajs/ava-docs/blob/master/fr_FR/docs/recipes/jspm-systemjs.md), [Italiano](https://github.com/avajs/ava-docs/blob/master/it_IT/docs/recipes/jspm-systemjs.md)

Per usare AVA con JSPM si richiede l'utilizzo di un loader speciale per far funzionare correttamente l'istruzione `import` dei pacchetti JSPM. Lo scopo del loader è quello di permettere di eseguire i test senza pre-compilare il tuo progetto JSPM.

## Setup

Questa ricetta è stata testata unicamente con JSPM v.0.17.0-beta.22 ma dovrebbe funzionare con ogni versione di JSPM v0.17 e forse anche con le versioni v0.16.

### Babel

Configura il file `.babelrc` per eseguire AVA se non l'hai ancora fatto. Puoi avere configurazioni aggiuntive nei tui file di configurazione JSPM per sovrascrivere queste impostazioni durante la fase di assemblamento (bundling) e compilazione.

```json
{
	"presets": ["es2015", "stage-2"]
}
```

Puoi trovare maggiori informazioni su come configurare Babel con AVA nella ricetta [babelrc](https://github.com/avajs/ava-docs/blob/master/it_IT/docs/recipes/babelrc.md).

### Helper JSPM Loader

Come prima cosa installa il [loader JSPM per AVA](https://github.com/skorlir/ava-jspm-loader) come una dipendenza dev.

```
$ npm install --save-dev ava-jspm-loader
```

Aggiorna anche la tua configurazione AVA nel `package.json` per usare il loader JSPM.

```json
{
	"ava": {
		"require": [
			"babel-register",
			"ava-jspm-loader"
		]
	}
}
```

NOTA: se usi `async`/`await` nel tuo codice sorgente (non nei tuoi file di test!) avrai bisogno di installare `babel-polyfill` da `npm` e aggiungerli al tuo Array di `require`.

### File test di esempio

Per tutti i tuoi file nel progetto avrai bisogno di utilizzare `System.import`. Quindi se hai chiamato il tuo progetto `app` e vuoi importare il tuo file `main.js` in un file di test dovrai importare il file in queto modo `import main from 'app/main'`.

```js
import test from 'ava';
import main from 'app/main';  // Maps to your JSPM config for "app/main.js"
import BigNumber from 'bignumber.js';  // In jspm_packages

function fn() {
	return Promise.resolve(new BigNumber('1234567890.123456789'));
}

test('example test', async t => {
	t.is((await fn()).toString(), '1234567890.123456789');
});
```