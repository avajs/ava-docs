___
**Nota del traductor**

Esta es la traducción del archivo [react.md](https://github.com/avajs/ava/blob/master/react.md). Aquí hay un [enlace](https://github.com/avajs/ava/compare/8e2f3dca177a4283ad882596d3c1425cabb998ef...master#diff-60cce07a584082115d230f2e3d571ad6) a las diferencias con la rama master de AVA (Si al hacer click en el enlace no se encuentran modificaciones en el archivo `react.md`, será por que la traducción está actualizada).
___
# Testeando componentes de React

Traducciones: [Español](https://github.com/avajs/ava-docs/blob/master/es_ES/docs/recipes/react.md), [Français](https://github.com/avajs/ava-docs/blob/master/fr_FR/docs/recipes/react.md)

## Configurando Babel

El primer paso es configurar Babel para transiplar el código JSX de los tests añadiendo una sección para ava en tu `package.json`:

```json
{
  "ava": {
    "require": ["babel-register"],
    "babel": "inherit"
  },
  "babel": {
    "presets": ["react"]
  }
}
```

Puedes encontrar más información sobre configurar Babel con AVA en la [receta babelrc](babelrc.md).

## Usando [Enzyme](https://github.com/airbnb/enzyme)

Primero veamos cómo utilizar AVA con una de las librerías de tests más populares para React: [Enzyme](https://github.com/airbnb/enzyme).

Si sólo pretendes usar [renderizado superficial de componentes](https://facebook.github.io/react/docs/test-utils.html#shallow-rendering), no necesitas ninguna configuración adicional.

Primero instala [los paquetes necesarios para Enzyme](https://github.com/airbnb/enzyme/#installation):

```console
$ npm install --save-dev enzyme react-addons-test-utils react-dom
```

Entonces puedes usar Enzyme directamente:

```js
import test from 'ava';
import React from 'react';
import { shallow } from 'enzyme';

const Foo = ({children}) =>
	<div className="Foo">
		<span className="bar">bar</span>
		{children}
		<span className="bar">bar</span>
	</div>;

Foo.propTypes = {
	children: React.PropTypes.any
};

test('tiene la clase `.Foo`', t => {
	const wrapper = shallow(<Foo/>);
	t.true(wrapper.hasClass('Foo'));
});

test('renderiza dos `.Bar`', t => {
	const wrapper = shallow(<Foo/>);
	t.is(wrapper.find('.bar').length, 2);
});

test('renderiza los hijos que se le pasen', t => {
	const wrapper = shallow(
		<Foo>
			<div className="unique"/>
		</Foo>
	);
	t.true(wrapper.contains(<div className="unique"/>));
});
```

Enzyme también tiene un helper `mount` y un `render` para testear en un entorno real de navegador. Si quieres usar esos helpers, tendrás que configurar un entorno de navegador. Échale un vistazo a la [receta de cómo hacer tests en un navegador](https://github.com/avajs/ava/blob/master/docs/recipes/browser-testing.md) para ver cómo hacerlo.

Para ver un ejemplo de uso de AVA junto con Enzyme configurado para tests en navegador, échale un vistazo a este [proyecto de ejemplo](https://github.com/adriantoine/ava-enzyme-demo).

Éste es un ejemplo básico de cómo integrar Enzyme con AVA. Para más información sobre Enzyme para testear componentes de React, échale un vistazo a la [documentación de Enzyme](http://airbnb.io/enzyme/).

## Utilizando helpers de JSX

Otro enfoque para testear componentes de React es usar el paquete [`react-element-to-jsx-string`](https://github.com/algolia/react-element-to-jsx-string) para comparar árboles DOM como cadenas. [`jsx-test-helpers`](https://github.com/MoOx/jsx-test-helpers) es una buena librería para gestionar el [renderizado superficial de componentes](https://facebook.github.io/react/docs/test-utils.html#shallow-rendering) y convertir JSX en strings para testear componentes de React utilizando aseveraciones ("asserts") de AVA.

```console
$ npm install --save-dev jsx-test-helpers
```

Ejemplo de uso:

```js
import test from 'ava';
import React from 'react';
import { renderJSX, JSX } from 'jsx-test-helpers';

const Foo = ({children}) =>
	<div className="Foo">
		<span className="bar">bar</span>
		{children}
		<span className="bar">bar</span>
	</div>;

Foo.propTypes = {
	children: React.PropTypes.any
};

test('renderiza correctamente el markup', t => {
	const actual = renderJSX(<Foo/>);
	const expected = JSX(
		<div className="Foo">
			<span className="bar">bar</span>
			<span className="bar">bar</span>
		</div>
	);
	t.is(actual, expected);
});

test('renderiza los hijos que se le pasen', t => {
	const actual = renderJSX(
		<Foo>
			<div className="unique"/>
		</Foo>
	);
	const expected = JSX(
		<div className="Foo">
			<span className="bar">bar</span>
			<div className="unique"/>
			<span className="bar">bar</span>
		</div>
	);
	t.is(actual, expected);
});
```

Nota el uso de variables como `actual` y `expected` porque [`power-assert` no gestiona el JSX correctamente](https://github.com/power-assert-js/power-assert/issues/34).

Éste es un ejemplo básico de cómo usar `jsx-test-helpers` con AVA. Para un uso más avanzado de esta librería, échale un vistazo a [este fichero de tests con anotaciones](https://github.com/MoOx/jsx-test-helpers/blob/master/src/__tests__/index.js).

[Este proyecto de ejemplo](https://github.com/MoOx/jsx-test-helpers) muestra un uso básico y con una configuración mínima de AVA con `jsx-test-helpers`.

## Utilizando otras librerías de aseveración

En AVA, puedes usar cualquier librería de aseveraciones, y ya hay unas cuentas ahí fuera para testear componentes de React. He aquí una lista de librerías de aseveraciones que combinan bien con AVA:

- [`expect-jsx`](https://github.com/algolia/expect-jsx) ([Ejemplo](https://github.com/avajs/ava/issues/186#issuecomment-161317068))
- [`unexpected-react`](https://github.com/bruderstein/unexpected-react) ([Proyecto de muestra con un ejemplo de output](https://github.com/adriantoine/ava-unexpected-react-demo))

## Referencias

- [Guía a fondo sobre cómo configurar AVA con cobertura ("coverage") de código en un proyecto con React](https://github.com/kentcdodds/react-ava-workshop)
