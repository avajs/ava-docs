# Configurar AVA para ejecutar tests en el navegador

AVA no soporta ejecutar tests en el navegador, [todavía](https://github.com/avajs/ava/issues/24). Algunas librerías requieren globales especifícos para el navegador (`window`, `document`, `navigator`, etc).
Un ejemplo de esto es React, al menos si quieres usar ReactDOM.render y simular eventos con ReactTestUtils.

Esta guía funciona para cualquier librería que necesita un en torno de navegador simulado.

## Instalar jsdom

Instalar [jsdom](https://github.com/tmpvar/jsdom).

> Una implementación en JavaScript de los estándares WHATWG DOM y HTML, para usar con node.js

```
$ npm install --save-dev jsdom
```

## Configurar jsdom

Crea un archivo de ayuda y guárdalo en el directorio `test/helpers`. De esta forma AVA no lo considera como un test.

`test/helpers/setup-browser-env.js`:

```js
global.document = require('jsdom').jsdom('<body></body>');
global.window = document.defaultView;
global.navigator = window.navigator;
```

## Configurar tests para que usen jsdom

Configura AVA para que requiera (`require`) el archivo de ayuda antes de cada test.

`package.json`:

```json
{
  "ava": {
    "require": [
      "./test/helpers/setup-browser-env.js"
    ]
  }
}
```

## Disfruta!

Escribe tus tests y disfruta del objeto window simulado.

`test/my.react.test.js`:

```js
import test from 'ava';
import React from 'react';
import {render} from 'react-dom';
import {Simulate} from 'react-addons-test-utils';
import sinon from 'sinon';
import CustomInput from './components/custom-input.jsx';

test('Input calls onBlur', t => {
  const onUserBlur = sinon.spy();
  const input = render(
    React.createElement(CustomInput, onUserBlur),
    div
  );

  Simulate.blur(input);

  t.true(onUserBlur.calledOnce);
});
```
