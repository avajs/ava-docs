___
**Nota del traduttore**

Questa è la traduzione del file [typescript.md](https://github.com/avajs/ava/blob/main/docs/recipes/typescript.md). Qui c'è il [link](https://github.com/avajs/ava/compare/c09462c3e515c41da8177a3d9ba5fb0f19759653...master#diff-0730bb7c2e8f9ea2438b52e419dd86c9) dove si confrontano le differenze tra commit di riferimento di questa traduzione e l'ultimo commit di AVA sul branch master (Se si clicca sul link, e non si vede il file `typescript.md` nella lista dei file modificati, questa traduzione è aggiornata).
___
# TypeScript

Traduzioni: [Español](https://github.com/avajs/ava-docs/blob/master/es_ES/docs/recipes/typescript.md), [Français](https://github.com/avajs/ava-docs/blob/master/fr_FR/docs/recipes/typescript.md), [Italiano](https://github.com/avajs/ava-docs/blob/master/it_IT/docs/recipes/typescript.md), [Русский](https://github.com/avajs/ava-docs/blob/master/ru_RU/docs/recipes/typescript.md), [简体中文](https://github.com/avajs/ava-docs/blob/master/zh_CN/docs/recipes/typescript.md)

AVA è dotato nativamente con un file definitione TypeScript. Questo permette agli sviluppatori di utilizzare TypeScript per scrivere i test.

## Configurazione

Come prima cosa installa il [TypeScript](https://github.com/Microsoft/TypeScript) (se è già installato, controlla che la versione sia 2.1 o successiva).

```
$ npm install --save-dev typescript
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

## Attivare [`context`](https://github.com/avajs/ava#test-context)

Il tipo predefinito di `t.context` è [`any`](https://www.typescriptlang.org/docs/handbook/basic-types.html#any). AVA espone un'interfaccia `RegisterContextual<T>` da utilizzare per applicare il tipo specifico a `t.context`. Questo può aiutare a rintracciare errori a tempo di compilazione:

```ts
import * as ava from 'ava';

function contextualize<T>(getContext: () => T): ava.RegisterContextual<T> {
	ava.test.beforeEach(t => {
		Object.assign(t.context, getContext());
	});

	return ava.test;
}

const test = contextualize(() => ({ foo: 'bar' }));

test.beforeEach(t => {
	t.context.foo = 123; // error:  Type '123' is not assignable to type 'string'
});

test.after.always.failing.cb.serial('very long chains are properly typed', t => {
	t.context.fooo = 'a value'; // error: Property 'fooo' does not exist on type '{ foo: string }'
});

test('an actual test', t => {
	t.deepEqual(t.context.foo.map(c => c), ['b', 'a', 'r']); // error: Property 'map' does not exist on type 'string'
});
```

## Esegui i test

```
$ npm test
```
