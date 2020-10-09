# Contexto de ejecución (argumento `t`)

Cada prueba o hook es llamado en un contexto de ejecución. Por convención se llama `t`.

```js
const test = require('ava');

test('mi prueba exitosa', t => {
	t.pass();
});
```

Cada prueba o hook recibe un objeto diferente. Este objeto contiene [aserciones] (./ 03-assertions.md) así como tambien métodos y propiedades que se enumeran a continuación.

## `t.title`

Titulo de la prueba.

## `t.context`

Contiene el estado compartido de los hooks.

## `t.passed`

Si una prueba pasa. Este valor solamente tiene un valor correcto en los hooks `test.afterEach()` y `test.afterEach.always()`.

## `t.end()`

Finaliza la prueba. Funciona solamente con `test.cb()`.

## `t.log(...values)`

Registra los valores contextualmente junto con el resultado de la prueba en lugar de imprimirlos inmediatamente a "stdout". Se comporta como `console.log`, pero sin soporte para tokens de marcador de posición (placeholders).

## `t.plan(count)`

Planifica cuántas aserciones hay en la prueba. La prueba fallará si el recuento de aserciones real no coincide con el número de aserciones planificadas. Consulte [Planificación de afirmaciones](./03-assertions.md#assertion-planning).

## `t.teardown(fn)`

Registra la función `fn` que se ejecutará una vez finalizada la prueba. Puede registrar varias funciones y se ejecutarán en orden <sup>†</sup>. Puede utilizar funciones asincrónicas: solo se ejecutará una a la vez.

No puede realizar aserciones usando el objeto `t` o registrar funciones adicionales desde dentro de` fn`.

Tampoco puede usar `t.teardown ()` en los hooks.

<sup>†</sup> En la próxima versión de AVA, cambiaremos esto para que las funciones de desmontaje (teardown) se ejecuten en orden inverso. 
La última función registrada se llamará primero. Puede optar por este comportamiento ahora habilitando el experimento `reverseTeardowns`.

**`package.json`**:

```json
{
	"ava": {
		"nonSemVerExperiments": {
			"reverseTeardowns": true
		}
	}
}
```

**`ava.config.js`**:

```js
export default {
	nonSemVerExperiments: {
		reverseTeardowns: true
	}
}
```

## `t.timeout(ms)`

Establece un tiempo de espera para la ejecución de la prueba, en milisegundos. La prueba fallará si se excede este tiempo de espera. El tiempo de espera se restablece cada vez que se realiza una aserción.
