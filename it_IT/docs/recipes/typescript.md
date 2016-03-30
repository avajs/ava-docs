___
**Nota del traduttore**

Questa è la traduzione del file [readme.md](https://github.com/sindresorhus/ava/blob/master/readme.md). Qui c'è il [link](https://github.com/sindresorhus/ava/compare/f2c070987ecee3caf7613190acf2c8a90700e058...master#diff-f2c070987ecee3caf7613190acf2c8a90700e058) dove si confrontano le differenze tra commit di riferimento di questa traduzione e l'ultimo commit di AVA sul branch master (Se si clicca sul link, e non si vede il file `readme.md` nella lista dei file modificati, questa traduzione è aggiornata).
___
# TypeScript

Traduzioni: [Français](https://github.com/sindresorhus/ava-docs/blob/master/fr_FR/docs/recipes/typescript.md), [Русский](https://github.com/sindresorhus/ava-docs/blob/master/ru_RU/docs/recipes/typescript.md), [Italiano](https://github.com/sindresorhus/ava-docs/blob/master/it_IT/recipes/typescript.md)

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
