# ![AVA](media/header.png)

> Футуристичный запускатор тестов

[![Build Status: Linux](https://travis-ci.org/sindresorhus/ava.svg?branch=master)](https://travis-ci.org/sindresorhus/ava) [![Build status: Windows](https://ci.appveyor.com/api/projects/status/igogxrcmhhm085co/branch/master?svg=true)](https://ci.appveyor.com/project/sindresorhus/ava/branch/master) [![Coverage Status](https://coveralls.io/repos/sindresorhus/ava/badge.svg?branch=master&service=github)](https://coveralls.io/github/sindresorhus/ava?branch=master) [![Gitter](https://img.shields.io/badge/Gitter-Join_the_AVA_chat_%E2%86%92-00d06f.svg)](https://gitter.im/sindresorhus/ava)

Even though JavaScript is single-threaded, IO in Node.js can happen in parallel due to its async nature. AVA takes advantage of this and runs your tests concurrently, which is especially beneficial for IO heavy tests. In addition, test files are run in parallel as separate processes, giving you even better performance and an isolated environment for each test file. [Switching](https://github.com/sindresorhus/pageres/commit/663be15acb3dd2eb0f71b1956ef28c2cd3fdeed0) from Mocha to AVA in Pageres brought the test time down from 31 sec to 11 sec. Having tests run concurrently forces you to write atomic tests, meaning tests don't depend on global state or the state of other tests, which is a great thing!

*Прочтите наш [гайд участника](contributing.md) если вы хотите внести вклад в проект(проблемы/пулл реквесты/etc).*

Подпишитесь на [Твиттер аккаунт AVA](https://twitter.com/ava__js) для отслеживания обновлений.

Переводы: [Español](https://github.com/sindresorhus/ava-docs/blob/master/es_ES/readme.md), [Français](https://github.com/sindresorhus/ava-docs/blob/master/fr_FR/readme.md), [日本語](https://github.com/sindresorhus/ava-docs/blob/master/ja_JP/readme.md), [Português](https://github.com/sindresorhus/ava-docs/blob/master/pt_BR/readme.md)


## Содержание

- [Использование](#Использование)
- [Командная строка](#Командная-строка)
- [Конфигурация](#Конфигурация)
- [Документация](#Документация)
- [API](#api)
- [Сравнения](#Сравнения)
- [FAQ](#faq)
- [Рецепты](#Рецепты)


## Почему AVA?

- Минимально и быстро
- Простой синтаксис тестов
- Запуск тестов конкурентно
- Атомарные тесты
- Нет явных globals
- [Изолированное окружение для каждого теста](#Изолированное-окружение-для-каждого-теста)
- [Поддержка ES2015](#es2015-support)
- [Поддержка обещаний](#promise-support)
- [Поддержка генераторов](#generator-function-support)
- [Поддержка асинхронных функций](#Поддержка-асинхронных-функций)
- [Поддержка Observable типов](#observable-support)
- [Улучшенные assert'ы](#enhanced-asserts)
- [Опциональный TAP вывод](#optional-tap-output)
- [Чистый трейс вызовов](#clean-stack-traces)


## Синтаксис теста

```js
import test from 'ava';

test(t => {
	t.same([1, 2], [1, 2]);
});
```


## Использование

#### Initialize

Установите AVA глобально `$ npm install --global ava` или выполните `$ ava --init` (с любыми аргументами) для добавления AVA в Ваш файл package.json или создания нового.

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

#### Создайте тестовый файл

Создайте тестовый файл `test.js` в корневом каталоге Вашего проекта:

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

<img src="screenshot.png" width="150" align="right">

#### Запуск

```
$ npm test
```


## Командная строка

```
$ ava --help

  Usage
    ava [<file|folder|glob> ...]

  Options
    --init           Add AVA to your project
    --fail-fast      Stop after first test failure
    --serial, -s     Run tests serially
    --require, -r    Module to preload (Can be repeated)
    --tap, -t        Generate TAP output
    --verbose, -v    Enable verbose output
    --no-cache       Disable the transpiler cache

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

Каталоги по умолчанию рекурсивны. Каталоги с названием `fixtures` и `helpers` игнорируются, также как и те, что начинаются с `_`. Это весьма полезно, если Ваши хелперы находятся в том же каталоге, что и тестовые файлы.

Когда используется команда `npm test`, Вы можете передать дополнительные аргументы `npm test test2.js`, но флаги должны быть переданы вот так - `npm test -- --verbose`.

*ВНИМАНИЕ - Нестандартное поведение:* Командная строка AVA CLI всегда будет искать локальную установку AVA в Ваших проектах. Поведение сохраняется даже при использовании глобальной команды `ava`. Это нестандартное поведение решает важную [проблему](https://github.com/sindresorhus/ava/issues/157), и не должно влиять на повседневное использование.

## Конфигурация

Все опции командной строки могут быть отконфигурированны в секции `ava` вашего `package.json` файла. Это позволяет изменять стандартное поведение команды `ava`, так что Вам не нужно будет постоянно вбивать одни и те же опци в командную строку.

```json
{
  "ava": {
    "files": [
      "my-test-folder/*.js",
      "!**/not-this-file.js"
    ],
    "failFast": true,
    "serial": true,
    "tap": true,
    "verbose": true,
    "require": [
      "babel-core/register"
    ]
  }
}
```

Аргументы переданные в командную строку всегда будут иметь приоритет перед конфигурацией в `package.json` файле.

## Документация

Тесты запускаются асинхронно и требуют, чтобы был возвращен объект, поддерживающий асинхронность(обещание, или [observable](https://github.com/zenparsing/zen-observable)). Мы *сильно* рекомендуем использовать [асинхронные функции](#Поддержка-асинхронных-функций); Они делают асинхронный код кратким и читабельным, неявно возвращая Обещания.

Если в коде Вы не возвращаете объект, поддерживающий асинхронность, тест будет считаться синхронным и завершится незамедлительно.

Если использовать обещания или любой другой объект, поддерживающий асинхронность, Вы можете включить "коллбек мод" таким образом `test.cb([title], fn)`. Все тесты, которые написаны с "коллбек модом" должны заканчиваться с `t.end()`. Этот режим в основном предназначен для тестирование API в коллбек стиле.

Вы должны сделать все тесты синхронными. Они не могут быть описаны внутри `setTimeout`, `setImmediate` и т.д.

Все тестовые файлы запускаются из текущего каталога, так что [`process.cwd()`](https://nodejs.org/api/process.html#process_process_cwd) тот же, что и [`__dirname`](https://nodejs.org/api/globals.html#globals_dirname). Вы можете использовать относительные пути вместо `path.join(__dirname, 'relative/path')`.

### Анатомия тестов

Для создания теста, Вы можете вызвать функцию `test` которую Вы импортировали `require` из AVA и передать опциональное имя теста и функцию, содержажую выполнение теста. Переданной функции задается контекст первым аргументом, так что Вы можете вызывать методы AVA и [сравнения](#Сравнения).

```js
test('name', t => {
	t.pass();
});
```

### Опциональное имя теста

Именование теста - опционально, но мы рекомендуем использовать именование, если у Вас больше, чем один тест.

```js
test(t => {
	t.pass();
});
```

Вы также можете выбрать именную функцию:

```js
test(function name(t) {
	t.pass();
});
```

### План сравнений

План сравнений необходим для того, чтобы убедиться, что заданное количество сравнений будет выполнено. В большинстве случаев, он гарантирует, что тест не прервется до того, как ожидаемое количество сравнений выполнится. План может провалить тест, если будет выполнено большое число сравнений, это удобно, если сравнения выполняются в коллбеке или цикле.

В результате этот тест завершится успешно:

```js
test(t => {
	t.plan(1);

	return Promise.resolve(3).then(n => {
		t.is(n, 3);
	});
});

test.cb(t => {
	setTimeout(() => {
		t.pass();
		t.end();
	}, 100);
});
```

#### ВНИМАНИЕ: последние изменения

AVA больше не поддерживает автоматическое завершение тестов через `t.plan(...)`. Это помогает избежать ложных срабатываний, если Вы добавили сравнения, но забыли увеличить ожидаемое количество сравнений в `t.plan(...)`.

```js
// Это больше не работает

test('auto ending is dangerous', t => {
	t.plan(2);

	t.pass();
	t.pass();

	// После достижений запланированных сравнений этот блок не сработает
	setTimeout(() => t.fail(), 10000);
});
```

Для того, чтобы пример выше сработал, Вы должны использовать "коллбек мод" и явно вызвать `t.end()`.

```js
test.cb('explicitly end your tests', t => {
	t.plan(2);

	t.pass();
	t.pass();

	setTimeout(() => {
		// Ошибка здесь будет обработана
		t.fail();
		t.end();
	}, 1000);
});
```

### Последовательные тесты

Конкурентность - это прекрасно, но есть некоторые вещи, которые не могут выполняться конкурентно. В редких случаях, Вы можете вызвать `test.serial`, для запуска последовательных тестов.

```js
test.serial(t => {
	t.pass();
});
```

### Конкретные тесты

Конкретные тесты указывают тесты для выполнения. Это может быть удобно для запуска определенных тестов во время разработки.

```js
test('will not be run', t => {
	t.fail();
})

test.only('will be run', t => {
	t.pass();
});
```

### Пропуск тестов

Пропущенные тесты показываются в выводе, но никогда не выполняются.

```js
test.skip('will not be run', t => {
	t.fail();
});
```

### До & после хуки

When setup and/or teardown is required, you can use `test.before()` and `test.after()`,
used in the same manner as `test()`. The test function given to `test.before()` and `test.after()` is called before/after all tests. You can also use `test.beforeEach()` and `test.afterEach()` if you need setup/teardown for each test. Hooks are run serially in the test file. Add as many of these as you want. You can optionally specify a title that is shown on failure.


```js
test.before(t => {
	// this runs before all tests
});

test.before(t => {
	// this runs after the above, but before tests
});

test.after('cleanup', t => {
	// this runs after all tests
});

test.beforeEach(t => {
	// this runs before each test
});

test.afterEach(t => {
	// this runs after each test
});

test(t => {
	// regular test
});
```

You may use async functions, return async objects, or enable "callback mode" in any of the hooks.

```js
test.before(async t => {
	await promiseFn();
});

test.cb.beforeEach(t => {
	setTimeout(t.end);
});

test.afterEach.cb(t => {
	setTimeout(t.end);
});

test.after(t => {
   return new Promise(/* ... */);
});
```

The `beforeEach` & `afterEach` hooks can share context with the test:

```js
test.beforeEach(t => {
	t.context.data = generateUniqueData();
});

test(t => {
	t.is(t.context.data + 'bar', 'foobar');
});
```

The context is by default an object, but it can also be directly assigned:

```js
test.beforeEach(t => {
	t.context = 'unicorn';
});

test(t => {
	t.is(t.context, 'unicorn');
});
```

### Chaining test modifiers

You can chain test modifiers together in the following ways:

```js
test.before.skip([title], testFn);
test.skip.after(....);
test.serial.only(...);
test.only.serial(...);
```

This is especially helpful temporarily using `skip` or `only` on a test, without losing the information and behavior the other modifiers provide.

### Custom assertion module

You can use any assertion module instead or in addition to the one that comes with AVA, but you won't be able to use the `.plan()` method, [yet](https://github.com/sindresorhus/ava/issues/25).

```js
import assert from 'assert';

test(t => {
	assert(true);
});
```

### ES2015 support

AVA comes with builtin support for ES2015 through [Babel 6](https://babeljs.io). Just write your tests in ES2015. No extra setup needed. You can use any Babel version in your project. We use our own bundled Babel with the [`es2015`](http://babeljs.io/docs/plugins/preset-es2015/) and [`stage-2`](http://babeljs.io/docs/plugins/preset-stage-2/) presets.

#### Transpiling Imported Modules

AVA currently only transpiles the tests you ask it to run. *It will not transpile modules you ```import``` from outside of the test.* While there are valid reasons for taking this approach, it may not be what you expect!

As a simple workaround, you can use [Babel's require hook](https://babeljs.io/docs/usage/require/) in order to do on-the-fly transpiling of modules that are subsequently imported. Because AVA supports ES2015 module syntax, you can use it to import the require hook itself:

```js
import test from 'ava';
import 'babel-core/register';
import foo from './foo'; // <-- foo can be written in ES2015!

test('foo bar', t => {
	t.same('baz', foo('bar'));
});
```

[#111](https://github.com/sindresorhus/ava/issues/111) is tracking this item as a potential enhancement.

### Promise support

If you return a promise in the test you don't need to explicitly end the test as it will end when the promise resolves.

```js
test(t => {
	return somePromise().then(result => {
		t.is(result, 'unicorn');
	});
});
```

### Generator function support

AVA comes with builtin support for [generator functions](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Statements/function*).

```js
test(function * (t) {
	const value = yield generatorFn();
	t.true(value);
});
```

### Async function support

AVA comes with builtin support for [async functions](https://tc39.github.io/ecmascript-asyncawait/) *(async/await)*.

```js
test(async function (t) {
	const value = await promiseFn();
	t.true(value);
});

// async arrow function
test(async t => {
	const value = await promiseFn();
	t.true(value);
});
```

### Observable support

AVA comes with builtin support for [observables](https://github.com/zenparsing/es-observable).
If you return an observable from a test, AVA will automatically consume it to completion before ending the test.

*You do not need to use "callback mode" or call `t.end()`.*

```js
test(t => {
	t.plan(3);
	return Observable.of(1, 2, 3, 4, 5, 6)
		.filter(n => {
			// only even numbers
			return n % 2 === 0;
		})
		.map(() => t.pass());
});
```

### Callback support

AVA supports using `t.end` as the final callback when using node-style error-first callback APIs. AVA will consider any truthy value passed as the first argument to `t.end` to be an error. Note that `t.end` requires "callback mode", which can be enabled by using the `test.cb` chain.

```js
test.cb(t => {
	// t.end automatically checks for error as first argument
	fs.readFile('data.txt', t.end);
});
```

### Optional TAP output

AVA can generate TAP output via `--tap` option for use with any [TAP reporter](https://github.com/sindresorhus/awesome-tap#reporters).

```
$ ava --tap | tap-nyan
```

<img src="media/tap-output.png" width="398">


### Clean stack traces

AVA automatically removes unrelated lines in stack traces, allowing you to find the source of an error much faster.

<img src="media/stack-traces.png" width="300">


## API

### test([title], body)
### test.serial([title], body)
### test.cb([title], body)
### test.only([title], body)
### test.skip([title], body)
### test.before([title], body)
### test.after([title], body)
### test.beforeEach([title], body)
### test.afterEach([title], body)

#### title

Type: `string`

Test title.

#### body(context)

Type: `function`

Should contain the actual test.

##### context

Passed into the test function and contains the different AVA methods and [assertions](#assertions).

###### .plan(count)

Plan how many assertion there are in the test. The test will fail if the actual assertion count doesn't match planned assertions.

###### .end()

End the test. Only works with `test.cb()`.


## Assertions

Assertions are mixed into the test [context](#context):

```js
test(t => {
	t.ok('unicorn'); // assertion
});
```

If multiple assertion failures are encountered within a single test, AVA will only display the *first* one.

### .pass([message])

Passing assertion.

### .fail([message])

Failing assertion.

### .ok(value, [message])

Assert that `value` is truthy.

### .notOk(value, [message])

Assert that `value` is falsy.

### .true(value, [message])

Assert that `value` is `true`.

### .false(value, [message])

Assert that `value` is `false`.

### .is(value, expected, [message])

Assert that `value` is equal to `expected`.

### .not(value, expected, [message])

Assert that `value` is not equal to `expected`.

### .same(value, expected, [message])

Assert that `value` is deep equal to `expected`.

### .notSame(value, expected, [message])

Assert that `value` is not deep equal to `expected`.

### .throws(function|promise, [error, [message]])

Assert that `function` throws an error or `promise` rejects.

`error` can be a constructor, regex, error message or validation function.

### .notThrows(function|promise, [message])

Assert that `function` doesn't throw an `error` or `promise` resolves.

### .regex(contents, regex, [message])

Assert that `contents` matches `regex`.

### .ifError(error, [message])

Assert that `error` is falsy.

## Skipping Assertions

Any assertion can be skipped using the `skip` modifier. Skipped assertions are still counted, so there is no need to change your planned assertion count.

```js
test(t => {
	t.plan(2);
	t.skip.is(foo(), 5); // no need to change your plan count when skipping
	t.is(1, 1);
});
```

## Enhanced asserts

AVA comes with [`power-assert`](https://github.com/power-assert-js/power-assert) builtin, giving you more descriptive assertion messages. It reads your test and tries to infer more information from the code.

The following test:

```js
test(t => {
	const x = 'foo';
	t.ok(x === 'bar');
});
```

Would normally give the unhelpful output:

```
false === true
```

With the enhanced asserts, you'll get:

```
t.ok(x === 'bar')
     |
     "foo"
```

True, you could use `t.is()` in this case, and probably should, but this is just a simple example.

Let try a more advanced example:

```js
test(t => {
	const a = /foo/;
	const b = 'bar';
	const c = 'baz';
	t.ok(a.test(b) || b === c);
});
```

And there you go:

```
t.ok(a.test(b) || b === c)
       |    |     |     |
       |    "bar" "bar" "baz"
       false
```

All the assert methods are enhanced.

Have fun!


## Isolated environment

Each test file is run in a separate Node.js process. This comes with a lot of benefits. Different test files can no longer affect each other. Like test files mocking with the global environment, overriding builtins, etc. However, it's mainly done for performance reasons. Even though Node.js can run async IO concurrently, that doesn't help much when tests are heavy on synchronous operations, which blocks the main thread. By running tests concurrently and test files in parallel we take full advantage of modern systems.


## Tips

### Temp files

Running tests concurrently comes with some challenges, doing IO is one. Usually, serial tests just create temp directories in the current test directory and cleans it up at the end. This won't work when you run tests concurrently as tests will conflict with each other. The correct way to do it is to use a new temp directory for each test. The [`tempfile`](https://github.com/sindresorhus/tempfile) and [`temp-write`](https://github.com/sindresorhus/temp-write) modules can be helpful.

### Debugging

AVA runs tests concurrently by default, which is suboptimal when you need to debug something. Instead, run tests serially with the `--serial` option:

```
$ ava --serial
```

### Code coverage

You can't use [`istanbul`](https://github.com/gotwarlost/istanbul) for code coverage as AVA [spawns the test files](#isolated-environment), but you can use [`nyc`](https://github.com/bcoe/nyc) instead, which is basically `istanbul` with support for subprocesses.

As of version `5.0.0` it uses source maps to report coverage for your actual code, regardless of transpilation. Make sure that the code you're testing includes an inline source map or references a source map file. If you use `babel/register` you can set the `sourceMaps` option in your `.babelrc` to `inline`.


## FAQ

### Why not `mocha`, `tape`, `node-tap`?

Mocha requires you to use implicit globals like `describe` and `it` with the default interface (which most people use), too unopinionated, bloated, synchronous by default, unusable programmatic API, serial test execution, and slow. Tape and node-tap are pretty good. AVA is highly inspired by their syntax. However, they both execute tests serially and they've made [TAP](https://testanything.org) a first-class citizen which has IMHO made their codebases a bit convoluted and coupled. TAP output is hard to read so you always end up using an external tap reporter. AVA is highly opinionated and concurrent. It comes with a default simple reporter and supports TAP through a CLI flag.

### How can I use custom reporters?

Use the [`--tap` flag](#optional-tap-output) with any [TAP reporter](https://github.com/sindresorhus/awesome-tap#reporters).

### How is the name written and pronounced?

AVA, not Ava or ava. Pronounced [`/ˈeɪvə/` ay-və](media/pronunciation.m4a?raw=true).

### What is the header background?

[Andromeda galaxy.](https://simple.wikipedia.org/wiki/Andromeda_galaxy)

### Конкурентность и параллелизм

[Concurrency is not parallelism. It enables parallelism.](http://stackoverflow.com/q/1050222)


## Рецепты

- [Code coverage](docs/recipes/code-coverage.md)
- [Endpoint testing](docs/recipes/endpoint-testing.md)
- [When to use `t.plan()`](docs/recipes/when-to-use-plan.md)


## Поддержка

- [Stack Overflow](https://stackoverflow.com/questions/tagged/ava)
- [Gitter chat](https://gitter.im/sindresorhus/ava)
- [Twitter](https://twitter.com/ava__js)


## Другое

- [AVA logo stickers](https://www.stickermule.com/user/1070705604/stickers)


## Похожее

- [gulp-ava](https://github.com/sindresorhus/gulp-ava) - Run tests with gulp
- [grunt-ava](https://github.com/sindresorhus/grunt-ava) - Run tests with grunt
- [fly-ava](https://github.com/pine613/fly-ava) - Run tests with fly
- [start-ava](https://github.com/start-runner/ava) - Run tests with start


## Команда

[![Sindre Sorhus](https://avatars.githubusercontent.com/u/170270?s=130)](http://sindresorhus.com) | [![Vadim Demedes](https://avatars.githubusercontent.com/u/697676?s=130)](https://github.com/vdemedes) | [![James Talmage](https://avatars.githubusercontent.com/u/4082216?s=130)](https://github.com/jamestalmage)
---|---|---|---
[Sindre Sorhus](http://sindresorhus.com) | [Vadim Demedes](https://github.com/vdemedes) | [James Talmage](https://github.com/jamestalmage)

#### Former

- [Kevin Mårtensson](https://github.com/kevva)



<div align="center">
	<br>
	<br>
	<br>
	<img src="https://cdn.rawgit.com/sindresorhus/ava/fe1cea1ca3d2c8518c0cc39ec8be592beab90558/media/logo.svg" width="200" alt="AVA">
	<br>
	<br>
</div>
