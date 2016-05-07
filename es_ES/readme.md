___
**Nota del traductor**

Esta es la traducción del archivo [readme.md](https://github.com/sindresorhus/ava/blob/master/readme.md). Aquí hay un [enlace](https://github.com/sindresorhus/ava/compare/f77ded959a0e7de6e9bf65d8c2fd6625bc59ba47...master#diff-0730bb7c2e8f9ea2438b52e419dd86c9) a las diferencias con la rama master de AVA (Si al hacer clic en el enlace no se encuentran modificaciones en el archivo `readme.md`, será por que la traducción está actualizada).
___
# ![AVA](https://github.com/sindresorhus/ava/blob/master/media/header.png)

> Test runner futurista

[![Build Status: Linux](https://travis-ci.org/sindresorhus/ava.svg?branch=master)](https://travis-ci.org/sindresorhus/ava) [![Build status: Windows](https://ci.appveyor.com/api/projects/status/igogxrcmhhm085co/branch/master?svg=true)](https://ci.appveyor.com/project/sindresorhus/ava/branch/master) [![Coverage Status](https://coveralls.io/repos/sindresorhus/ava/badge.svg?branch=master&service=github)](https://coveralls.io/github/sindresorhus/ava?branch=master) [![Gitter](https://badges.gitter.im/join chat.svg)](https://gitter.im/sindresorhus/ava)

A pesar de que JavaScript se ejecuta en un solo hilo, IO en Node.js puede ejecutarse en paralelo debido a su naturaleza asíncrona. AVA aprovecha esto y corre sus tests al mismo tiempo, lo que es especialmente beneficioso para tests pesados en IO. Además, los archivos de test se ejecutan en paralelo como procesos separados, que le da un mejor rendimiento y un entorno aislado para cada archivo de test. [Cambiando](https://github.com/sindresorhus/pageres/commit/663be15acb3dd2eb0f71b1956ef28c2cd3fdeed0) de Mocha a AVA en Pageres llevó el tiempo de los test por debajo de 31 segundos, concretamente a 11 segundos. El tener tests que se ejecutan al mismo tiempo nos obliga a escribir tests atómicos, es decir, los tests no dependen del estado global o el estado de otros tests, lo que está muy bien!

*Lea nuestra [guía de contribución](contributing.md) si está pensando en contribuir (issues/PRs/etc).*

Siga la [cuenta de Twitter de AVA](https://twitter.com/ava__js) para actualizaciones.

Traducciones: [Español](https://github.com/sindresorhus/ava-docs/blob/master/es_ES/readme.md), [Français](https://github.com/sindresorhus/ava-docs/blob/master/fr_FR/readme.md), [Italiano](https://github.com/sindresorhus/ava-docs/blob/master/it_IT/readme.md), [日本語](https://github.com/sindresorhus/ava-docs/blob/master/ja_JP/readme.md), [Portugués](https://github.com/sindresorhus/ava-docs/blob/master/pt_BR/readme.md)


## Tabla de contenidos

- [Uso](#uso)
- [Uso de CLI](#cli)
- [Configuración](#configuración)
- [Documentación](#documentación)
- [API](#api)
- [Aserciones](#aserciones)
- [FAQ](#faq)
- [Recetas](#recetas)


## ¿Por qué AVA?

- Mínima y rápida
- Sintaxis simple de test
- Ejecuta los tests concurrentemente
- Fuerza a escribir tests atómicos
- Sin globales implícitas
- [Entorno aislado para cada archivo de test](#entorno-aislado)
- [Escribe tus tests en ES2015](#soporte-a-es2015)
- [Soporte a Promesas](#soporte-a-promesas)
- [Soporte a Generadores](#soporte-a-generadores)
- [Soporte a Async](#soporte-a-funciones-async)
- [Soporte a Observable](#soporte-a-observable)
- [Aserciones mejoradas](#aserciones-mejoradas)
- [Salida de TAP opcional](#salida-tap-opcional)
- [Trazas de pila limpias](#trazas-de-pila-limpias)


## Sintaxis de test

```js
import test from 'ava';

test(t => {
	t.same([1, 2], [1, 2]);
});
```


## Uso

#### Inicializar

Instalar AVA globalmente `$ npm install --global ava` y ejecutar `$ ava --init` (con cualquiera de las opciones) para añadir AVA a tu package.json o crear una.

```json
{
	"name": "awesome-package",
	"scripts": {
		"test": "ava"
	},
	"devDependencies": {
		"ava": "^0.6.0"
	}
}
```

#### Crear tu archivo de test

```js
import test from 'ava';
import delay from 'delay';

test('foo', t => {
	t.pass();
});

test('bar', async t => {
	t.plan(1);

	const bar = Promise.resolve('bar').then(delay(200));

	t.is(await bar, 'bar');
});
```

<img src="https://github.com/sindresorhus/ava/blob/master/screenshot.png" width="150" align="right">

#### Ejecutar el test

```
$ npm test
```


## CLI

```
$ ava --help

  Usage (Uso)
    ava [<file|folder|glob> ...]

  Options (Opciones)
    --init       	Add AVA to your project (Añadir AVA a su proyecto)
    --fail-fast  	Stop after first test failure (Parar después de que el test falle)
    --serial, -s    Run tests serially (Ejecutar tests en serie)
    --require, -r   Module to preload (Can be repeated) (Módulo a precargar (se puede repetir))
    --tap, -t       Generate TAP output (Generar salida de TAP)
    --verbose, -v   Enable verbose output (Habilitar el modo verboso)

  Examples (Ejemplos)
    ava
    ava test.js test2.js
    ava test-*.js
    ava test
    ava --init
    ava --init foo.js

  Default patterns when no arguments (Patrones por defecto cuando no hay argumentos):
  test.js test-*.js test/**/*.js
```

* Tenga en cuenta que el CLI usará su instalación local de AVA cuando esté disponible, incluso cuando se ejecute globalmente.*

Los directorios son recursivos por defecto. Los archivos que se encuentren en directorios llamados `fixtures` y `helpers` serán ignorados, así como los archivos que comienzan con `_`. Esto nos puede ser útil para tener helpers en el mismo directorio que los archivos de test.

## Configuración

Todas las opciones de CLI pueden configurarse en la sección de 'ava' de su 'package.json'. Esto le permite modificar el comportamiento predeterminado del comando 'ava', para no tener que escribir varias veces las mismas opciones en el indicador de comandos.

```json
{
  "ava": {
    "files": [
      "my-test-folder/*.js",
      "!**/not-this-file.js"
    ],
    "failFast": true,
    "tap": true,
    "require": ["babel-core/register", "coffee-script/register"]
  }
}
```

Los argumentos pasados al CLI siempre tendrán prioridad sobre la configuración en 'package.json'.

## Documentación

Los tests se ejecutan asincronamente y requieren devolver y soportar un objeto asíncrono (una promesa, u [observable](https://github.com/zenparsing/zen-observable)). Nosotros recomendamos *altamente* el uso de [funciones asícronas](#soporte-a-funciones-async); Hacen el código asíncrono, conciso y fácil de leer, y de forma implícita devuelven una promesa, por lo que no tiene que hacerla.

Si usted no devuelve uno de los objetos soportados y asincrónicos mencionados anteriormente, la prueba se considerará síncrona y terminará inmediatamente.

Si usted es incapaz de usar promesas u otros objetos asíncronos soportados, puede activar el "callback" por la definición de su test con `test.cb([title], fn)`. Los test declarados de esta manera **deben** terminar manualmente con `t.end()`. Este modo está pensado principalmente para las APIs de tests con estilo callback.

Debe definir todas las pruebas de forma sincrona. No pueden estar definidos dentro de `setTimeout`, `setImmediate`, etc.

Los archivos de test se ejecutan desde el directorio actual, con lo que [`process.cwd()`](https://nodejs.org/api/process.html#process_process_cwd) es siempre el mismo que [`__dirname`](https://nodejs.org/api/globals.html#globals_dirname). Puede tambien usar rutas de acceso relativas en lugar de hacer `path.join(__dirname, 'relative/path')`.

### Anatomía de test

Para crear un test, llamaremos a la función de `test` que requiera de AVA y pasaremos un nombre de test opcional y una función que contiene la ejecución del test. La función que pasemos será tomada en contexto como el primer argumento, dónde puedes invocar los diferentes métodos y [aserciones](#aserciones) de AVA.

```js
test('name', t => {
	t.pass();
});
```

### Nombre opcional del test

Dar nombre a un test es opcional, pero se recomienda usar uno si tenemos más de un test.

```js
test(t => {
	t.pass();
});
```

También puede optar por utilizar una función nombrada en su lugar:

```js
test(function name(t) {
	t.pass();
});
```

### Plan de aserción

Un plan de aserción se puede utilizar para garantizar que un número determinado de afirmaciones se cumplen. El escenario más común es validar que el test no ha salido antes de ejecutar el número esperado de aserciones. También falla el test si se ejecutan demasiadas aserciones, puede ser útil si tiene aserciones dentro de callbacks o bucles. Tenga en cuenta que, a diferencia del nodo-tap y tape, AVA *no* terminará automaticamente un test cuando se alcance el final previsto de la aserción.

Esto terminará en un test ejecutado satisfactoriamente:

```js
test(t => {
	t.plan(1);

	return Promise.resolve(3).then(n => {
		t.is(n, 3);
	});
});

test.cb(t => {
	t.plan(1);

   someAsyncFunction(() => {
  		t.pass();
  		t.end();
 	});
});
```

### Serial-tests

Mientras que la concurrencia es impresionante, hay algunas cosas que no se pueden hacer concurrentemente. En estos extraños casos, puede usar `test.serial`, lo que obligará a esos tests funcionar en serie antes de los concurrentes.

```js
test.serial(t => {
	t.pass();
});
```

### Only-tests

Only-tests fuerza a que se cumplan sólo los test que se ejecutan. Esto puede ser útil para el funcionamiento de sólo unos pocos tests durante el desarrollo.

```js
test('will not be run', t => {
	t.fail();
})

test.only('will be run', t => {
	t.pass();
});
```

### Skip-tests

Skip-tests se muestran como omitidos pero nunca ejecutados.

```js
test.skip('will not be run', t => {
	t.fail();
});
```

### Before & after hooks

Cuando se requiera empezar a preparar y/o abandonar, puede usar `test.before()` y `test.after()`,
usandolo de la misma manera que `test()`. La función test permite el uso de `test.before()` y `test.after()`, siempre serán ejecutadas antes o despues de todos los tests. Puede usar incluso `test.beforeEach()` y `test.afterEach()` si necesita preparar algo antes y despues de cada test. Los hooks se ejecutan en serie en el archivo de test. Añada tantos como quiera. Opcionalmente puede especificar un título que se muestre en el error.

Si necesita crear un estado global entre tests usando 'test.beforeEach()' y 'test.afterEach()' (como espiar 'console.log' [por ejemplo] (https://github.com/sindresorhus/ava/issues/560)), necesitará asegurarse de que los tests se ejecutan en serie (mediante [test.serial](#serial-tests) o ['--serial'](#cli)).

```js
test.before(t => {
	// esto se ejecuta antes de todos los tests
});

test.before(t => {
	// esto se ejecuta después del anterior, pero antes de los tests
});

test.after('cleanup', t => {
	// esto se ejecuta despues de todos los tests
});

test.beforeEach(t => {
	// esto se ejecuta antes de cada test
});

test.afterEach(t => {
	// esto se ejecuta despues de cada test
});

test(t => {
	// test regular
});
```

Puede utilizar las funciones asíncronas, devolver objetos asincrónicos, o activar el "modo callback" en cualquiera de los hooks.

```js
test.before(async t => {
	await promiseFn();
});

test.cb.beforeEach(t => {
	setTimeout(t.end);
});

test.afterEach.cb(t => {
	setTimeout(t.end);
});

test.after(t => {
   return new Promise(/* ... */);
});
```

Los hooks `beforeEach` y `afterEach` pueden compartir el contexto del test:

```js
test.beforeEach(t => {
	t.context.data = generateUniqueData();
});

test(t => {
	t.is(t.context.data + 'bar', 'foobar');
});
```

El contexto por defecto es un objeto, pero también se puede asignar directamente:

```js
test.beforeEach(t => {
	t.context = 'unicorn';
});

test(t => {
	t.is(t.context, 'unicorn');
});
```

### Encadenar modificaciones de test

Puede encadenar modificaciones de test juntas de la siguiente manera:

```js
test.before.skip([title], testFn);
test.skip.after(....);
test.serial.only(...);
test.only.serial(...);
```

Es especialmente util utilizar temporalmente `skip` o `only` en un test, sin perder la información y el comportamiento que los otros modificadores proporcionan.

### Módulo aserción customizado

Puede utilizar cualquier módulo de aserción en lugar o además del que viene con AVA, pero no podrá utilizar el método `.plan()`, [aún](https://github.com/sindresorhus/ava/issues/25).

```js
import assert from 'assert';

test(t => {
	assert(true);
});
```

### Soporte a ES2015

AVA está construido con soporte a ES2015 a través de [Babel 6](https://babeljs.io). Sólo tiene que escribir sus pruebas en ES2015. No necesitará ninguna configuración adicional. Se puede utilizar cualquier versión de Babel en su proyecto. Utilizamos nuestro propio paquete de Babel con el [`es2015`](http://babeljs.io/docs/plugins/preset-es2015/) y [`stage-2`](http://babeljs.io/docs/plugins/preset-stage-2/) preajustados, así como [`espower`](https://github.com/power-assert-js/babel-plugin-espower) y [`transform-runtime`](https://babeljs.io/docs/plugins/transform-runtime/) plugins.

La correspondiente configuración de Babel para la configuración de AVA es la siguiente:

```json
{
  "presets": [
    "es2015",
    "stage-0",
  ],
  "plugins": [
    "espower",
    "transform-runtime"
  ]
}
```

Puede personalizar cómo AVA transpila los archivos de test a través de la opción de `babel` en AVA [`package.json` configuration](#configuración). Por ejemplo, para reemplazar los valores predeterminados se pueden utilizar:

```json
{
	"ava": {
		 "babel": {
			 "presets": [
					"es2015",
					"stage-0",
					"react"
			 ]
		 }
	},
}
```

También puede utilizar la palabra especial `"inherit"`. Esto hace que AVA difiera a la configuración de Babel [archivo `.babelrc` o `package.json`] (https://babeljs.io/docs/usage/babelrc/). De esta manera los archivos de test serán transpilados utilizando la misma configuración como los archivos de origen sin tener que repetirse para AVA:

```json
{
   "babel": {
	   "presets": [
		   "es2015",
		   "stage-0",
		   "react"
	   ]
   },
   "ava": {
	   "babel": "inherit",
   },
}
```

Tenga en cuenta que AVA *siempre* aplicará la [`espower`] (https://github.com/power-assert-js/babel-plugin-espower) y plugins [`transform-runtime`] (https://babeljs.io/docs/plugins/transform-runtime/).

### Soporte a TypeScript

AVA incluye typings para TypeScript. Tienes que configurar la transpilación por tí mismo. Cuando se establece en el archivo `tsconfig.json` `módulo` a `commonjs`, TypeScript encontrará automáticamente las definiciones de tipo de AVA. Se debe establecer `target` a `es2015` con promesas y funciones de async.


#### Transpilando módulos importados

AVA actualmente sólo transpilará los tests que usted le pida que ejecute. *No transpilará módulos que ```importen``` de fuera del test.* Si bien hay razones válidas para adoptar este enfoque, puede que no sea lo que esperas!

Como solución simple, puede utilizar [Babel's require hook](https://babeljs.io/docs/usage/require/) con el fin de hacer transpilación sobre la marcha de los módulos que se importan posteriormente. Por que AVA soporta la sintaxis de módulo de ES2015, se puede utilizar para importar el hook requerido en sí:

```js
import test from 'ava';
import 'babel-core/register';
import foo from './foo'; // <-- foo puede ser escrito en ES2015!

test('foo bar', t => {
	t.same('baz', foo('bar'));
});
```

[#111](https://github.com/sindresorhus/ava/issues/111) esta haciendo seguimiento de este tema como una mejora potencial.

### Soporte a Promesas

Si usted devuelve una promesa en el test no necesitará poner fin de manera explícita al test, ya que terminará cuando la promesa se resuelva.

```js
test(t => {
	return somePromise().then(result => {
		t.is(result, 'unicorn');
	});
});
```

### Soporte a Generadores

AVA viene con soporte a [generadores](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Statements/function*).

```js
test(function * (t) {
	const value = yield generatorFn();
	t.true(value);
});
```

### Soporte a funciones async

AVA viene con soporte a [funciones async](https://tc39.github.io/ecmascript-asyncawait/) *(async/await)*.

```js
test(async function (t) {
	const value = await promiseFn();
	t.true(value);
});

// async arrow function
test(async t => {
	const value = await promiseFn();
	t.true(value);
});
```

### Soporte a Observable

AVA viene con soporte a [observables](https://github.com/zenparsing/es-observable).
Si devuelve un observable a partir de un test, AVA automáticamente podrá consumirlo hasta su finalización antes de terminar el test.

*Si no necesita usar "callback mode" o llamar a `t.end()`.*

```js
test(t => {
	t.plan(3);
	return Observable.of(1, 2, 3, 4, 5, 6)
		.filter(n => {
			// sólo números
			return n % 2 === 0;
		})
		.map(() => t.pass());
});
```

### Soporte a Callback

AVA es compatible con el uso de `t.end` como callback final al utilizar las API de callback de error-first y node-style. AVA considerará como error cualquier valor truthy pasado como primer argumento de `t.end`. Tenga en cuenta que `t.end` requiere "callback mode", que se puede activar mediante el uso de la cadena `test.cb`.

```js
test.cb(t => {
	// t.end comprueba automáticamente los errores para el primer argumento
	fs.readFile('data.txt', t.end);
});
```

### Salida TAP opcional

AVA puede generar una salida TAP a través de la opción `--tap` para su uso con cualquier [TAP reporter](https://github.com/sindresorhus/awesome-tap#reporters).

```
$ ava --tap | tap-nyan
```

<img src="https://github.com/sindresorhus/ava/blob/master/media/tap-output.png" width="398">

### Trazas de pila limpias

AVA elimina automáticamente líneas sin relación de la pila, lo que le permite encontrar la fuente de un error mucho más rápido.

<img src="https://github.com/sindresorhus/ava/blob/master/media/stack-traces.png" width="300">


## API

### test([título], cuerpo)
### test.serial([título], cuerpo)
### test.cb([título], cuerpo)
### test.only([título], cuerpo)
### test.skip([título], cuerpo)
### test.before([título], cuerpo)
### test.after([título], cuerpo)
### test.beforeEach([título], cuerpo)
### test.afterEach([título], cuerpo)

#### título

Tipo: `string`

Título de test.

#### cuerpo (contexto)

Tipo: `function`

Debe contener el test actual.

##### contexto

Pasado a la función de test y contiene los diferentes métodos de AVA y [aserciones](#aserciones).

###### .plan(count)

Planifique cuántas aserciones hay en el test. El test fallará si el recuento actual de aserciones no coincide con las aserciones planificadas.

###### .end()

Finalizar el test. Sólo funciona con `test.cb()`.


## Aserciones

Las aserciones se mezclan en el [contexto](#contexto) del test:

```js
test(t => {
	t.ok('unicorn'); // aserción
});
```

Si se encuentran varios errores de aserción en un solo test, AVA sólo mostrará la *primera*.

### .pass([mensaje])

Pasando la aserción.

### .fail([mensaje])

Fallando la aserción.

### .ok(valor, [mensaje])

Afirmando que `valor` es truthy.

### .notOk(valor, [mensaje])

Afirmando que `valor` es falsy.

### .true(valor, [mensaje])

Afirmando que `valor` es `true`.

### .false(valor, [mensaje])

Afirmando que `valor` es `false`.

### .is(valor, esperado, [mensaje])

Afirmando que `valor` es igual a `esperado`.

### .not(valor, esperado, [mensaje])

Afirmando que `valor` no es igual a `esperado`.

### .same(valor, esperado, [mensaje])

Afirmando que `valor` es profundamente igual a `esperado`.

### .notSame(valor, esperado, [mensaje])

Afirmando que `valor` no es profundamente igual a `esperado`.

### .throws(function|promise, [error, [message]])

Afirmando que `function` arroja un error o `promesa` rechazada.

`error` puede contener un constructor, expresiones regulares, mensaje de error o la función de validación.

### .notThrows(function|promise, [mensaje])

Afirma que `function` no lanza un `error` o resuelve la `promise`.

### .regex(contents, regex, [message])

Afirma que `contents` coincide con `regex`.

### .ifError(error, [mensaje])

Afirma que `error` es falsy.

## Omitir aserciones

Cualquier aserción se puede omitir usando el modificador `skip`. Aunque se omitan dichas aserciones aún se cuentan, con lo que no hay necesidad de cambiar el contador de aserciones planificado.

```js
test(t => {
  t.plan(2);
  t.skip.is(foo(), 5); // no hay necesidad de cambiar su contador planificado cuando omita aserciones.
  t.is(1, 1);
});
```

## Aserciones mejoradas

AVA viene con [`power-assert`](https://github.com/power-assert-js/power-assert) incorporado, dándole mensajes de aserción más descriptivos. Lee el test y trata de inferir más información del código.

El siguiente test:

```js
test(t => {
	const x = 'foo';
	t.ok(x === 'bar');
});
```

Normalmente dará la salida inútil:

```
false === true
```

Con las aserciones mejoradas, obtendrás:

```
t.ok(x === 'bar')
     |
     "foo"
```

Es cierto que se podría usar `t.is()` en este caso, y probablemente se debería, pero esto es sólo un ejemplo sencillo.

Probemos un ejemplo más avanzado:

```js
test(t => {
	const a = /foo/;
	const b = 'bar';
	const c = 'baz';
	t.ok(a.test(b) || b === c);
});
```

Y ahí lo tienes:

```
t.ok(a.test(b) || b === c)
       |    |     |     |
       |    "bar" "bar" "baz"
       false
```

Todos los métodos assert se han mejorado.

¡Diviértete!


## Entorno aislado

Cada archivo de test se ejecuta en un proceso de Node.js separado. Esto nos brinda un montón de beneficios. Los diferentes archivos de test ya no se afectan entre sí. Como archivos de test moqueados con el ambiente global, comandos internos imperativos, etc. Sin embargo, se hace principalmente por razones de rendimiento. Aunque Node.js puede ejecutar simultáneamente IO asíncrono, eso no ayuda mucho cuando las pruebas son operaciones síncronas pesadas que bloquean el hilo principal. Mediante la ejecución de tests al mismo tiempo y archivos de test en paralelo, tomamos todas las ventajas de los sistemas modernos.


## Consejos

### Los archivos temporales

Ejecutar tests simultáneamente tiene algunos desafíos, hacer IO es uno. Por lo general, los test por defecto crean directorios temporales en el directorio de test actual y lo limpian al final. Esto no funcionará cuando se ejecuten tests en paralelo y entrando en conflicto entre sí. La forma correcta de hacerlo es utilizar un nuevo directorio temporal para cada test. Los módulos [`tempfile`](https://github.com/sindresorhus/tempfile) y [`temp-write`](https://github.com/sindresorhus/temp-write) pueden ser útiles.

### Depuración

AVA ejecuta pruebas en paralelo por defecto, que es subóptimo cuando se necesita depurar algo. En su lugar, ejecuta pruebas en serie con la opción `--serial`:

```
$ ava --serial
```

### Cobertura de código

Puede usar [`istanbul`](https://github.com/gotwarlost/istanbul) para la cobertura de código como AVA [spawns the test files](#entorno-aislado), pero puede usar [`nyc`](https://github.com/bcoe/nyc) en su luegar, que es básicamente `istanbul` con soporte para subprocesos.

Desde la versión `5.0.0` utiliza source maps para reportar la cobertura de su código real, independientemente de la transpilación. Asegúrese de que el código que está probando incluye un source map en línea o referencia a un archivo de source map de origen. Si usa `babel/register`se puede establecer la opción `sourceMaps` en su `.babelrc` a `inline`.


## FAQ

### Por qué no `mocha`, `tape`, `node-tap`?

Mocha requiere el uso de globales implícitas como `describe` y `it` por defecto (que utilizan la mayoría de las personas), sin demasiadas opiniones buenas, hinchado, síncrono por defecto, ejecuta los tests en serie y es lento. Tape y node-tap son bastante buenos. AVA está muy inspirado en su sintaxis. Sin embargo, ambos ejecutan los tests en serie y han hecho de [TAP](https://testanything.org) un ciudadano de primera clase que ha hecho en mi humilde opinión que sus códigos base sean un poco enrevesadas y acopladas. La salida de TAP es difícil de leer, así que siempre terminan usando una herramienta de reporte de TAP externa. AVA tiene bastantes buenas opiniones y es concurrente. Viene con un simple reporter por defecto y tiene voluntad en el futuro de dar apoyo a Tun TAP reporter.

### ¿Cómo puedo utilizar reportes personalizados?

Use la [opción `--tap`](#salida-tap-opcional) con cualquier [TAP reporter](https://github.com/sindresorhus/awesome-tap#reporters).

### ¿Cómo es el nombre escrito y pronunciado?

AVA, no Ava o ava. Pronunciado [`/ˈeɪvə/` ay-və](https://github.com/sindresorhus/ava/blob/master/media/pronunciation.m4a?raw=true).

### ¿Cuál es el fondo de la cabecera?

[Andromeda galaxy.](https://es.wikipedia.org/wiki/Galaxia_de_Andr%C3%B3meda)

### Concurrencia vs. paralelismo

Concurrencia no es paralelismo. Permite paralelismo. [Aprende más.](http://stackoverflow.com/q/1050222)

## Recetas

- [Cobertura de código](docs/recipes/code-coverage.md)
- [Endpoint testing](docs/recipes/endpoint-testing.md)
- [Cuando usar `t.plan()`](docs/recipes/when-to-use-plan.md)


## Soporte

- [Stack Overflow](https://stackoverflow.com/questions/tagged/ava)
- [Gitter chat](https://gitter.im/sindresorhus/ava)
- [Twitter](https://twitter.com/ava__js)


## Relacionado

- [sublime-ava](https://github.com/sindresorhus/sublime-ava) - Fragmentos de código para tests de AVA
- [atom-ava](https://github.com/sindresorhus/atom-ava) - Fragmentos de código para tests de AVA
- [vscode-ava](https://github.com/samverschueren/vscode-ava) - Fragmentos de código para tests de AVA
- [eslint-plugin-ava](https://github.com/sindresorhus/eslint-plugin-ava) - Reglas de lint para tests de AVA
- [gulp-ava](https://github.com/sindresorhus/gulp-ava) - Ejecutar tests con gulp
- [grunt-ava](https://github.com/sindresorhus/grunt-ava) - Ejecutar tests con grunt
- [fly-ava](https://github.com/pine/fly-ava) - Ejecutar tests con fly
- [start-ava](https://github.com/start-runner/ava) - Ejecutar los tests con inicio


## Enlaces

- [Compre AVA stickers](https://www.stickermule.com/user/1070705604/stickers)
- [Lista Impresionante](https://github.com/sindresorhus/awesome-ava)


## Creado por

[![Sindre Sorhus](https://avatars.githubusercontent.com/u/170270?s=130)](http://sindresorhus.com) | [![Kevin Mårtensson](https://avatars.githubusercontent.com/u/709159?s=130)](https://github.com/kevva) | [![Vadim Demedes](https://avatars.githubusercontent.com/u/697676?s=130)](https://github.com/vdemedes) | [![James Talmage](https://avatars.githubusercontent.com/u/4082216?s=130)](https://github.com/jamestalmage) | [![Mark Wubben](https://avatars.githubusercontent.com/u/33538?s=130)](https://novemberborn.net)
---|---|---|---|---
[Sindre Sorhus](http://sindresorhus.com) | [Kevin Mårtensson](https://github.com/kevva) | [Vadim Demedes](https://github.com/vdemedes) | [James Talmage](https://github.com/jamestalmage) | [Mark Wubben](https://novemberborn.net)


<div align="center">
	<br>
	<br>
	<br>
	<img src="https://cdn.rawgit.com/sindresorhus/ava/fe1cea1ca3d2c8518c0cc39ec8be592beab90558/media/logo.svg" width="200" alt="AVA">
	<br>
	<br>
</div>
