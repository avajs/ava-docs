___
**备注**

这是 [readme.md](https://github.com/avajs/ava/blob/master/readme.md) 的简体中文翻译。这个[链接](https://github.com/avajs/ava/compare/67bf0472133041f59ef469f737b696de05ae316b...master#diff-0730bb7c2e8f9ea2438b52e419dd86c9) 用来查看本翻译与 AVA 的 master 分支是否有差别（如果你没有看到 `readme.md` 发生变化，那就意味着这份翻译文档是最新的）。
___

# <img src="media/header.png" title="AVA" alt="AVA logo" width="530">

[![Build Status](https://travis-ci.org/avajs/ava.svg?branch=master)](https://travis-ci.org/avajs/ava)  [![Coverage Status](https://codecov.io/gh/avajs/ava/branch/master/graph/badge.svg)](https://codecov.io/gh/avajs/ava/branch/master) [![XO code style](https://img.shields.io/badge/code_style-XO-5ed9c7.svg)](https://github.com/xojs/xo) [![Join the community on Spectrum](https://withspectrum.github.io/badge/badge.svg)](https://spectrum.chat/ava)
[![Mentioned in Awesome Node.js](https://awesome.re/mentioned-badge.svg)](https://github.com/sindresorhus/awesome-nodejs)

测试可能是一个累赘。但 AVA 可以帮助你完成它。AVA 是一个基于 Node.js 的的测试运行工具，通过提供简洁的 API，详尽的错误输出，拥抱新的语言特性和进程隔离的特性让你更高效地编写测试。所以你可以产出更多更好的代码。🚀

关注 [AVA 的 Twitter 账号](https://twitter.com/ava__js) 以获取更新。

如果你想参与贡献（issues / PRs 等），请翻阅我们的 [贡献指南](contributing.md)

![](media/mini-reporter.gif)


Translations: [Español](https://github.com/avajs/ava-docs/blob/master/es_ES/readme.md), [Français](https://github.com/avajs/ava-docs/blob/master/fr_FR/readme.md), [Italiano](https://github.com/avajs/ava-docs/blob/master/it_IT/readme.md), [日本語](https://github.com/avajs/ava-docs/blob/master/ja_JP/readme.md), [한국어](https://github.com/avajs/ava-docs/blob/master/ko_KR/readme.md), [Português](https://github.com/avajs/ava-docs/blob/master/pt_BR/readme.md), [Русский](https://github.com/avajs/ava-docs/blob/master/ru_RU/readme.md), [简体中文](https://github.com/avajs/ava-docs/blob/master/zh_CN/readme.md)


## 为什么选择 AVA？

- 轻量且高效
- 简单的测试语法
- 并行地运行测试
- 强制编写原子性的测试
- 没有隐式的全局变量
- 包含 TypeScript 的类型定义
- [Magic assert](#magic-assert)
- [Isolated environment for each test file](./docs/01-writing-tests.md#process-isolation)
- [使用最新的 JavaScript 语法编写你的测试](#latest-javascript-support)
- [支持 Promise](./docs/01-writing-tests.md#promise-support)
- [支持 Async 函数](./docs/01-writing-tests.md#async-function-support)
- [支持 Observable](./docs/01-writing-tests.md#observable-support)
- [Enhanced assertion messages](./docs/03-assertions.md#enhanced-assertion-messages)
- [在 CI 上自动的并发运行测试](#parallel-runs-in-ci)
- [TAP reporter](./docs/05-command-line.md#tap-reporter)


## Usage

要安装并配置 AVA，运行：

```console
npm init ava
```

你的 `package.json` 看起来将会像这样 (exact version notwithstanding)：

```json
{
	"name": "awesome-package",
	"scripts": {
		"test": "ava"
	},
	"devDependencies": {
		"ava": "^1.0.0"
	}
}
```

或者如果你偏好使用 Yarn：

```console
yarn add ava --dev
```

相应的你也可以手动安装 `ava`：

```console
npm install --save-dev ava
```

别忘记参照下文在你的 `package.json` 配置 `test` 要运行的脚本。

### 创建你的测试文件

在项目根目录创建 `test.js` 文件：

```js
import test from 'ava';

test('foo', t => {
	t.pass();
});

test('bar', async t => {
	const bar = Promise.resolve('bar');
	t.is(await bar, 'bar');
});
```

### 运行你的测试

```console
npm test
```

或者使用 `npx`:

```console
npx ava
```

通过 `--watch` flag 运行以启用 AVA 的 [watch 模式](docs/recipes/watch-mode.md)：

```console
npx ava --watch
```

## 所支持的 Node.js 版本

AVA supports the latest release of any major version that [is supported by Node.js itself](https://github.com/nodejs/Release#release-schedule). Read more in our [support statement](docs/support-statement.md).

## Highlights

### Magic assert

AVA adds code excerpts and clean diffs for actual and expected values. If values in the assertion are objects or arrays, only a diff is displayed, to remove the noise and focus on the problem. The diff is syntax-highlighted too! If you are comparing strings, both single and multi line, AVA displays a different kind of output, highlighting the added or missing characters.

![](media/magic-assert-combined.png)

### Clean stack traces

AVA automatically removes unrelated lines in stack traces, allowing you to find the source of an error much faster, as seen above.

### 最新的 JavaScript 支持

AVA 使用了 [Babel 7](https://babeljs.io)，所以你可以在你的测试中使用最新的 JavaScript 语法。无需额外的配置。你并不需要在为此你自己的项目中使用 Babel。

We aim to support all [finished syntax proposals](https://github.com/tc39/proposals/blob/master/finished-proposals.md), as well as all syntax from ratified JavaScript versions (e.g. ES2017). See our [`@ava/stage-4`](https://github.com/avajs/babel-preset-stage-4) preset for the currently supported proposals.

请注意我们没有修改或添加任何内建的方法，如果你在你的测试中使用 [`Object.fromEntries()`](https://github.com/tc39/proposal-object-from-entries)，由于 Node.js 10 不支持这个方法，这将会在 Node.js 10 下会崩溃。

你可以禁用这个语法支持，或者自定义 AVA 的 Babel pipeline。请查阅我们的 [Babel recipe] 了解更多信息。

### CI 并行构建

AVA 会自动检查你的 CI 环境是否支持并行构建。Each build will run a subset of all test files, while still making sure all tests get executed. 请翻阅 [`ci-parallel-vars`](https://www.npmjs.com/package/ci-parallel-vars) 包以获得所支持的 CI 环境列表。


## 文档

请翻阅 [files in the `docs` directory](./docs):

* [编写测试](./docs/01-writing-tests.md)
* [Execution context](./docs/02-execution-context.md)
* [断言](./docs/03-assertions.md)
* [Snapshot 测试](./docs/04-snapshot-testing.md)
* [Command line (CLI)](./docs/05-command-line.md)
* [配置](./docs/06-configuration.md)
* [测试超时](./docs/07-test-timeouts.md)

### Common pitfalls

We have a growing list of [common pitfalls](docs/08-common-pitfalls.md) you may experience while using AVA. If you encounter any issues you think are common, comment in [this issue](https://github.com/avajs/ava/issues/404).

### Recipes

- [Test setup](docs/recipes/test-setup.md)
- [代码覆盖率](docs/recipes/code-coverage.md)
- [Watch 模式](docs/recipes/watch-mode.md)
- [Endpoint 测试](docs/recipes/endpoint-testing.md)
- [何时使用 `t.plan()`](docs/recipes/when-to-use-plan.md)
- [浏览器测试](docs/recipes/browser-testing.md)
- [TypeScript](docs/recipes/typescript.md)
- [Flow](docs/recipes/flow.md)
- [配置 Babel][Babel recipe]
- [使用 ES modules](docs/recipes/es-modules.md)
- [向你的测试文件传递参数](docs/recipes/passing-arguments-to-your-test-files.md)
- [测试 React 组件](docs/recipes/react.md)
- [测试 Vue.js 组件](docs/recipes/vue.md)
- [JSPM 和 SystemJS](docs/recipes/jspm-systemjs.md)
- [使用 Chrome DevTools 调试测试](docs/recipes/debugging-with-chrome-devtools.md)
- [使用 VSCode 调试测试](docs/recipes/debugging-with-vscode.md)
- [使用 WebStorm 调试测试](docs/recipes/debugging-with-webstorm.md)
- [隔离的 MongoDB 集成测试](docs/recipes/isolated-mongodb-integration-tests.md)
- [使用 Puppeteer 测试 web 应用](docs/recipes/puppeteer.md)

## FAQ

### 为什么不选择 `mocha`, `tape`, `tap`?

Mocha requires you to use implicit globals like `describe` and `it` with the default interface (which most people use). It's not very opinionated and executes tests serially without process isolation, making it slow.

Tape and tap are pretty good. AVA is highly inspired by their syntax. They too execute tests serially. Their default [TAP](https://testanything.org) output isn't very user-friendly though so you always end up using an external tap reporter.

In contrast AVA is highly opinionated and runs tests concurrently, with a separate process for each test file. Its default reporter is easy on the eyes and yet AVA still supports TAP output through a CLI flag.

### 名字如何拼写和发音?

AVA, 不是 Ava 或 ava. 读作 [`/ˈeɪvə/`](media/pronunciation.m4a?raw=true): Ay (f**a**ce, m**a**de) V (**v**ie, ha**v**e) A (comm**a**, **a**go)

### 头部的背景图是什么?

It's the [Andromeda galaxy](https://simple.wikipedia.org/wiki/Andromeda_galaxy).

### What is the difference between concurrency and parallelism?

[Concurrency is not parallelism. It enables parallelism.](https://stackoverflow.com/q/1050222)

## 支持

- [Stack Overflow](https://stackoverflow.com/questions/tagged/ava)
- [Spectrum](https://spectrum.chat/ava)
- [Twitter](https://twitter.com/ava__js)

## 相关

- [eslint-plugin-ava](https://github.com/avajs/eslint-plugin-ava) - Lint rules for AVA tests
- [sublime-ava](https://github.com/avajs/sublime-ava) - Snippets for AVA tests
- [atom-ava](https://github.com/avajs/atom-ava) - Snippets for AVA tests
- [vscode-ava](https://github.com/samverschueren/vscode-ava) - Snippets for AVA tests
- [gulp-ava](https://github.com/avajs/gulp-ava) - Run tests with gulp
- [grunt-ava](https://github.com/avajs/grunt-ava) - Run tests with grunt
- [More…](https://github.com/avajs/awesome-ava#packages)

## 链接

- [AVA stickers, t-shirts, etc](https://www.redbubble.com/people/sindresorhus/works/30330590-ava-logo)
- [Awesome list](https://github.com/avajs/awesome-ava)
- [AVA Casts](http://avacasts.com)
- [More…](https://github.com/avajs/awesome-ava)

## 团队

[![Mark Wubben](https://github.com/novemberborn.png?size=100)](https://github.com/novemberborn) | [![Sindre Sorhus](https://github.com/sindresorhus.png?size=100)](https://github.com/sindresorhus) | [![Vadim Demedes](https://github.com/vadimdemedes.png?size=100)](https://github.com/vadimdemedes)
---|---|---
[Mark Wubben](https://novemberborn.net) | [Sindre Sorhus](http://sindresorhus.com) | [Vadim Demedes](https://github.com/vadimdemedes)

###### Former

- [Kevin Mårtensson](https://github.com/kevva)
- [James Talmage](https://github.com/jamestalmage)
- [Juan Soto](https://github.com/sotojuan)
- [Jeroen Engels](https://github.com/jfmengels)


<div align="center">
	<br>
	<br>
	<br>
	<a href="https://ava.li">
		<img src="media/logo.svg" width="200" alt="AVA">
	</a>
	<br>
	<br>
</div>

[Babel recipe]: docs/recipes/babel.md
