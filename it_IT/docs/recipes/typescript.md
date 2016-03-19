# TypeScript

Translations: [Français](https://github.com/sindresorhus/ava-docs/blob/master/fr_FR/docs/recipes/typescript.md), [Русский](https://github.com/sindresorhus/ava-docs/blob/master/ru_RU/docs/recipes/typescript.md)

AVA comes bundled with a TypeScript definition file. This allows developers to leverage TypeScript for writing tests.

## Setup

First install the TypeScript compiler [tsc](https://github.com/Microsoft/TypeScript).

```
$ npm install --save-dev tsc
```

Create a [`tsconfig.json`](https://github.com/Microsoft/TypeScript/wiki/tsconfig.json) file. This file specifies the compiler options required to compile the project or the test file.

```json
{
	"compilerOptions": {
		"module": "commonjs",
		"target": "es2015"
	}
}
```

___
**Nota del traduttore**

Questa è la traduzione del file [readme.md](https://github.com/sindresorhus/ava/blob/master/readme.md). Questo è il [link](https://github.com/sindresorhus/ava/compare/8d47119458e83d3899683ad3ea3a4c1c01b7dd49...master#diff-8d47119458e83d3899683ad3ea3a4c1c01b7dd49) con le differenza tra il ramo master di AVA ed il commit di quando è stata aggiornata questo file (Se si clicca sul link, e non si vede il file `readme.md` nella lista dei file modificati, questa è traduzione aggiornata).
___
Add a `test` script in the `package.json` file. It will compile the project first and then run AVA.

```json
{
  "scripts": {
    "test": "tsc && ava"
  }
}
```


## Add tests

Create a `test.ts` file.

```ts
import test from 'ava';

async function fn() {
    return Promise.resolve('foo');
}

test(async (t) => {
    t.is(await fn(), 'foo');
});
```


## Execute the tests

```
$ npm test
```
