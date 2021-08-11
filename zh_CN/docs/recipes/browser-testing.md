___
**备注**

这是 [browser-testing.md](https://github.com/avajs/ava/blob/main/docs/recipes/browser-testing.md) 的简体中文翻译。这个[链接](https://github.com/avajs/ava/compare/8e2f3dca177a4283ad882596d3c1425cabb998ef...master#diff-9d3d394077fa7f97cbbb0fefc098ac60) 用来查看本翻译与 AVA 的 master 分支是否有差别（如果你没有看到`browser-testing.md`发生变化，那就意味着这份翻译文档是最新的）。
___

# 设置 AVA 做浏览器测试

翻译：[Français](https://github.com/avajs/ava-docs/blob/master/fr_FR/docs/recipes/browser-testing.md), [Italiano](https://github.com/avajs/ava-docs/blob/master/it_IT/docs/recipes/browser-testing.md) [Русский](https://github.com/avajs/ava-docs/blob/master/ru_RU/docs/recipes/browser-testing.md), [简体中文](https://github.com/avajs/ava-docs/blob/master/zh_CN/docs/recipes/browser-testing.md)



AVA [还](https://github.com/avajs/ava/issues/24)不支持在浏览器中运行测试。一些库需要浏览器提供的全局变量（`window`, `document`, `navigator`等等）。
React 就是其中的一个例子，至少当你想用 ReactDOM.render 和用 ReactTestUTils 模拟事件。

这个秘方让需要模拟浏览器环境的库可以工作。

## 安装 browser-env

> **❗️ 重点提示**
>
>`browser-env` 将 `jsdom` 里浏览器环境上的属性添加到 Node.js 的全局环境。这是明确的[违背了](https://github.com/tmpvar/jsdom/wiki/Don't-stuff-jsdom-globals-onto-the-Node-global) `jsdom` 的推荐行为。请读完链接里的 wiki 页面并确保你明白了其中的注意事项。如果你只是需要一个并不包涵太多依赖的浏览器环境，那也许 [`window`](https://github.com/lukechilds/window#universal-testing-pattern) 是个更好的解决方案。

安装 [browser-env](https://github.com/lukechilds/browser-env)。

> 使用 jsdom 模拟一个全局的浏览器环境。

```
$ npm install --save-dev browser-env
```

## 设置 browser-env

在 `test/helpers` 目录中创建一个 helper 文件。这样确保 AVA 不会把它当成测试来处理。

`test/helpers/setup-browser-env.js`:

```js
import browserEnv from 'browser-env';
browserEnv();
```

## 配置测试使用 browser-env

配置 AVA，通过 `require` 那个 helper 文件使得每个测试运行前都加载它。

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

## 享受！

编写你的测试并享受一个模拟的 window 对象吧。

`test/my.dom.test.js`:

```js
import test from 'ava';

test('Insert to DOM', t => {
	const div = document.createElement('div');
	document.body.appendChild(div);

	t.is(document.querySelector('div'), div);
});
```

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
