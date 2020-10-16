# Aserciones

Las aserciones son mezcladas como parte del [contexto de ejecución](./02-execution-context.md) proporcionado a cada implementación de prueba:

```js
test('los unicornios existen', t => {
	t.truthy('unicorn'); // asercion
});
```

Las aserciones están vinculadas a sus pruebas, por lo que puedes asignarlas a una variable o pasarlas como argumentos:

```js
test('los unicornios existen', t => {
	const truthy = t.truthy;
	truthy('unicorn');
});
```

Si se encuentran múltiples fallas en las aserciones dentro de una sola prueba, AVA solo mostrará la *primera* de ellas.

## Planificación de aserciones

La planificacion de aserciones garantiza que las pruebas solo pasen cuando se haya ejecutado un número específico de aserciones. 
Te ayudará a detectar casos en los que las pruebas terminan demasiado pronto. También harán que las pruebas fallen si se ejecutan demasiadas aserciones, lo que puede ser útil si tiene aserciones dentro de callbacks o bucles.

Si no especificas un plan de aserción, tu prueba aún fallará si no se ejecuta ninguna aserción. Debes establecer la opción `failWithoutAssertions` en` false` en la [configuración `package.json`] de AVA (./ 06-configuration.md) para deshabilitar este comportamiento.

Ten en cuenta que, a diferencia de [`tap`] (https://www.npmjs.com/package/tap) y [` tape`] (https://www.npmjs.com/package/tape), AVA *no* finaliza automáticamente una prueba cuando se alcanza el recuento de aserciones planificado.

Estos ejemplos darán como resultado una prueba exitosa:

```js
test('se resuelve a 3', t => {
	t.plan(1);

	return Promise.resolve(3).then(n => {
		t.is(n, 3);
	});
});

test.cb('invoca a un callback', t => {
	t.plan(1);

	someAsyncFunction(() => {
		t.pass();
		t.end();
	});
});
```

Estas no:

```js
test('el loop se ejecuta dos veces', t => {
	t.plan(2);

	for (let i = 0; i < 3; i++) {
		t.true(i < 3);
	}
}); // Falla, 3 aserciones son ejecutadas lo cual es demasiado

test('llama a un callback sincronicamente', t => {
	t.plan(1);

	someAsyncFunction(() => {
		t.pass();
	});
}); // Falla, la prueba falla sincronicamente antes de que la asercion sea ejecutada
```

## Omitiendo Aserciones

Cualquier aserción puede omitirse usando el modificador `skip`. 
Las afirmaciones omitidas todavía se cuentan, por lo que no es necesario cambiar el recuento de afirmaciones planificadas.

```js
test('omitir asercion', t => {
	t.plan(2);
	t.is.skip(foo(), 5); // No necesitas cambiar tu plan cuando estas omitiendo
	t.is(1, 1);
});
```

## Mensajes de afirmación mejorados

Habilitando [Babel] (./recipes/babel.md) también habilitará [`power-assert`] (https://github.com/power-assert-js/power-assert), brindándote mensajes de afirmación más descriptivos.

Tomemos este ejemplo, usando la [biblioteca `assert` estándar de Node](https://nodejs.org/api/assert.html):

```js
const a = /foo/;
const b = 'bar';
const c = 'baz';
require('assert').ok(a.test(b) || b === c);
```

Si lo pegas en un REPL de node, retornará:

```
AssertionError: false == true
```

Sin embargo, con la asercion de AVA "assert", esta prueba:

```js
test('aserciones mejoradas', t => {
	const a = /foo/;
	const b = 'bar';
	const c = 'baz';
	t.assert(a.test(b) || b === c);
});
```

La salida será:

```
6:   const c = 'baz';
7:   t.assert(a.test(b) || b === c);
8: });

Value is not truthy:

false

a.test(b) || b === c
=> false

b === c
=> false

c
=> 'baz'

b
=> 'bar'

a.test(b)
=> false

b
=> 'bar'

a
=> /foo/
```

## Aserciones personalizadas

Puedes usar cualquier biblioteca de aserciones en lugar o además de la incorporada, siempre que genere excepciones cuando la aserción falle.

Sin embargo, esto no te brindará una experiencia tan agradable como la que obtendrías con las [aserciones integradas](#built-in-assertions), y no podrás usar la [planificación de aserciones] (#assertion-planning) ([mira #25] (https://github.com/avajs/ava/issues/25)).

Tendrás que configurar AVA para que no falle las pruebas si no se ejecutan aserciones, porque AVA no puede decir si las aserciones personalizadas pasan. Establece la opción `failWithoutAssertions` en` false` en la [configuración `package.json`] de AVA (./06-configuration.md).

```js
const assert = require('assert');

test('custom assertion', t => {
	assert(true);
});
```

## Aserciones incorporadas

### `.pass(message?)`

Aserción exitosa.

### `.fail(message?)`

Aserción fallida.

### `.assert(value, message?)`

Verifica que `value` es verdadero. Esto es [`power-assert`](#enhanced-assertion-messages) habilitado.

### `.truthy(value, message?)`

Varifica que `value` is verdadero.

### `.falsy(value, message?)`

Verifica que `value` es falso.

### `.true(value, message?)`

Verifica que `value` es verdadero.

### `.false(value, message?)`

Verifica que `value` es falso.

### `.is(value, expected, message?)`

Verifica que `value` es igual a `expected`. Esta verificación se basa en [`Object.is()`](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/Object/is).

### `.not(value, expected, message?)`

Verifica que `value` no es igual a `expected`. Esta verificación se basa en [`Object.is()`](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/Object/is).

### `.deepEqual(value, expected, message?)`

Verifica que `value` tiene valores identicos a `expected`. Mira [Concordance](https://github.com/concordancejs/concordance) prara mas detalles. 
Funciona con [elemento de React y `react-test-renderer`](https://github.com/concordancejs/react).

### `.notDeepEqual(value, expected, message?)`

Verifica que `value` no tenga valores identicos a `expected`. Lo contrario de `.deepEqual()`.

### `.like(value, selector, message?)`

Verifica que `value` sea como `selector`. Es una variante de `.deepEqual()`, sin embargo `selector` 
no necesita tener las mias propiedades que `value` tiene.

En su lugar AVA deriva un objeto *comparable* a partir de `value`, basado en las propiedades anidadas identicas de `selector`. 
Este objeto es entonces comparado a `selector` haciendo uso de `.deepEqual()`.

Cualquiera de los valores en `selector` que no sean objetos regulares deberian ser identicamente iguales a los valores correspondientes en `value`.

En el siguiente ejemplo, la propiedad `map` de `value` debe ser identica a la de `selector`. 
Sin embargo, `nested.qux` es ignorado, puesto que no esta en el `selector`.

```js
t.like({
	map: new Map([['foo', 'bar']]),
	nested: {
		baz: 'thud',
		qux: 'quux'
	}
}, {
	map: new Map([['foo', 'bar']]),
	nested: {
		baz: 'thud',
	}
})
```

### `.throws(fn, expectation?, message?)`

Verifica que un error es retornado. `fn` debe ser una funcion que retorne un error. El valor retornado por la funcion *debe* ser un error. 
El error es retornado de forma que tu puedes ejecutar mas validaciones contra dicho error.

`expectation` puede ser un objeto con una o mas de las siguientes propiedades:

* `instanceOf`: un contructor, el error arrojado debe ser una instancia de
* `is`: el error arrojado debe ser estrictamente igual a `expectation.is`
* `message`: puede ser un string, el es comparado con el mensaje de error retornado, o una expresion regular, la cual es comparada con el mensaje
* `name`: un valor esperado para `.name` del error retornado
* `code`: un valor esperado para `.code` del error retornado

`expectation` no necesitas especificarlo. Si no lo necesitas pero deseas crear un mensaje de asercion debes espeficiar `null`.

Por ejemplo:

```js
const fn = () => {
	throw new TypeError('🦄');
};

test('throws', t => {
	const error = t.throws(() => {
		fn();
	}, {instanceOf: TypeError});

	t.is(error.message, '🦄');
});
```

### `.throwsAsync(thrower, expectation?, message?)`

Verifica que se produce un error. `thrower` puede ser una función asincrónica que debería arrojar el error, o una promesa que debería ser rechazada. Esta afirmación debe ser esperada a que se termine (awaited).

El valor arrojado *debe* ser un error. Se devuelve para que puedas ejecutar más verificaciones usandola.

`expectation` puede ser un objeto con una o más de las siguientes propiedades:

* `instanceOf`: un constructor, el error lanzado debe ser una instancia de
* `is`: el error arrojado debe ser estrictamente igual a `expectation.is`
* `message`: ya sea una cadena, que se compara con el mensaje de error arrojado, o una expresión regular, que se compara con este mensaje
* `name`: el valor esperado de `.name` del error lanzado
* `code`: el valor esperado de `.code` del error lanzado

`expectation` no es necesario especificarlo. Si no lo necesitas pero deseas establecer un mensaje de aserción, debe especificar `null`.

Ejemplo:

```js
test('throws', async t => {
	await t.throwsAsync(async () => {
		throw new TypeError('🦄');
	}, {instanceOf: TypeError, message: '🦄'});
});
```

```js
const promise = Promise.reject(new TypeError('🦄'));

test('promesa rechazada', async t => {
	const error = await t.throwsAsync(promise);
	t.is(error.message, '🦄');
});
```

### `.notThrows(fn, message?)`

Verifica que no se produce ningún error. `fn` debe ser una función que no debería arrojar errores.

### `.notThrowsAsync(nonThrower, message?)`

Verifica que no se produce ningún error. `nonThrower` puede ser una función asincrónica que no debería lanzar, o una promesa que debería resolverse exitosamente.

Al igual que la aserción `.throwsAsync ()`, debe esperar a que se complete la aserción:

```js
test('promesa resuelta exitosamente', async t => {
	await t.notThrowsAsync(promise);
});
```

### `.regex(contents, regex, message?)`

Verifica que `contents` coincida con `regex`.

### `.notRegex(contents, regex, message?)`

Verifica que `contents` no coincida con `regex`.

### `.snapshot(expected, message?)`
### `.snapshot(expected, options?, message?)`

Compara el valor `expected` con un snapshot registrado anteriormente. Los snapshots se almacenan para cada prueba, así que asegúrate de asignar títulos únicos a tus pruebas. Alternativamente, pasa un objeto `options` para seleccionar un snapshot específico, por ejemplo,` {id: 'my snapshot'} `.

Las verificaciones de los snapshots no se pueden omitir cuando se actualizan los snapshots.

### `.try(title?, implementation | macro | macro[], ...args?)`

`.try()` te permite *probar* aserciones sin hacer que la prueba falle.

La función de implementación se comporta igual que cualquier otra función de prueba. Incluso puedes usar macros. El primer argumento del título siempre es opcional. Los argumentos adicionales se pasan a la función de implementación o a la función macro.

`.try ()` es una función asíncrona. Debes esperar a que se resuelva `await`. El objeto de resultado tiene métodos `commit ()` y `discard ()`.  Debes decidir si haces `commit` o `discard` sobre el resultado. Si haces `commit` sobre un resultado fallido, su prueba fallará.

Puede comprobar si el intento se aprobó utilizando la propiedad `pass`. Cualquier error de afirmación está disponible a través de la propiedad `errors`. El título del intento está disponible a través de la propiedad `title`.

Los logs de `t.log ()` están disponibles a través de la propiedad `logs`. Puedes optar por conservar estos registros como parte de tu prueba pasando `{keepLogs: true}` a los métodos `commit ()` y `discard ()`.

La función de implementación recibe su propio [contexto de ejecución] (./02-execution-context.md), al igual que una función de prueba. Debe tener cuidado de realizar solo aserciones utilizando el contexto de ejecución del intento. Al menos una afirmación debe aprobarse para que su intento sea marcado como exitoso.

Puedes ejecutar varios intentos al mismo tiempo, dentro de una sola prueba. Sin embargo, no puedes usar snapshots cuando haces eso.

Ejemplo:

```js
const twoRandomIntegers = () => {
	const rnd = Math.round(Math.random() * 100);
	const x = rnd % 10;
	const y = Math.floor(rnd / 10);
	return [x, y];
};

test('flaky macro', async t => {
	const firstTry = await t.try((tt, a, b) => {
		tt.is(a, b);
	}, ...randomIntegers());

	if (firstTry.passed) {
		firstTry.commit();
		return;
	}

	firstTry.discard();
	t.log(firstTry.errors);

	const secondTry = await t.try((tt, a, b) => {
		tt.is(a, b);
	}, ...randomIntegers());
	secondTry.commit();
});
```
