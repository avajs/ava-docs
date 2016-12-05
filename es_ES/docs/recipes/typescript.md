___
**Nota del traductor**

Esta es la traducción del archivo [typescript.md](https://github.com/avajs/ava/blob/master/typescript.md). Aquí hay un [enlace](https://github.com/avajs/ava/compare/8e2f3dca177a4283ad882596d3c1425cabb998ef...master#diff-60cce07a584082115d230f2e3d571ad6) a las diferencias con la rama master de AVA (Si al hacer clic en el enlace no se encuentran modificaciones en el archivo `typescript.md`, será por que la traducción está actualizada).
___
# TypeScript

Traducciones: [Español](https://github.com/avajs/ava-docs/blob/master/es_ES/docs/recipes/typescript.md),
[Français](https://github.com/avajs/ava-docs/blob/master/fr_FR/docs/recipes/typescript.md), [Italiano](https://github.com/avajs/ava-docs/blob/master/it_IT/recipes/typescript.md), [Русский](https://github.com/avajs/ava-docs/blob/master/ru_RU/docs/recipes/typescript.md), [简体中文](https://github.com/avajs/ava-docs/blob/master/zh_CN/docs/recipes/typescript.md)

AVA viene de serie con un fichero TypeScript con las definiciones. Esto permite a los desarrolladores usar TypeScript para escribir tests.

## Configuración

Primero instala [TypeScript](https://github.com/Microsoft/TypeScript).

```
$ npm install --save-dev typescript
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
