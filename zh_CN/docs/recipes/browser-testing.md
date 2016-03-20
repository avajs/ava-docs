___
**备注**

这是[browser-testing.md](https://github.com/sindresorhus/ava/blob/master/docs/recipes/browser-testing.md)的简体中文翻译。这个[链接](https://github.com/sindresorhus/ava/compare/master...zhaozhiming:master)用来查看本翻译与AVA的master分支是否有差别（如果你没有看到`browser-testing.md`发生变化，那就意味着这份翻译文档是最新的）。
___

# 设置AVA做浏览器测试

翻译: [Français](https://github.com/sindresorhus/ava-docs/blob/master/fr_FR/docs/recipes/browser-testing.md), [Русский](https://github.com/sindresorhus/ava-docs/blob/master/ru_RU/docs/recipes/browser-testing.md), [简体中文](https://github.com/sindresorhus/ava-docs/blob/master/zh_CN/docs/recipes/browser-testing.md)



AVA[还](https://github.com/sindresorhus/ava/issues/24)不支持在浏览器中运行测试。一些库要求浏览器指定全局变量（`window`, `document`, `navigator`等等）。
React就是其中的一个例子，最低要求如果你想用ReactDOM.render和用ReactTestUTils模拟事件。

这个秘方让需要模拟浏览器环境的库可以工作。

## 安装jsdom

安装[jsdom](https://github.com/tmpvar/jsdom)。

> 一个WHATWG DOM和HTML标准的JavaScript实现，给node.js使用的。

```
$ npm install --save-dev jsdom
```

## 设置jsdom

创建一个helper文件并放在`test/helpers`文件夹中，这样确保AVA不会把它当成测试来处理。

`test/helpers/setup-browser-env.js`:

```js
global.document = require('jsdom').jsdom('<body></body>');
global.window = document.defaultView;
global.navigator = window.navigator;
```

## 配置测试使用jsdom

配置AVA，将`require`设置为helper文件，这样每个测试运行前都会先加载它。

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

## 享受!

编写你的测试并享受一个模拟的window对象吧。

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
