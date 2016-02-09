___
**Nota del traductor**

Esta es la traducción del archivo [when-to-use-plan.md](https://github.com/sindresorhus/ava/blob/master/docs/recipes/when-to-use-plan.md). Aquí hay un [enlace](https://github.com/sindresorhus/ava/compare/4111f9483f2ff6a158b603735a712eee3ab074c4...master#diff-0c25d982e94d600cb6b8e438a0e67169) a las diferencias con la rama master de AVA (Si al hacer clic en el enlace no se encuentran modificaciones en el archivo `when-to-use-plan.md`, será por que la traducción está actualizada).
___
# Cuándo utilizar `t.plan()`

Traducciones: [Français](https://github.com/sindresorhus/ava-docs/blob/master/fr_FR/docs/recipes/when-to-use-plan.md),
[Español](https://github.com/sindresorhus/ava-docs/blob/master/es_ES/docs/recipes/when-to-use-plan.md), [Português](https://github.com/sindresorhus/ava-docs/blob/master/pt_BR/docs/recipes/when-to-use-plan.md)

Una gran diferencia entre AVA y [`tap`](https://github.com/tapjs/node-tap)/[`tape`](https://github.com/substack/tape) es el comportamiento de 't.plan()'. En AVA, 't.plan()' se utiliza para afirmar que el número esperado de aserciones son llamadas; esto no auto termina el test.

## Pobre uso de 't.plan()'

Muchos usuarios trasicionan de 'tap'/'cinta' están acostumbrados a usar 't.plan()' prolíficamente en cada test. Sin embargo en AVA no consideramos que sea una "una buena práctica". Por el contrario, creemos que 't.plan()' debe usarse en situaciones donde proporciona algo de valor.

### Pruebas de sincronización con ninguna ramificación

't.plan()' es innecesario en la mayoría de tests de sincronización.

```js
test(t => {
	// MAL: no hay ninguna ramificación aquí - t.plan() no tiene sentido
	t.plan(2);

	t.is(1 + 1, 2);
	t.is(2 + 2, 4);
});
```

't.plan()' no proporciona ningún valor y crea una tarea extra si alguna vez decide añadir o quitar las aserciones.

### Promesas que esperan resolverse

```js
test(t => {
	t.plan(1);

	return somePromise().then(result => {
		t.is(result, 'foo');
	});
});
```

De un vistazo, este test parece hacer un buen uso de 't.plan()' desde un controlador de promesa asíncrona está involucrado. Sin embargo hay varios problemas con la prueba:

1. 't.plan()' presumiblemente se utiliza aquí para proteger contra la posibilidad de que 'somePromise()' pueda ser rechazada; Pero devolviendo una promesa rechazada no pasa el test de todos modos.

2. Sería mejor aprovechar `async`/`await`:

```js
test(async t => {
	t.is(await somePromise(), 'foo');
});
```

## Buenos usos de 't.plan()'

`t.plan()` tiene muchos usos aceptables.

### Promesas con un bloque de '.catch()'

```js
test(t => {
	t.plan(2);

	return shouldRejectWithFoo().catch(reason => {
		t.is(reason.message, 'Hello') // Prefieren t.throws() si todo lo que le importa es el mensaje
		t.is(reason.foo, 'bar');
	});
});
```

Aquí, 't.plan()' se utiliza para que el código en el bloque 'catch' ocurre. En la mayoría de los casos, prefiere la aserción de 't.throws()', pero es aceptable el uso de 't.throws()', sólo le permite hacer la aserción contra la propiedad del 'mensaje' de error.

### Garantizar que la instrucción catch sucede

```js
test(t => {
	t.plan(2);

	try {
		shouldThrow();
	} catch (err) {
		t.is(err.message, 'Hello') // Prefieren t.throws() si todo lo que le importa es el mensaje
		t.is(err.foo, 'bar');
	}
});
```

Tal como se indica en el ejemplo anterior, usando 't.throws()', la afrimación `try`/`catch` es generalmente una mejor opción, sólo le permite hacer la aserción contra la propiedad del 'mensaje' de error.

### Garantizar que los callbacks de llamadas múltiple se invocan

```js
test.cb(t => {
	t.plan(2);

	const callbackA = () => {
		t.pass();
		t.end();
	};

	const callbackB = () => t.pass();

	bThenA(callbackA, callbackB);
});
```

Lo anterior asegura que 'callbackB' se llama primero (y solamente una vez), seguido por 'callbackA'. Cualquier otra combinación no satisfaría el plan.

### Pruebas con declaraciones de ramificación

En la mayoría de los casos, no es buena idea utilizar cualquier compleja ramificación dentro de los tests. Una excepción notable es para tests que son generados automáticamente (tal vez de un documento JSON). A continuación 't.plan()' se utiliza para asegurar la corrección de la JSON de entrada:

```js
const testData = require('./fixtures/test-definitions.json');

testData.forEach(testDefinition => {
	test(t => {
		const result = functionUnderTest(testDefinition.input);

		// testDefinition debe tener una expectativa de 'foo' o 'bar' pero no ambos
		t.plan(1);

		if (testDefinition.foo) {
			t.is(result.foo, testDefinition.foo);
		}

		if (testDefinition.bar) {
			t.is(result.bar, testDefinition.foo);
		}
	});
});
```

## Conclusiones

't.plan()' tiene un montón de usos válidos, pero no debe ser utilizado indiscriminadamente. Una buena regla es usarlo en cualquier momento en su *test* que no tenga sencillo o fácil de razonar acerca del flujo de código. Tests con aserciones dentro de callbacks, declaraciones `if`/`then`, loops `for`/`while` y (en algunos casos) en bloques `try`/`catch`, son todos buenos candidatos para 't.plan()'.
