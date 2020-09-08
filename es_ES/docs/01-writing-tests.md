# Escribiendo pruebas

Las pruebas se ejecutan concurrentemente. Tu puedes especificar si deseas que tus pruebas se ejecuten de manera sincronica o asincronica. Las pruebas se consideran sincronicas a menos que retornen una promesa, un [observable](https://github.com/zenparsing/zen-observable), o se declare uno como callback en la prueba.

Tu debes definir todas las pruebas sincronicamente. Ellas no pueden ser definidas dentro de `setTimeout`, `setImmediate`, etc.

AVA trata de ejecutar los archivos de prueba con su directorio actual de trabajo configurado al directorio que contiene el archivo `package.json`.

## Aislamiento de procesos

Cada archivo de pruebas es ejecutado en un proceso de Node.js separado. Esto permite que cambies el estado global o sobre-escribir una configuracion personalizada en un archivo de pruebas, sin afectar a otro. Esto es genial para mejorar el rendimiento sobre procesadores multi-core, permitiendo multiples archivos de prueba ejecutarse en paralelo.

AVA configurará `process.env.NODE_ENV` a `test`, a menos que la variable de ambiente `NODE_ENV` haya sido configurada. Esto es util si el codigo que estas probando tiene pruebas por defecto (por ejemplo cuando se configura que base de datos debe ser tomada para establecer una conexion). Esto puede causar que tu código o dependencias se comporten distinto. Nota que `'NODE_ENV' en process.env` siempre sera `true`.

## Declarando pruebas

Para declarar una prueba tu llamas a la funcion `test` que importas de AVA. Provees un titulo e implementacion de la funcion requeridos. El titulo debe ser unico dentro de cada archivo de pruebas. La funcion sera llamada cuando tu prueba es ejecutada y es pasada como un [execution object](./02-execution-context.md) como su primer argumento.

**Nota:** Para que [los mensajes de asercion mejorados](./03-assertions.md#enhanced-assertion-messages) se comporten correctamente, el primer argumento **debe** ser nombrado `t`.

```js
const test = require('ava');
test('mi prueba exitosa', t => {
	t.pass();
});
```

## Ejecutando pruebas serialmente

Las pruebas se ejecutan de manera concurrente por defecto, sin embargo, a veces tienes que escribir pruebas que no puede ejecutarse concurrentemente. In estos casos raros puedes usar el modificador `.serial`. Esto forzará a que dichas pruebas se ejecuten de manera serial *antes* de que se ejecuten las concurrentes.

```js
test.serial('prueba serial exitosa', t => {
	t.pass();
});
```

Nota que esto solamente aplica a las pruebas dentro de un archivo de pruebas en particulas. AVA aun ejecutará multiples archivos de prueba al mismo tiempo a menos que uses la bandera [`--serial` en el CLI](./05-command-line.md).

Puedes usar el modificador `.serial` con todas las pruebas, hooks e incluso `.todo()`, pero esto esta disponible unicamente en la funcion `test`.

## Soporte para promesas

Las pruebas pueden retornar una promesa. AVA esperará a que la promesa se resuelva antes de terminar la prueba. Si la promesa es rechazada la prueba fallará.

```js
test('la promesa se resuelve con unicorn', t => {
	return somePromise().then(result => {
		t.is(result, 'unicorn');
	});
});
```

## Soporte para funciones asincronicas

AVA incluye soporte para [funciones asincronicas](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Statements/async_function).

```js
test(async function (t) {
	const value = await promiseFn();
	t.true(value);
});
// Async arrow function
test('la promesa se resuelve a true', async t => {
	const value = await promiseFn();
	t.true(value);
});
```

## Soporte para observables

AVA incluye soporte para [observables](https://github.com/zenparsing/es-observable). 
Si retornas un observable en una prueba, AVA automaticamente lo consume antes de terminar la ejecucion de la prueba.

*No necesitas usar el "callback mode" o llamar `t.end()`.*

```js
test('manejo de observables', t => {
	t.plan(3);
	return Observable.of(1, 2, 3, 4, 5, 6)
		.filter(n => {
			// Solo numeros pares
			return n % 2 === 0;
		})
		.map(() => t.pass());
});
```

## Soporte para Callbacks

AVA soporta el uso de `t.end` como ultimo callback para chequear errores. AVA considerara como un error cualquier valor verdadero pasado como argumento a `t.end`. Note que `t.end` requiere un "callback mode", el mismo que puede habilitarse usando `test.cb`.

```js
test.cb('data.txt puede leerse', t => {
	// `t.end` automaticamente chequea por errores en el primer argumento
	fs.readFile('data.txt', t.end);
});
```

## Correr ciertas pruebas especificamente

Durante el desarrollo puede ser util solamente ejecutar pocas pruebas especificas. Esto puede lograrse usando el modificador `only`:

```js
test('no se ejecutara', t => {
	t.fail();
});
test.only('se ejecutara', t => {
	t.pass();
});
```

Puedes usar el modificador `.only` con todas las pruebas. No puede usarse con hooks o `.todo()`.

*Nota:* El modificador `.only` aplica al archivo de pruebas en el cual esta definido, por lo que si ejecutas varios archivos con pruebas, las pruebas en otros archivos se ejecutaran. Si deseas ejecutar el archivo que tiene `test.only`, indicale ese archivo de pruebas a AVA.

No puedes actualizar snapshots cuando se usa `.only()`.

## Saltar pruebas

Algunas veces pruebas que fallan son dificiles de arreglar. Puedes decirle a AVA que temporalmente salte estas pruebas usando el modificador `.skip`. Las prueban aun seran mostradas en el output (como saltadas) pero no se ejecutaran nunca.

```js
test.skip('no se ejecutara', t => {
	t.fail();
});
```

Debes especificar la funcion que la implementa. Puedes usar el modificador `.skip` con todas las pruebas y hooks, per no con `.todo()`. No puedes aplicar modificares adicionales a `.skip`.

No puedes actualizar snapshots al usar `.skip()`. Si la prueba va a estar fallando por un largo tiempo, mejor usa `.failing()` en su lugar.

## Marcadores para pruebas ("todo")

Tu puedes usar el modificador `.todo` cuando estas planeando escribir una prueba. Al igual que las pruebas saltadas estos marcadores son mostrados en el output. Solo requieren un titulo; no puedes proveer la funcion que lo implementa

```js
test.todo('pensare acerca de esto luego');
```

Puedes indicar que necesitar escribit una prueba que se ejecute de manera serial:

```js
test.serial.todo('pensare acerca de esto luego');
```

## Pruebas fallidas

Tu puedes usar el modificador `.failing` para documentar codigo que necesita ser arreglado. Pruebas fallidas son ejecutadas al igual que las normales, pero se espera que fallen, y no romperan tu buid cuando fallan.
Si una prueba marcada como fallida se ejecuta sin errores, un error sera reportado y fallara el build con un mensaje indicando que el modificador `failing` debe ser removido.

Esto nos permite mezclar `.failing` tests antes de implementar una solucion sin romper un build.
Es una forma genial de reconocer un buen reporte de errores incluso si quien reporta el error no sabe como arreglarlo.

```js
// See: github.com/user/repo/issues/1234
test.failing('demuestra algun error', t => {
	t.fail(); // Se cuenta con que la prueba pase
});
```

## Hooks before & after

AVA te permite registrar hooks que se ejecutan antes y despues de tus pruebas. Esto permite que realices ciertas configuraciones antes de ejecutar tus pruebas o termines el uso de ciertos recursos cuando tus pruebas se hayan ejecutado.

`test.before()` registra un hook antes de que la primera prueba en tu archivo se ejecute. 
De manera similar `test.after()` registra un hook a ser ejecutado luego de la ultima prueba. 
Use `test.after.always()`  para registrar un hook que siempre se ejecutara cuando tus pruebas y otros hooks hayan terminado de ejecutarse. Los `.always()` hooks son ejecutados sin importar si hay fallas tempranas, por lo cual son ideales como tareas de limpieza. Nota sin embargo que exceptiones no tratadas, rechazos no manejados o timeouts romperan tus pruebas, posiblemente no permites que los hook `always` sean ejecutados.

`test.beforeEach()` registra un hook a ser ejecutado antes de cada prueba en tu archivo de pruebas. De forma similar, `test.afterEach()` registra un hook a ejecutarse luego de cada prueba. Usa `test.afterEach.always()` para registrar un hook a ejecutarse incluso si otros hooks o pruebas fallan.

Si una prueba es ignorada con el modificador `.skip`, los respectivos `.beforeEach()`, `.afterEach()` and `.afterEach.always()` hooks no se ejecutaras. De igual manera, si todas las pruebas en un archivo de pruebas son saltados, los hooks `.before()`, `.after()` y `.after.always()` del archivo no se ejecutaran.

*Tu podria no necesitar usar `.afterEach.always()` para realizar tareas de limpieza posterior a una prueba.* Tu puedes usar [`t.teardown()`](./02-execution-context.md#tteardownfn) para remover side-effects *dentro* de una prueba en particular.

Al igual que `test()` estos metodos toman un titulo de manera opcional y una funcion que los implementa. El titulo es mostrado si el hook falla al ejecutarse. La implementacion es llamada con un [objecto execution](./02-execution-context.md). Puedes usar aserciones en tus hooks. Tambien puedes pasar una [function macro](#reusing-test-logic-through-macros) y argumentos adicionales.

Los hooks `.before()` se ejecutan antes de los hooks `.beforeEach()`. Los hooks `.afterEach()` se ejecutan antes de los hooks `.after()`. Dentro de su categoria los hooks se ejecutan en el ordern en donde ellos fueron definidos. Por defecto los hooks se ejecutan de manera concurrente, pero puedes hacer que se ejecuten uno a la vez usando `test.serial`. A diferencia de las pruebas, los hooks seriales no correran antes que otros hooks:

```js
test.before(t => {
	// Corre antes de todas las pruebas
});
test.before(t => {
	// Corre concurrentemente con el anterior
});
test.serial.before(t => {
	// Se ejecuta luego del anterior
});
test.serial.before(t => {
	// Se ejecuta luego del anterior y antes de las pruebas
});
test.after('limpiar', t => {
	// Se ejecuta luego de todas las pruebas
});
test.after.always('limpieza garantizada', t => {
	// Siempre se ejecutara, sin importar de error tempraneros
});
test.beforeEach(t => {
	// Se ejecuta antes de cada test
});
test.afterEach(t => {
	// Se ejecuta luego de cada test
});
test.afterEach.always(t => {
	// Se ejecuta luego de cada prueba y otros hooks de la prueba incluso si han fallado
});
test('titulo de la prueba', t => {
	// Prueba normal
});
```

Los hooks pueden ser sincronicos o asincronicos, justo igual que las pruebas. Para hacer que un hook asincrónico devuelva una promesa u observable, use una función asíncrona o habilite el modo callback a través de `test.before.cb ()`, `test.beforeEach.cb ()` etc.

```js
test.before(async t => {
	await promiseFn();
});
test.after(t => {
	return new Promise(/* ... */);
});
test.beforeEach.cb(t => {
	setTimeout(t.end);
});
test.afterEach.cb(t => {
	setTimeout(t.end);
});
```

Tenga en cuenta que los hooks `.beforeEach ()` y `.afterEach ()` se ejecutan justo antes y después de que se ejecuta una prueba, y que de forma predeterminada, las pruebas se ejecutan simultáneamente. Esto significa que cada hook `.beforeEach ()` se puede ejecutar simultáneamente. El uso de `test.serial.beforeEach ()` no cambia esto. Si necesita configurar el estado global para cada prueba (como espiar en `console.log` [por ejemplo] (https://github.com/avajs/ava/issues/560)), deberá asegurarse que las pruebas como tal son [ejecutadas en serie] (#running-tests-serially).

Recuerda que AVA ejecuta cada archivo de prueba en su propio proceso. Es posible que no tenga que limpiar el estado global en un hook `.after ()` ya que solo se llama justo antes de que finalice el proceso.

## Contexto de las pruebas

Los hooks pueden compartir contexto con la prueba:

```js
test.beforeEach(t => {
	t.context.data = generateUniqueData();
});
test('la context data es foo', t => {
	t.is(t.context.data + 'bar', 'foobar');
});
```

El contexto creado en los hooks `.before ()` es [clonado] (https://www.npmjs.com/package/lodash.clone) antes de pasarlo a los hooks `.beforeEach ()` y/o pruebas. Los hooks `.after ()` y `.after.always ()` reciben el valor del contexto original.

Para los hooks `.beforeEach()`, `.afterEach()` y `.afterEach.always()` el contexto no es compartido entre diferentes pruebas, permitiéndote configurar datos de manera que no se filtren a otras pruebas.

Por defecto, `t.context` es un objeto, pero puedes reasignarlo:

```js
test.before(t => {
	t.context = 'unicorn';
});
test('el contexto es unicorn', t => {
	t.is(t.context, 'unicorn');
});
```

## Recuperando metadatos de prueba

Acceda a los datos sobre el archivo de prueba leyendo `test.meta`.
Propiedades disponibles:

* `file`: ruta al archivo de prueba
* `snapshotDirectory`: directorio donde se almacenan los snapshots

```js
const test = require('ava');
console.log('Archivo de prueba que se esta ejecutando actualmente:', test.meta.file);
```

## Reutilización de la lógica de prueba a través de macros

Los argumentos adicionales pasados a la declaración de prueba se pasarán a la implementación de prueba. Esto es útil para crear macros de prueba reutilizables.

```js
function macro(t, input, expected) {
	t.is(eval(input), expected);
}
test('2 + 2 = 4', macro, '2 + 2', 4);
test('2 * 3 = 6', macro, '2 * 3', 6);
```

Puede crear el título de la prueba mediante programación adjuntando una función `title` a la macro:

```js
function macro(t, input, expected) {
	t.is(eval(input), expected);
}
macro.title = (providedTitle = '', input, expected) => `${providedTitle} ${input} = ${expected}`.trim();
test(macro, '2 + 2', 4);
test(macro, '2 * 3', 6);
test('providedTitle', macro, '3 * 3', 9);
```

El argumento `provideTitle` toma el valor predeterminado de` undefined` si el usuario no proporciona un título. Esto significa que puede utilizar una asignación de parámetros para establecer el valor predeterminado. El ejemplo anterior usa unaa cadena vacía como predeterminada.

También puede pasar arreglos de funciones macro:

```js
const safeEval = require('safe-eval');
function evalMacro(t, input, expected) {
	t.is(eval(input), expected);
}
function safeEvalMacro(t, input, expected) {
	t.is(safeEval(input), expected);
}
test([evalMacro, safeEvalMacro], '2 + 2', 4);
test([evalMacro, safeEvalMacro], '2 * 3', 6);
```

Le recomendamos que utilice macros en lugar de crear sus propios generadores de prueba ([aquí hay un ejemplo] (https://github.com/avajs/ava-codemods/blob/47073b5b58aa6f3fb24f98757be5d3f56218d160/test/ok-to-truthy.js#L7 -L9) de código que debe reemplazarse con una macro). Las macros están diseñadas para realizar análisis estáticos de su código, lo que puede conducir a un mejor rendimiento, integración de IDE y reglas de linter.
