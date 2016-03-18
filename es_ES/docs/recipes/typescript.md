# TypeScript

Traducciones: [Français](https://github.com/sindresorhus/ava-docs/blob/master/fr_FR/docs/recipes/typescript.md), [Русский](https://github.com/sindresorhus/ava-docs/blob/master/ru_RU/docs/recipes/typescript.md), [Español](https://github.com/sindresorhus/ava-docs/blob/master/es_ES/docs/recipes/typescript.md)

AVA viene de serie con un fichero TypeScript con las definiciones. Esto permite a los desarrolladores usar TypeScript para escribir tests.

## Configuración

Primero instala el compilador de TypeScript [tsc](https://github.com/Microsoft/TypeScript).

```
$ npm install --save-dev tsc
```

Crea un fichero [`tsconfig.json`](https://github.com/Microsoft/TypeScript/wiki/tsconfig.json). Este fichero especifica al compilador las opciones necesarias para compilar el proyecto o el fichero de tests.

```json
{
	"compilerOptions": {
		"module": "commonjs",
		"target": "es2015"
	}
}
```

Añade un script `test` en el fichero `package.json`. Primero compilará el proyecto y luego ejecutará AVA.

```json
{
  "scripts": {
    "test": "tsc && ava"
  }
}
```


## Añadir tests

Crea un fichero `test.ts`.

```ts
import test from 'ava';

async function fn() {
    return Promise.resolve('foo');
}

test(async (t) => {
    t.is(await fn(), 'foo');
});
```


## Ejecutar los tests

```
$ npm test
```