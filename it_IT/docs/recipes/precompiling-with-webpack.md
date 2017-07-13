___
**Nota del traduttore**

Questa è la traduzione del file [precompiling-with-webpack.md](https://github.com/sindresorhus/ava/blob/master/docs/recipes/precompiling-with-webpack.md). Qui c'è il [link](https://github.com/avajs/ava/compare/0069a7ef4797a88149031124e7eade090a18ad4a...master#diff-b4c25a4a1412e9204ac923133f67814f) dove si confrontano le differenze tra commit di riferimento di questa traduzione e l'ultimo commit di AVA sul branch master (Se si clicca sul link, e non si vede il file `precompiling-with-webpack.md` nella lista dei file modificati, questa traduzione è aggiornata).
___
## Pre-compila i file sorgente con webpack

Translations: [Français](https://github.com/avajs/ava-docs/blob/master/fr_FR/docs/recipes/precompiling-with-webpack.md),
 [Italiano](https://github.com/avajs/ava-docs/blob/master/it_IT/docs/recipes/precompiling-with-webpack.md).

Il file [readme](https://github.com/avajs/ava-docs/blob/master/it_IT/readme.md#usare-il-transpiler-per-i-moduli-importati) dice che sia possibile transpilare i moduli importati come alternativa alla compilazione a runtime, ma non spiega come farlo. Questa ricetta spiega diversi approcci per uware webpack v2. Vengono discussi diversi approcci al problema in quanto ciascun approccio ha i suoi pro ed i suoi contro. Dovrai scegliere l'approccio che meglio di presta al tuo caso. Per maggior informazioni vedi la discussione originale sul problema [qui](https://github.com/avajs/ava/pull/1385).

- [File di test singolo](#file-di-test-singolo)

- [File di test multipli](#file-di-test-multipli)

### File di test singolo

Questo è il caso base, cioè il più semplice. Potresti aver bisogno di questo approccio qualosa stessi usando gli [alias](https://github.com/avajs/ava/issues/1011).


###### `webpack.config.js`

```js
const path = require('path');
const nodeExternals = require('webpack-node-externals');
module.exports = {
	entry: ['test.js'],
	target: 'node',
	output: {
		path: path.resolve(__dirname, '_build'),
		filename: 'test.js'
	},
	externals: [nodeExternals()],
	module: {
		rules: [
			{
				test: /\.(js|jsx)$/,
				use: 'babel-loader',
				options: {
					cacheDirectory: true
				}
			}
		]
	}
};
```

I punti salienti della configurazione sopra sono `target: 'node'`, che ignora le istruzioni `require` specifiche per Node.js (es. `fs`, `path`, etc.) e `externals: nodeModules`, che inibisce webpack dall'includere moduli specifici di Node.js che potrebbero dare problemi.

Ora puoi eseguire `$ ava _build/test.js` per eseguire i test inclusi nell'output.

### File di test multipli

Per questo scenario le cose sono un po' più complicate. Noi raccomandiamo l'uso di [babel-register](https://github.com/avajs/ava-docs/blob/master/it_IT/docs/recipes/babelrc.md) almeno fino a quando non ci sono grandi problemi di performance.

I possibili approcci in questo caso sono:

- [Usare un riferimento ai sorgenti precompilati nei test](#usare-un-riferimento-ai-sorgenti-precompilati-nei-test)

- [Usare un singolo entry file](#usare-un-singolo-entry-file)

- [Usare più entry file](#usare-piu-entry-file)

- [Testare sorgenti precompilati](#testare-sorgenti-precompilati)

#### Usare un riferimento ai sorgenti precompilati nei test

I file sorgenti possono essere compilati nella cartella `_src` e poi chiamati nei file di test. Anche se questa opzione è la meno elegante, ha buone prestazioni ed il flusso può essere ottimizzato con il [watch mode di `babel-cli`](https://babeljs.io/docs/usage/cli/#babel).

```js
// Before
import fresh from '../src';

// After
import fresh from '../_src';
```

### Usare un singolo entry file

Si possono compilare più file di test in un singolo file. Questo approccio ha le performance migliori anche se ha alcuni svantaggi. Tutti i test saranno infatti inclusi in un unico file, quindi potrebbe essere più complicato sapere quale file di test è fallito, poichè AVA non potrà più mostrare le informazioni sul file di test originario. Un altro svantaggio riguarda la perdita dell'[isolamento dei processi](https://github.com/avajs/ava-docs/blob/master/it_IT/readme.md#isolamento-de-processi).

###### `webpack.config.js`

[Risposta originaria da Stack Overflow](http://stackoverflow.com/questions/32874025/how-to-add-wildcard-mapping-in-entry-of-webpack/34545812#34545812)
```js
const path = require('path');
const glob = require('glob');
const nodeExternals = require('webpack-node-externals');

module.exports = {
	target: 'node',
	entry: glob.sync('./test/**/*.js'),
	output: {
		path: path.resolve(__dirname, '_build'),
		filename: 'tests.js'
	},
	externals: [nodeExternals()],
	module: {
		rules: [
			{
				test: /\.(js|jsx)$/,
				use: {
					loader: 'babel-loader',
					options: {
						cacheDirectory: true
					}
				}
			}
		]
	}
};
```

<details>
<summary>Comparazione del report di errore</summary>

```
# Before
  aggregations-test » cardinality-agg » sets precision_threshold option
  E:\Projects\repos\elastic-builder\test\_macros.js:167
   166:         const expected = getExpected(keyName, recursiveToJSON(propValue));
   167:         t.deepEqual(value, expected);
   168:     }
  Difference:
      Object {
        my_agg: Object {
          cardinality: Object {
    -       precision_threshol: 5000,
    +       precision_threshold: 5000,
          },
        },
      }
# After
  sets precision_threshold option
  E:\Projects\repos\elastic-builder\_build\tests.js:106
   105:                     column: 21
   106:                 }
   107:             },
  Difference:
      Object {
        my_agg: Object {
          cardinality: Object {
    -       precision_threshol: 5000,
    +       precision_threshold: 5000,
          },
        },
      }
```

</details>

### Usare più entry file

È possibile configurare webpack per generare più entry file. Questo approccio aiuta nel mantenere i nomi dei file di test così che il report in caso di errore siano più facili da interpretare. Lo svantaggio in questo caso è che ogni entry file avrà la sua copia dei file sorgenti. Il risultato finale sarà quindi una serie di file molto più grandi. Le performance in questo caso saranno [molto basse](https://github.com/avajs/ava/pull/1385#issuecomment-304684047) alla prima esecuzione.

###### `webpack.config.js`

```js
const path = require('path');
const glob = require('glob');
const nodeExternals = require('webpack-node-externals');

const entryObj = glob.sync('./test/**/*.js')
	.reduce((acc, file) => {
		acc[path.basename(file, path.extname(file))] = file;
		return acc;
	}, {});

module.exports = {
	target: 'node',
	entry: entryObj,
	output: {
		path: path.resolve(__dirname, '_build'),
		filename: '[name].js'
	},
	externals: [nodeExternals()],
	module: {
		rules: [
			{
				test: /\.(js|jsx)$/,
				use: {
					loader: 'babel-loader',
					options: {
						cacheDirectory: true
					}
				}
			}
		]
	}
};
```

#### Testare sorgenti precompilati

Questo approccio è sicuramente il più complesso ma ha una buona resa in termini di performance e mantiene i nomi dei file inalterati. Con questo approccio useremo la `babel-cli` per compilare i file sorgenti e preservare la struttura dei file. I percorsi dei file richiesti nei file di test verranno sovrascritti quando i file di test verranno compilati con webpack. L'esempio seguente è per una specifica struttura di file. A seconda di come siano organizzati i tuoi file sorgente e di test potresti aver bisogno di modificare un po' la configurazione dell'esempio.

Struttura dei file

```
├───src
│   ├───my-pkg-fldr
│   │   ├───my-module.js
│   │   └───index.js
│   └───index.js
└───test
    ├───my-pkg-fldr
    │   └───my-module.test.js
    └───index.test.js
# Struttura di file generata
├───_src
│   ├───my-pkg-fldr
│   │   ├───my-module.js
│   │   └───index.js
│   └───index.js
└───_build
    ├───my-module.test.js
    └───index.test.js
```

npm scripts:

```js
{
	"scripts": {
		"precompile-src": "cross-env NODE_ENV=test babel src --out-dir _src",
		"precompile-tests": "cross-env NODE_ENV=test webpack --config webpack.config.test.js",
		"pretest": "npm run precompile-src && npm run precompile-tests",
		"test": "cross-env NODE_ENV=test nyc --cache ava _build --concurrency 3"
	}
}
```

###### `webpack.config.js`

[Documentazione Webpack `externals`](https://webpack.js.org/configuration/externals/#function)

```js
const path = require('path');
const glob = require('glob');
const nodeExternals = require('webpack-node-externals');

const entryObj = glob.sync('./test/**/*.js')
	.reduce((acc, file) => {
		acc[path.basename(file, path.extname(file))] = file;
		return acc;
	}, {});

module.exports = {
	target: 'node',
	entry: entryObj,
	output: {
		path: path.resolve(__dirname, '_build'),
		filename: '[name].js'
	},
	externals: [
		nodeExternals(),
		// Sovrascrivi i percorsi in require() usando `_src`
		(context, request, callback) => {
            // Questo è un po' complicate perchè i test non verranno messi esattamente
            // come nella struttura originale
			// test/index.test.js → _build/index.test.js
				//=> ../src → ../_src
			// test/my-pkg-fldr/my-module.test.js → _build/my-module.test.js
				//=> ../../src → ../_src
			if (request.includes('/src')) {
				const requestReqwrite = request
					.replace('/src', '/_src')
					.replace('../../_src', '../_src');
				return callback(null, `commonjs ${requestReqwrite}`);
			}
			callback();
		}
	]
};
```