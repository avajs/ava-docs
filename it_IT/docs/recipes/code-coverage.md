___
**Nota del traduttore**

Questa è la traduzione del file [code-coverage.md](https://github.com/sindresorhus/ava/blob/master/docs/recipes/code-coverage.md). Qui c'è il [link](https://github.com/avajs/ava/compare/c09462c3e515c41da8177a3d9ba5fb0f19759653...main#diff-d3030a18b089fdb1fbfabf6e75e4aef0) dove si confrontano le differenze tra commit di riferimento di questa traduzione e l'ultimo commit di AVA sul branch master (Se si clicca sul link, e non si vede il file `code-coverage.md` nella lista dei file modificati, questa traduzione è aggiornata).
___
# Code coverage

Traduzioni: [Español](https://github.com/avajs/ava-docs/blob/main/es_ES/docs/recipes/code-coverage.md), [Français](https://github.com/avajs/ava-docs/blob/main/fr_FR/docs/recipes/code-coverage.md), [日本語](https://github.com/avajs/ava-docs/blob/main/ja_JP/docs/recipes/code-coverage.md), [Portugués](https://github.com/avajs/ava-docs/blob/main/pt_BR/docs/recipes/code-coverage.md)

Poichè AVA [usa processi isolati](processi-isolati), non si può usare ['istanbul'] per la code coverage; invece, si può usare ['nyc'] che è in pratica ['istanbul'] con il supporto per i sub-processi.

## Configurazione

Installare NYC come prima cosa:

```
$ npm install nyc --save-dev
```

Poi aggiungere le cartelle `.nyc_output` e `coverage` nel tuo file `.gitignore`.

`.gitignore`:

```
node_modules
coverage
.nyc_output
```

## Coverage codice in ES5

Utilizzare NYC per avere la code coverage per il codice in produzione scritto con ES5 è facile. Aggiungi `nyc` davanti al tuo test script:

```json
{
	"scripts": {
		"test": "nyc ava"
	}
}
```

Tutto qua!

Se vuoi creare report HTML per il code coverage, o caricare i risultati su Coveralls, vai alla relativa sezione alla fine di questo file.

## Code coverage ES2015

Usando Babel per transpilare il tuoi codice in produzione è un po' più complicato. Abbiamo cercato di spiegarlo qui con diversi passaggi.

### Configurare Babel

Prima cosa, abbiamo bisogno di una configurazione Babel. La seguente è solo un esempio. Puoi modificarla per i tuoi bisogni.

`package.json`:
```json
{
	"babel": {
		"presets": ["es2015"],
		"plugins": ["transform-runtime"],
		"ignore": "test.js",
		"env": {
			"development": {
				"sourceMaps": "inline"
			}
		}
	}
}
```

Ci sono due cose importanti da considerare dall'esempio qui sopra.

  1. Abbiamo ignorato i file di test perché AVA già si occupa di transpilarli per te.

	2. Abbiamo specificato delle source maps `inline` per lo sviluppo. Questo è importante per la generazione del code coverage. Utilizzando la sezione `env` nella configurazione di Babel possiamo disabilitare le source maps per il codice che andrà in produzione.

### Creare un build script

Poiché sembra improbabile che tu voglia le source map `inline` nel codice in produzione, dovrai specificare una variabile alternativa di ambiente nel tuo build script:

`package.json`

```json
{
  "scripts": {
    "build": "BABEL_ENV=production babel --out-dir=dist index.js"
  }
}
```

> Attenzione: `BABEL_ENV=production` non funziona su Windows, ma devi usare la keyword `set` (`set BABEL_ENV=production`). Per build script multi-piattaforma dai un'occhiata a [`cross-env`].

Nota che il build script non ha molto in comune con AVA, questa è solo una dimostrazione di come utilizzare `env` nella configurazione di Babel per manipolare le tue impostazioni in modo da essere compatibili con AVA.

### Utilizzare l'hook `require` di Babel

Per utilizzare l'hook `require` di Babel, aggiungi `babel-core/register` nella sezione `require` della tua configurazione AVA nel file `package.json`.

```json
{
	"ava": {
		"require": ["babel-core/register"]
	}
}
```


### Combinando tutto

Combinando tutti i passaggi precedenti, il tuo `package.json` completo dovrebbe assomigliare al seguente:

```json
{
	"scripts": {
		"test": "nyc ava",
		"build": "BABEL_ENV=production babel --out-dir=dist index.js"
	},
	"babel": {
		"presets": ["es2015"],
		"plugins": ["transform-runtime"],
		"ignore": "test.js",
		"env": {
			"development": {
				"sourceMaps": "inline"
			}
		}
	},
	"ava": {
		"require": ["babel-core/register"]
	}
}
```


## Report in HTML

NYC crea un file 'json' di code coverage per ogni processo nella cartella `.nyc_output`.

Per combinare tutti questi file in un report HTML leggibile, eseguire il seguente comando:

```
$ ./node_modules/.bin/nyc report --reporter=html
```

Oppure utilizza uno script `npm` per evitare di scriverlo ogni volta:

```json
{
	"scripts": {
		"report": "nyc report --reporter=html"
	}
}
```

Questo comando genererà un file HTML nella cartella `coverage`.

## Hosted Coverage

### Travis CI & Coveralls

Prima di tutto, fai il login su [coveralls.io] e attivalo sul tuo repository.

Una volta fatto, aggiungi [`coveralls`] come dipendenza da sviluppo:

```
$ npm install coveralls --save-dev
```

Poi aggiungi il seguente frammento nel file `.travis.yml`:

```yaml
after_success:
    - './node_modules/.bin/nyc report --reporter=text-lcov | ./node_modules/.bin/coveralls'
```

La tua code coverage apparirà su coveralls non appena Travis completerà l'esecuzione.

[`babel`]:      https://github.com/babel/babel
[coveralls.io]: https://coveralls.io
[`coveralls`]:  https://github.com/nickmerwin/node-coveralls
[`cross-env`]:  https://github.com/kentcdodds/cross-env
[processi-isolati]: https://github.com/avajs/ava-docs/blob/main/it_IT/readme.md#isolamento-dei-processi
[`istanbul`]:   https://github.com/gotwarlost/istanbul
[`nyc`]:        https://github.com/bcoe/nyc
