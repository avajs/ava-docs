___
**备注**

这是 [readme.md](https://github.com/sindresorhus/ava/blob/master/readme.md) 的简体中文翻译。这个[链接](https://github.com/sindresorhus/ava/compare/67bf0472133041f59ef469f737b696de05ae316b...master#diff-0730bb7c2e8f9ea2438b52e419dd86c9) 用来查看本翻译与 AVA 的 master 分支是否有差别（如果你没有看到`readme.md`发生变化，那就意味着这份翻译文档是最新的）。
___

# ![AVA](https://github.com/sindresorhus/ava/blob/master/media/header.png)

> 未来的测试运行器

[![Build Status: Linux](https://travis-ci.org/sindresorhus/ava.svg?branch=master)](https://travis-ci.org/sindresorhus/ava) [![Build status: Windows](https://ci.appveyor.com/api/projects/status/igogxrcmhhm085co/branch/master?svg=true)](https://ci.appveyor.com/project/sindresorhus/ava/branch/master) [![Coverage Status](https://coveralls.io/repos/sindresorhus/ava/badge.svg?branch=master&service=github)](https://coveralls.io/github/sindresorhus/ava?branch=master) [![Gitter](https://img.shields.io/badge/Gitter-Join_the_AVA_chat_%E2%86%92-00d06f.svg)](https://gitter.im/sindresorhus/ava)

虽然 JavaScript 是单线程，但在 Node.js 里由于其异步的特性使得 IO 可以并行。AVA 利用这个优点让你的测试可以并发执行，这对于 IO 繁重的测试特别有用。另外，测试文件可以在不同的进程里并行运行，让每一个测试文件可以获得更好的性能和独立的环境。在 Pageres 项目中从 Mocha[切换](https://github.com/sindresorhus/pageres/commit/663be15acb3dd2eb0f71b1956ef28c2cd3fdeed0) 到 AVA 让测试时间从 31 秒下降到 11 秒。测试并发执行强制你写原子测试，意味着测试不需要依赖全局状态或者其他测试的状态，这是一件非常好的事情。

*如果你想贡献（问题、PRs 等），请先阅读我们的[贡献向导](contributing.md)。*

关注 [AVA 的 Twitter 账号](https://twitter.com/ava__js) 以获取最新信息。

翻译：[Español](https://github.com/sindresorhus/ava-docs/blob/master/es_ES/readme.md), [Français](https://github.com/sindresorhus/ava-docs/blob/master/fr_FR/readme.md), [日本語](https://github.com/sindresorhus/ava-docs/blob/master/ja_JP/readme.md), [Português](https://github.com/sindresorhus/ava-docs/blob/master/pt_BR/readme.md), [简体中文](https://github.com/sindresorhus/ava-docs/blob/master/zh_CN/readme.md)

## 目录

- [用法](#用法)
- [CLI 用法](#cli)
- [配置](#配置)
- [文档](#文档)
- [API](#api)
- [断言](#断言)
- [小贴士](#小贴士)
- [FAQ](#faq)
- [秘方](#秘方)
- [支持](#支持)
- [相关](#相关)
- [链接](#链接)
- [团队](#团队)


## 为什么要用 AVA?

- 轻量和高效
- 简单的测试语法
- 并发运行测试
- 强制编写原子测试
- 没有隐藏的全局变量
- [为每个测试文件隔离环境](#隔离进程)
- [用 ES2015 编写测试](#支持es2015)
- [支持 Promise](#支持promise)
- [支持 Generator](#支持generator)
- [支持 Async](#支持async)
- [支持 Observable](#支持observable)
- [强化断言信息](#强化断言信息)
- [可选的 TAP 输出显示](#可选的tap输出)
- [简明的堆栈跟踪](#简明的堆栈跟踪)


## 测试语法

```js
import test from 'ava';

test(t => {
    t.same([1, 2], [1, 2]);
});
```


## 用法

### 在项目中添加 AVA

通过带`--init`参数运行 AVA 全局安装命令，将会添加 AVA 到`package.json`：

```console
$ npm install --global ava
$ ava --init
```

```json
{
    "name": "awesome-package",
    "scripts": {
        "test": "ava"
    },
    "devDependencies": {
        "ava": "^0.11.0"
    }
}
```

写在`--init`后面的任何参数都会被添加到`package.json`。

#### 手动安装

你也可以直接安装 AVA：
```console
$ npm install --save-dev ava
```

你还需要配置`test`脚本在你的`package.json`，值为`ava`（参照上面的配置）。

### 创建你的测试文件

在你的工程根目录下创建一个名字为`test.js`的文件：

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

<img src="https://github.com/sindresorhus/ava/blob/master/screenshot.png" width="150" align="right">

### 运行

```console
$ npm test
```

### 观察

```console
$ npm test -- --watch
```

AVA 来自一个非常聪明的观察模式。[可以在它的秘方中查看详情](docs/recipes/watch-mode.md)。

## CLI

![](https://github.com/sindresorhus/ava/blob/master/screenshot-mini-reporter.gif)

```console
$ ava --help

  Usage
    ava [<file|directory|glob> ...]

  Options
    --init           Add AVA to your project
    --fail-fast      Stop after first test failure
    --serial, -s     Run tests serially
    --require, -r    Module to preload (Can be repeated)
    --tap, -t        Generate TAP output
    --verbose, -v    Enable verbose output
    --no-cache       Disable the transpiler cache
    --match, -m      Only run tests with matching title (Can be repeated)',
    --watch, -w      Re-run tests when tests and source files change
    --source, -S     Pattern to match source files so tests can be re-run (Can be repeated)

  Examples
    ava
    ava test.js test2.js
    ava test-*.js
    ava test
    ava --init
    ava --init foo.js

  Default patterns when no arguments:
  test.js test-*.js test/**/*.js
```

*注意，如果你本地安装的 AVA 可用的话 CLI 将会运行它，即使你的全局也安装了 AVA。*

文件夹会被递归遍历，带上`*.js`参数的话全部文件都会被作为测试文件。名字为`fixtures`，`helpers`和`node_modules`的文件夹*总会*被忽略。所以把 helper 名字以`_`开头命名就可以一起放置在测试文件的目录下。

当使用`npm test`时你可以直接传参数`npm test test2.js`，但标志需要像这样传递`npm test -- --verbose`。


## 配置

所有的 CLI 选项都可以配置在`package.json`的`ava`属性中。你可以修改`ava`命令的默认行为，所以你不需要重复在命令行中输入相同的选项。

```json
{
  "ava": {
    "files": [
      "my-test-folder/*.js",
      "!**/not-this-file.js"
    ],
    "source": [
      "**/*.{js,jsx}",
      "!dist/**/*"
    ],
    "match": [
      "*oo",
      "!foo"
    ],
    "failFast": true,
    "tap": true,
    "require": [
      "babel-register"
    ],
    "babel": "inherit"
  }
}
```

传递给 CLI 的参数总是比`package.json`中的配置优先级高。

请看[支持ES2015](#支持es2015) 章节以了解`babel`选项的更多详情。

## 文档

测试是并发执行的，你可以选择同步或异步执行测试，在返回一个 promise 或 [observable](https://github.com/zenparsing/zen-observable) 时你才需要考虑使用同步。

我们*强烈*推荐使用 [async 函数](#支持async)，它让异步代码简洁更具可读性，并且它隐式返回一个 promise 让你无需手动创建。

如果你不能使用 promise 或者 observales，你可以通过这样定义你的测试`test.cb([title], fn)`来启用“callback 模式”。通过这种方式声明的测试*必须*手动添加`t.end()`来结束，这种模式的主要目的是为了测试 callback 方式的 API。

你必须同时将所有测试都定义为同步，它们不能定义在`setTimeout`，`setImmediate`等里面。

测试文件在当前文件夹中被执行，所以 [`process.cwd()`](https://nodejs.org/api/process.html#process_process_cwd) 和 [`__dirname`](https://nodejs.org/api/globals.html#globals_dirname) 总是相同。你可以使用相对路径代替`path.join(__dirname, 'relative/path')`操作。

### 创建测试

你可以通过调用 AVA 中导入的`test`方法来创建一个测试。提供可选的标题和 callback 函数，函数将在你运行测试时被调用。它会传递一个[执行对象](#t) 作为其第一个且唯一的一个参数。按照惯例这个参数名字为`t`。

```js
import test from 'ava';

test('my passing test', t => {
    t.pass();
});
```

#### 标题

标题是可选的，意味着你可以这样：

```js
test(t => {
    t.pass();
});
```

如果你有多个测试的话建议写上测试标题。

如果你没有提供测试标题，但 callback 是一个有名字的函数，那这个名字将作为测试标题：

```js
test(function name(t) {
    t.pass();
});
```

### 断言计划

断言计划确保测试只能通过指定次数的断言，它们可以帮你捕捉测试太早退出的情况，如果太多断言被执行的话它们也会让测试失败，如果在你的 callback 或循环中有断言的话断言计划会比较有用。

请注意：不像 [`tap`](https://www.npmjs.com/package/tap) 和 [`tape`](https://www.npmjs.com/package/tape)，当断言计划数达到了指定数量 AVA 并*不*会自动结束。

这些例子将会通过测试：

```js
test(t => {
    t.plan(1);

    return Promise.resolve(3).then(n => {
        t.is(n, 3);
    });
});

test.cb(t => {
    t.plan(1);

    someAsyncFunction(() => {
        t.pass();
        t.end();
    });
});
```

这些则不会：

```js
test(t => {
    t.plan(2);

    for (let i = 0; i < 3; i++) {
        t.true(i < 3);
    }
}); // 失败，3 个断言被执行，但计划只有 2 个

test(t => {
    t.plan(1);

    someAsyncFunction(() => {
        t.pass();
    });
}); // 失败，在断言执行之前测试会因为同步而提前结束。
```

### 串行运行测试

测试默认是并发运行的，这很棒。但有时你必须写不是并发运行的测试。

这种情况比较少见，但你可以使用`.serial`修饰符，它将强制让那些测试在并发的测试前*先*串行运行。

```js
test.serial(t => {
    t.pass();
});
```

注意这只影响一个特定测试文件里面的测试，AVA 将仍然同时运行多个测试文件，除非你传递 [`--serial` CLI 标志](#cli)。

### 运行指定的测试

在开发中只运行少量指定的测试非常有用，这可以通过使用`.only`修饰符来完成。

```js
test('will not be run', t => {
    t.fail();
});

test.only('will be run', t => {
    t.pass();
});
```

`.only` 影响所有测试文件，所以你在一个文件里面使用了它，那其他测试文件里的测试将不会运行。

### 运行匹配标题的测试

`--match`标志允许你只运行包含匹配标题的测试，这可以通过简单的通配模式来做到，模式是大小写敏感的，详情请看 [`matcher`](https://github.com/sindresorhus/matcher)。

匹配以`foo`结尾的标题：

```console
$ ava --match='*foo'
```

匹配以`foo`开头的标题：

```console
$ ava --match='foo*'
```

匹配包含了`foo`的标题：

```console
$ ava --match='*foo*'
```

匹配*精确*等于`foo`的标题（尽管大小写敏感）：

```console
$ ava --match='foo'
```

匹配不包含了`foo`的标题：

```console
$ ava --match='!*foo*'
```

匹配以`foo`开头并且以`bar`结束的标题：

```console
$ ava --match='foo*bar'
```

匹配以`foo`开头或者以`bar`结束的标题：

```console
$ ava --match='foo*' --match='*bar'
```

注意，匹配模式优先级高于`.only`修饰符，只有带明确标题的测试可以被匹配，当使用`--match`时，没有标题的或者标题是从 callback 函数名来的测试都会被跳过。

下面是当使用一个匹配模式`*oo*`来匹配测试的结果：

```js
test('foo will run', t => {
    t.pass();
});

test('moo will also run', t => {
    t.pass();
});

test.only('boo will run but not exclusively', t => {
    t.pass();
});

// 不会运行，没有标题
test(function (t) {
    t.fail();
});

// 不会运行，没有明确定义的标题
test(function foo(t) {
    t.fail();
});
```

### 跳过测试

有时候失败的测试一时难以修复，你可以使用`.skip`来告诉 AVA 跳过这些测试。它们仍然会显示在输出结果中（标识为 skipped），但不会被运行。

```js
test.skip('will not be run', t => {
    t.fail();
});
```

你必须指定 callback 函数。

### 测试占位符 ("todo")

当你计划写一个测试的时候你可以使用`.todo`修饰符，像跳过测试一样这些占位符也会显示在输出结果中，它们只要求一个标题，你不能指定 callback 函数。

```js
test.todo('will think about writing this later');
```

### Before & after 钩子

AVA 让你可以注册在测试之前和之后跑的钩子，这允许你执行 setup 和 teardow 代码。

`test.before()`在你的测试文件中注册了一个钩子并在第一个测试前运行，同样地，`test.after()`注册了一个钩子在最后一个测试后运行。

`test.beforeEach()`在你的测试文件中注册了一个钩子并在每个测试前运行，同样地，`test.afterEach()`注册了一个钩子在每个测试后运行。

像`test()`这些方法一样接收一个可选的标题和一个 callback 函数，如果你的钩子失败了标题就会显示出来，callback 函数传递一个[执行对象](#t)。

`before`在`beforeEach`之前执行，`afterEach`在`after`之前执行，同一类的钩子按照它们定义的顺序来执行。

```js
test.before(t => {
    // 这个会在所有测试前运行
});

test.before(t => {
    // 这个会在上面的方法后面运行，但在测试之前运行
});

test.after('cleanup', t => {
    // 这个会在所有测试之后运行
});

test.beforeEach(t => {
    // 这个会在每个测试之前运行
});

test.afterEach(t => {
    // 这个会在每个测试之后运行
});

test(t => {
    // 正常的测试
});
```

钩子可以同步或异步，就像测试一样。让钩子异步返回一个 promise 或 observable，使用一个 async 函数，或者通过`test.cb.before()`，`test.cb.beforeEach()`等来启用 callback 模式。

```js
test.before(async t => {
    await promiseFn();
});

test.after(t => {
    return new Promise(/* ... */);
});

test.cb.beforeEach(t => {
    setTimeout(t.end);
});

test.afterEach.cb(t => {
    setTimeout(t.end);
});
```

请记住，`beforeEach`和`afterEach`钩子在一个测试之前和之后运行，并且默认情况下测试是并发运行的，如果你需要为每个测试设置一个全局的状态（比如`console.log`的 spying[例子](https://github.com/sindresorhus/ava/issues/560)），你需要确保这些测试是[串行运行](#串行运行测试)的。

记住，AVA 运行每个测试文件会有各自单独的进程，你可能不需要在`after`钩子中清理全局状态，因为它只会在进程退出前被调用。

`beforeEach`和`afterEach`钩子可以共享测试的上下文：

```js
test.beforeEach(t => {
    t.context.data = generateUniqueData();
});

test(t => {
    t.is(t.context.data + 'bar', 'foobar');
});
```

默认情况下`t.context`是一个对象，但你可以重新赋值：

```js
test.beforeEach(t => {
    t.context = 'unicorn';
});

test(t => {
    t.is(t.context, 'unicorn');
});
```

上下文共享在`before`和`after`钩子中*不*可用。

### 连接测试修饰符

你可以以任何顺序使用`.serial`，`.only`和`.skip`，与`test`，`before`，`after`，`beforeEach`和`afterEach`一起，比如：

```js
test.before.skip(...);
test.skip.after(...);
test.serial.only(...);
test.only.serial(...);
```

这意味着你可以在每个测试或钩子的末尾临时添加`.skip`或`.only`，而不用做其他的修改。

### 自定义断言

你可以使用其他断言库来替代内置的断言库，当断言失败可以让其抛出异常。

但这样做的话你将得不到[内置断言库](#断言)的良好体验，同时你也将不会用到[断言计划](#断言计划） (『看#25](https://github.com/sindresorhus/ava/issues/25))。

```js
import assert from 'assert';

test(t => {
    assert(true);
});
```

### 支持ES2015

AVA 通过 [Babel 6](https://babeljs.io) 内置支持 ES2015，只需要用 ES2015 的方式写你测试，不需要额外的配置。你可以在你的工程中使用任何的 Babel 版本，我们使用我们捆绑的 Babel，带有 [`es2015`](https://babeljs.io/docs/plugins/preset-es2015/) 和 [`stage-2`](https://babeljs.io/docs/plugins/preset-stage-2/) 设置，和 [`espower`](https://github.com/power-assert-js/babel-plugin-espower) 和 [`transform-runtime`](https://babeljs.io/docs/plugins/transform-runtime/) 插件一样。

类似的 Babel 配置同样适用于 AVA，如下：

```json
{
  "presets": [
    "es2015",
    "stage-2",
  ],
  "plugins": [
    "espower",
    "transform-runtime"
  ]
}
```

你可以自定义 AVA 如何通过 [`package.json`配置](#配置)`babel`选项来转换测试文件，例如你可以这样来覆盖配置：

```json
{
  "ava": {
    "babel": {
      "presets": [
        "es2015",
        "stage-0",
        "react"
      ]
    }
  },
}
```

你也可以使用特殊的`"inherit"`关键字，这让 AVA 遵从你的 [`.babelrc`或`package.json`文件](https://babeljs.io/docs/usage/babelrc/) 中的 Babel 配置，这种方式让测试文件的转换方式和与源文件的相同，可以无需在 AVA 中重复配置。

 ```json
{
    "babel": {
        "presets": [
            "es2015",
            "stage-0",
            "react"
        ]
    },
    "ava": {
        "babel": "inherit",
    },
}
```

注意，AVA*总是*应用 [`espower`](https://github.com/power-assert-js/babel-plugin-espower) 和 [`transform-runtime`](https://babeljs.io/docs/plugins/transform-runtime/) 插件。

### 支持 TypeScript

AVA 支持 TypeScript，你必须自己配置转换规则，当你在你的`tsconfig.json`文件中把`module`设为`commonjs`，TypeScipt 将自动为 AVA 找到类型定义。你应该把`target`设置为`es2015`来使用 promises 和 async 函数。

### 转换导入模块

AVA 现在只转换需要运行的测试，*它不会转换那些在测试中你`import`的模块*，这可能不是你所期望的但这就是现在的工作方案。

如果你使用 Babel 你可以使用它的 [require 钩子](https://babeljs.io/docs/usage/require/) 来实时转换导入模块，运行 AVA 带上`--require babel-register` （请看 [CLI](#cli)) 参数或[在你的`package.json`里配置](#配置)。

你也可以在另外一个进程里转换你的模块，并且参考你的转换文件而不是你的测试源码。

### 支持Promise

如果你在测试里返回一个 promise，你不需要在测试里明确的结束测试，当 promise resolve 的时候它会自己结束。

```js
test(t => {
    return somePromise().then(result => {
        t.is(result, 'unicorn');
    });
});
```

### 支持Generator

AVA 自带对 [generator 函数](https://developer.mozilla.org/zh-CN/docs/Web/JavaScript/Reference/Statements/function*) 的内置支持。

```js
test(function * (t) {
    const value = yield generatorFn();
    t.true(value);
});
```

### 支持Async

AVA 自带对 [async functions](https://tc39.github.io/ecmascript-asyncawait/) *(async/await)*的内置支持。

```js
test(async function (t) {
    const value = await promiseFn();
    t.true(value);
});

// 异步箭头函数
test(async t => {
    const value = await promiseFn();
    t.true(value);
});
```

### 支持Observable

AVA 自带对 [observables](https://github.com/zenparsing/es-observable) 的内置支持。如果你从测试中返回一个 observable，AVA 会自动消费它使其在测试结束前完成。

*你不需要使用“callback 模式"或者调用`t.end()`。*

```js
test(t => {
    t.plan(3);
    return Observable.of(1, 2, 3, 4, 5, 6)
        .filter(n => {
            // 只有奇数
            return n % 2 === 0;
        })
        .map(() => t.pass());
});
```

### 支持 Callback

当使用 node-style, error-first 等 callback API 时，AVA 支持使用`t.end`作为最后一个 callback 函数。AVA 将把任何为真的值传递给`t.end`其实变成一个 error。注意，`t.end`要求“callback 模式”，这个可以通过使用`test.cb`链来启用。

```js
test.cb(t => {
    // t.end 自动检查第一个参数是否为错误
    fs.readFile('data.txt', t.end);
});
```

### 可选的TAP输出

AVA 可以通过`--tap`选项来生成 TAP 的输出，可以选择任意的 [TAP 报告](https://github.com/sindresorhus/awesome-tap#reporters)。

```console
$ ava --tap | tap-nyan
```

<img src="https://github.com/sindresorhus/ava/blob/master/media/tap-output.png" width="398">

### 简明的堆栈跟踪

AVA 会在堆栈跟踪信息里面自动移除不相关的行，让你更快地找到错误的原因。

<img src="https://github.com/sindresorhus/ava/blob/master/media/stack-traces.png" width="300">

## API

### `test([title], callback)`
### `test.serial([title], callback)`
### `test.cb([title], callback)`
### `test.only([title], callback)`
### `test.skip([title], callback)`
### `test.todo(title)`
### `test.before([title], callback)`
### `test.after([title], callback)`
### `test.beforeEach([title], callback)`
### `test.afterEach([title], callback)`

#### `title`

类型：`string`

测试标题

#### `callback(t)`

类型：`function`

应该包含实际的测试。

##### `t`

类型：`object`

特定测试的执行对象，每个测试的 callback 接收到一个不同的对象，包含[断言](#断言） ，`.plan(count)`和`.end()`等方法。`t.context`可以包含`beforeEach`钩子中的共享状态。

###### `t.plan(count)`

计划有多少断言将在测试中被执行，如果实际断言的数量没有匹配计划数，那么测试将失败，详情请见[断言计划](#断言计划)。

###### `t.end()`

结束测试，只在`test.cb()`中有效。

## 断言

断言也被包含在[执行对象](#t) 中，可以提供给每个测试 callback：

```js
test(t => {
    t.ok('unicorn'); // 断言
});
```

如果单个测试中有多个断言同时失败了，那 AVA 只会显示*第一个*。

### `.pass([message])`

测试通过。

### `.fail([message])`

断言失败。

### `.ok(value, [message])`

断言`value`是否是真值。

### `.notOk(value, [message])`

断言`value`是否是假值。

### `.true(value, [message])`

断言`value`是否是`true`。

### `.false(value, [message])`

断言`value`是否是`false`。

### `.is(value, expected, [message])`

断言`value`是否和`expected`相等。

### `.not(value, expected, [message])`

断言`value`是否和`expected`不等。

### `.same(value, expected, [message])`

断言`value`是否和`expected`深度相等。

### `.notSame(value, expected, [message])`

断言`value`是否和`expected`深度不等。

### `.throws(function|promise, [error, [message]])`

断言`function`抛出一个异常，或者`promise` reject 一个错误。

`error`可以是一个构造器，正则，错误信息或者验证函数。

返回由`function`抛出的异常或`promise`的拒绝原因。

### `.notThrows(function|promise, [message])`

断言`function`没有抛出一个异常，或者`promise` resolve。

### `.regex(contents, regex, [message])`

断言`contents`匹配`regex`。

### `.ifError(error, [message])`

断言`error`是假值。

### 跳过断言

通过`skip`修饰符可以跳过任何断言，跳过的断言仍然会被计数，所以不需要去改变你的断言计划数量。

```js
test(t => {
    t.plan(2);
    t.skip.is(foo(), 5); // 当跳过时不需要改变你的计划数
    t.is(1, 1);
});
```

### 强化断言信息

AVA 自带内置的 [`power-assert`](https://github.com/power-assert-js/power-assert)，给你更多的描述性断言信息，它阅读你的测试代码并从中试图推断出更多信息。

我们来举个例子，使用 Node 的标准 [`断言`库](https://nodejs.org/api/assert.html)。

```js
const a = /foo/;
const b = 'bar';
const c = 'baz';
require('assert').ok(a.test(b) || b === c);
```

如果你把代码粘贴到 Node REPL 中会返回：

```
AssertionError: false == true
```

而在 AVA 中，这个测试：

```js
test(t => {
    const a = /foo/;
    const b = 'bar';
    const c = 'baz';
    t.ok(a.test(b) || b === c);
});
```

将会输出：

```
t.ok(a.test(b) || b === c)
       |    |     |     |
       |    "bar" "bar" "baz"
       false
```

## 隔离进程

每个测试文件都会在一个独立的 Node 进程中运行，这样让你可以在一个测试文件中改变全局状态或将其覆盖一个内置的全局状态，而不会影响其他测试文件。这样更加有效地利用现代的多核处理器，让多个测试可以并发地执行。

## 小贴士

### 临时文件

并发运行测试面临着许多挑战，文件的 IO 操作就是其中一个。

一般来讲，串行测试在当前文件夹中创建临时文件夹，然后会在测试结束的时候清理它们。但这种做法在并发测试中不起作用，因为测试会一起操作这个文件夹而导致冲突。正确的做法是为每个测试创建一个新的临时文件夹，使用 [`tempfile`](https://github.com/sindresorhus/tempfile) 和 [`temp-write`](https://github.com/sindresorhus/temp-write) 模块可能会有帮助。

### 调试

AVA 默认情况下是并发执行测试，这样在调试信息时并不是最理想的，你可以通过设置`--serial`选项来让测试串行执行。

```console
$ ava --serial
```

### 代码覆盖率

你不能使用 [`istanbul`](https://github.com/gotwarlost/istanbul) 来做代码覆盖率因为 AVA[处理过这些测试文件](#隔离进程），你可以使用 [`nyc`](https://github.com/bcoe/nyc) 来代替，你可以把它看做是支持子进程的`istanbul`。

从版本`5.0.0`开始，在报告覆盖率时为你的代码使用原生映射，而不是转换后的代码。确保你测试的代码包括了一个内联的原生映射或者引用了一个原生映射文件。如果你使用了`babel-register`，你可以在你的 Babel 配置中将`sourceMaps`设置为`inline`。

## FAQ

### 为什么不用`mocha`，`tape`，`tap`？

Mocha 要求你使用隐式全局变量比如`describe`和`it`作为其默认接口（这是大部分人使用的）。这样做不是很好，并且串行执行测试没有进程隔离，使得测试十分缓慢。

Tape 和 tap 是非常好的。AVA 在它们的语法中得到大量启发，但它们也是串行执行测试，它们的默认 [TAP](https://testanything.org) 输出不是非常友好，因此你总是需要使用额外的 tap 报告。

与它们不同的是，AVA 可以并发执行测试，为每个测试文件提供独立进程，它的默认报告简单明了，并且 AVA 也支持通过 CLI 标志来输出一个 TAP 报告。

### 我如何使用自定义的报告？

AVA 支持 TAP 格式，所以它兼容任何 [TAP 报告](https://github.com/sindresorhus/awesome-tap#reporters)，使用 [`--tap`标志](#可选的tap输出)来启用 TAP 输出。

### 项目名字要怎么写才是正确的？如何发音？

AVA，不是 Ava，也不是 ava，发音 [`/ˈeɪvə/` ay-və](https://github.com/sindresorhus/ava/blob/master/media/pronunciation.m4a?raw=true)。

### 项目背景图片是什么意思？

它是[处女座星系](https://zh.wikipedia.org/wiki/%E4%BB%99%E5%A5%B3%E5%BA%A7%E6%98%9F%E7%B3%BB)。

### 并发和并行有什么不同？

[并发不是并行，并发可以并行。](https://stackoverflow.com/q/1050222)

## 秘方

- [代码覆盖率](docs/recipes/code-coverage.md)
- [观察模式](docs/recipes/watch-mode.md)
- [端点测试](docs/recipes/endpoint-testing.md)
- [什么时候使用`t.plan()`](docs/recipes/when-to-use-plan.md)
- [浏览器测试](docs/recipes/browser-testing.md)
- [TypeScript](docs/recipes/typescript.md)

## 支持

- [Stack Overflow](https://stackoverflow.com/questions/tagged/ava)
- [Gitter chat](https://gitter.im/sindresorhus/ava)
- [Twitter](https://twitter.com/ava__js)

## 相关

- [sublime-ava](https://github.com/sindresorhus/sublime-ava) - AVA 测试的代码片段
- [atom-ava](https://github.com/sindresorhus/atom-ava) - AVA 测试的代码片段
- [vscode-ava](https://github.com/samverschueren/vscode-ava) - AVA 测试的代码片段
- [eslint-plugin-ava](https://github.com/sindresorhus/eslint-plugin-ava) - AVA 测试的代码规则
- [gulp-ava](https://github.com/sindresorhus/gulp-ava) - 用 gulp 运行测试
- [grunt-ava](https://github.com/sindresorhus/grunt-ava) - 用 grunt 运行测试
- [fly-ava](https://github.com/pine613/fly-ava) - 用 fly 运行测试
- [start-ava](https://github.com/start-runner/ava) - 用 start 运行测试

[更多...](https://github.com/sindresorhus/awesome-ava#packages)

## 链接

- [购买 AVA 贴纸](https://www.stickermule.com/user/1070705604/stickers)
- [Awesome 列表](https://github.com/sindresorhus/awesome-ava)

## 团队

[![Sindre Sorhus](https://avatars.githubusercontent.com/u/170270?s=130)](http://sindresorhus.com) | [![Vadim Demedes](https://avatars.githubusercontent.com/u/697676?s=130)](https://github.com/vdemedes) | [![James Talmage](https://avatars.githubusercontent.com/u/4082216?s=130)](https://github.com/jamestalmage) | [![Mark Wubben](https://avatars.githubusercontent.com/u/33538?s=130)](https://novemberborn.net)
---|---|---|---|---
[Sindre Sorhus](http://sindresorhus.com) | [Vadim Demedes](https://github.com/vdemedes) | [James Talmage](https://github.com/jamestalmage) | [Mark Wubben](https://novemberborn.net)

### 前任

- [Kevin Mårtensson](https://github.com/kevva)

<div align="center">
    <br>
    <br>
    <br>
    <img src="https://cdn.rawgit.com/sindresorhus/ava/fe1cea1ca3d2c8518c0cc39ec8be592beab90558/media/logo.svg" width="200" alt="AVA">
    <br>
    <br>
</div>
