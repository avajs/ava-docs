___
**Nota del traduttore**

Questa è la traduzione del file [typescript.md](https://github.com/avajs/ava/blob/master/docs/recipes/typescript.md). Qui c'è il [link](https://github.com/avajs/ava/compare/195390ec1db90cf7d394407c846a69fbaa08806d...master#diff-0730bb7c2e8f9ea2438b52e419dd86c9) dove si confrontano le differenze tra commit di riferimento di questa traduzione e l'ultimo commit di AVA sul branch master (Se si clicca sul link, e non si vede il file `typescript.md` nella lista dei file modificati, questa traduzione è aggiornata).
___
# TypeScript

Traduzioni: [Français](https://github.com/avajs/ava-docs/blob/master/fr_FR/docs/recipes/typescript.md), [Русский](https://github.com/avajs/ava-docs/blob/master/ru_RU/docs/recipes/typescript.md)

AVA è dotato nativamente con un file definitione TypeScript. Questo permette agli sviluppatori di utilizzare TypeScript per scrivere i test.

## Configurazione

Come prima cosa installa il compilatore TypeScript [tsc](https://github.com/Microsoft/TypeScript).

```
$ npm install --save-dev tsc
```

Crea un file [`tsconfig.json`](https://github.com/Microsoft/TypeScript/wiki/tsconfig.json). Questo file serve per le opzioni del compilatore richieste per la compilazione del progetto o dei file di test.

```json
{
	"compilerOptions": {
		"module": "commonjs",
		"target": "es2015"
	}
}
```

Aggiungi uno script `test` nel file `package.json`. Questo servirà a compilare prima il progetto e poi eseguire AVA.

```json
{
  "scripts": {
    "test": "tsc && ava"
  }
}
```


## Aggiungi i test

Crea un file `test.ts`.


```ts
import test from 'ava';

async function fn() {
    return Promise.resolve('foo');
}

test(async (t) => {
    t.is(await fn(), 'foo');
});
```


## Esegui i test

```
$ npm test
```
