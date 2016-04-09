___
**Примечание от переводчика**

Здесь содержится перевод файла [browser-testing.md](https://github.com/sindresorhus/ava/blob/master/docs/recipes/browser-testing.md). Если при нажатии на [ссылку](https://github.com/sindresorhus/ava/compare/8e2f3dca177a4283ad882596d3c1425cabb998ef...master#diff-9d3d394077fa7f97cbbb0fefc098ac60), содержащую последние правки в master ветке AVA, Вы не можете найти файл `browser-testing.md`, то можете считать что перевод актуален.
___
# Настройка AVA для тестирования браузера

Переводы: [Français](https://github.com/sindresorhus/ava-docs/blob/master/fr_FR/docs/recipes/browser-testing.md), [Italiano](https://github.com/sindresorhus/ava-docs/blob/master/it_IT/recipes/browser-testing.md), [Русский](https://github.com/sindresorhus/ava-docs/blob/master/ru_RU/docs/recipes/browser-testing.md)

AVA не поддерживает запуск тестов в браузере, [пока что](https://github.com/sindresorhus/ava/issues/24). Некоторым библиотекам необходимы браузерное глобальное окружение(`window`, `document`, `navigator`, и т.д.).
Один из примеров - это React, по крайней мере, если Вы собираетесь использовать ReactDOM.render и эмулировать DOM события с ReactTestUtils.

Этот пример работает с любыми библиотеками, которые моделируют браузерное окружение.

## Установка jsdom

Установите [jsdom](https://github.com/tmpvar/jsdom).

> JavaScript реализация WHATWG DOM и HTML стандартов, для использования с node.js

```
$ npm install --save-dev jsdom
```

## Настройка jsdom

Создайте вспомогательный файл и поместите его в каталог `test/helpers`. В этом случае, AVA не будет рассматривать файл в качестве теста.

`test/helpers/setup-browser-env.js`:

```js
global.document = require('jsdom').jsdom('<body></body>');
global.window = document.defaultView;
global.navigator = window.navigator;
```

## Сконфигурируйте тесты с использованием jsdom

Сконфигурируйте AVA `require` для подключения вспомогательного файла перед каждым тестовым файлом.

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

## Наслаждайтесь!

Напишите Ваши тесты и наслаждайтесь смоделируемым `window` объектом.

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
		React.createElement(CustomInput, {onUserBlur),
		div
	)

	Simulate.blur(input);

	t.true(onUserBlur.calledOnce);
});
```
