___
**Nota del traduttore**

Questa è la traduzione del file [readme.md](https://github.com/sindresorhus/ava/blob/master/readme.md). Questo è il [link](https://github.com/sindresorhus/ava/compare/8d47119458e83d3899683ad3ea3a4c1c01b7dd49...master#diff-8d47119458e83d3899683ad3ea3a4c1c01b7dd49) con le differenza tra il ramo master di AVA ed il commit di quando è stata aggiornata questo file (Se si clicca sul link, e non si vede il file `readme.md` nella lista dei file modificati, questa è traduzione aggiornata).
___
# TypeScript

Traduzioni: [Français](https://github.com/sindresorhus/ava-docs/blob/master/fr_FR/docs/recipes/typescript.md), [Русский](https://github.com/sindresorhus/ava-docs/blob/master/ru_RU/docs/recipes/typescript.md)

AVA è dotato nativamente con un file definitione TypeScript. Questo permette agli sviluppatori di utilizzare TypeScript per scrivere i tests.

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
