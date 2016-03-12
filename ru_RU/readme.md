___
**Примечание от переводчика**

Здесь содержится перевод файла [readme.md](https://github.com/sindresorhus/ava/blob/master/readme.md). Если при нажатии на [ссылку](https://github.com/sindresorhus/ava/compare/d5274c060ab785ca5823bf15e6c07ac2a2f7230b...master#diff-0730bb7c2e8f9ea2438b52e419dd86c9), содержащую последние правки в master ветке AVA, Вы не можете найти файл `readme.md`, то можете считать что перевод актуален.
___
# ![AVA](https://github.com/sindresorhus/ava/blob/master/media/header.png)

> Футуристичный инструмент для тестирования

[![Build Status: Linux](https://travis-ci.org/sindresorhus/ava.svg?branch=master)](https://travis-ci.org/sindresorhus/ava) [![Build status: Windows](https://ci.appveyor.com/api/projects/status/igogxrcmhhm085co/branch/master?svg=true)](https://ci.appveyor.com/project/sindresorhus/ava/branch/master) [![Coverage Status](https://coveralls.io/repos/sindresorhus/ava/badge.svg?branch=master&service=github)](https://coveralls.io/github/sindresorhus/ava?branch=master) [![Gitter](https://badges.gitter.im/join chat.svg)](https://gitter.im/sindresorhus/ava)

Даже с учетом того, что JavaScript выполняется в одном потоке, IO в Node.js могут происходить параллельно в связи с природой самого Node.js. AVA в полной мере пользуется этим преимуществом и запускает Ваши тесты одновременно, что особенно важно при серьезных IO в тестах. Кроме того, тестовые файлы запускаются параллельно в разных процессах, предоставляя большую производительность и изолированное окружение для каждого тестового файла. [Переход](https://github.com/sindresorhus/pageres/commit/663be15acb3dd2eb0f71b1956ef28c2cd3fdeed0) с Mocha на AVA в Pageres улучшил время тестирования с 31 секунды до 11 секунд. Возможность запускать тесты одновременно, заставляет Вас писать тесты, в которых нет необходимости зависеть от глобального состояния или состояний других тестов, что поистине круто!

*Прочтите наш [гайд участника](contributing.md) если вы хотите внести вклад в проект(проблемы/пулл реквесты/etc).*

Подпишитесь на [Твиттер аккаунт AVA](https://twitter.com/ava__js) для отслеживания обновлений.

Переводы: [Español](https://github.com/sindresorhus/ava-docs/blob/master/es_ES/readme.md), [Français](https://github.com/sindresorhus/ava-docs/blob/master/fr_FR/readme.md), [日本語](https://github.com/sindresorhus/ava-docs/blob/master/ja_JP/readme.md), [Português](https://github.com/sindresorhus/ava-docs/blob/master/pt_BR/readme.md)


## Содержание

- [Использование](#Использование)
- [Командная строка](#Командная-строка)
- [Конфигурация](#Конфигурация)
- [Документация](#Документация)
- [API](#api)
- [Проверки](#Проверки)
- [Советы](#Советы)
- [FAQ](#faq)
- [Рецепты](#Рецепты)
- [Поддержка](#Поддержка)
- [Похожее](#Похожее)
- [Ссылки](#Ссылки)
- [Команда](#Команда)

## Почему AVA?

- Минимально и быстро
- Простой синтаксис тестов
- Запуск тестов параллельно
- Атомарные операции в тестах
- Нет неявных глобальных переменных
- [Изолированное окружение для каждого теста](#Изолированное-окружение-для-каждого-теста)
- [Поддержка ES2015](#ES2015)
- [Поддержка Promises](#Promises)
- [Поддержка генераторов](#Поддержка-генераторов)
- [Поддержка асинхронных функций](#Поддержка-асинхронных-функций)
- [Поддержка Observable типов](#Observable)
- [Поддержка коллбеков](#Поддержка-коллбеков)
- [Улучшенные проверки](#Улучшенные-проверки)
- [Опциональный TAP вывод](#tap-report)
- [Чистый Stack trace](#stack-trace)


## Синтаксис тестов

```js
import test from 'ava';

test(t => {
	t.same([1, 2], [1, 2]);
});
```


## Использование

#### Добавление AVA в Ваш проект

Установите AVA глобально и запустите с флагом `--init`, чтобы добавить AVA в Ваш `package.json` файл.

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

Любые аргументы, переданные после `--init` будут добавлены в `package.json` файл.

#### Ручная установка
Вы также можете установить AVA в текущий каталог модулей:

```console
$ npm install --save-dev ava
```

Вам необходимо настроить `test` скрипт в Вашем `package.json` файле для использования `ava`
(смотрите ниже).

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

<img src="https://github.com/sindresorhus/ava/raw/master/screenshot.png" width="150" align="right">

### Запуск

```console
$ npm test
```

### Watch it

```console
$ npm test -- --watch
```

В AVA есть интеллектуальный режим наблюдения. [Подробнее в этом рецепте](docs/recipes/watch-mode.md).

## Командная строка
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
	--match, -m      Only run tests with matching title (Can be repeated)
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

*Замечание - командная утилита AVA CLI будет использовать локальную установку AVA, даже при запуске глобальной команды `ava`.*

Каталоги по умолчанию рекурсивны, все `*.js` файлы в них будут считаться тестовыми. Каталоги с названием `fixtures`, `helpers` и `node_modules` игнорируются, также как и те, что начинаются с `_`. Это весьма полезно, если Ваши вспомогательные файлы находятся в том же каталоге, что и тестовые файлы.

Когда используется команда `npm test`, Вы можете передать дополнительные аргументы `npm test test2.js`, но флаги должны быть переданы вот так - `npm test -- --verbose`.


## Конфигурация

Все опции командной строки также могут быть установлены в секции `ava` вашего `package.json` файла. Это позволяет изменять стандартное поведение команды `ava`, так что Вам не нужно постоянно писать одни и те же опции в командной строке.

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
    "serial": true,
    "tap": true,
    "verbose": true,
    "require": [
      "babel-register"
  	],
	"babel": "inherit"
  }
}
```

Аргументы переданные в командную строку всегда будут иметь приоритет перед конфигурацией в `package.json` файле.
Смотрите секцию [Поддержка ES2015](#ES2015) для пояснений по опции `babel`.

## Документация

Тесты выполняются одновременно. Ваши тесты могут быть и асинхронными, и синхронными. Тесты, которые не возвращают `promise` или [`observable`](https://github.com/zenparsing/zen-observable), считаются синхронными.

Мы *настойчиво* рекомендуем использовать [асинхронные функции](#Поддержка-асинхронных-функций); Они делают асинхронный код кратким и читабельным, неявно возвращая Promise.

При использовании `Promise` или `observable`, Вы можете включить "коллбек мод" следующим образом: `test.cb([title], fn)`. Все тесты, которые написаны с "коллбек модом" должны заканчиваться вызовом `t.end()`. Этот режим в основном предназначен для тестирования API в коллбек стиле.

Вы должны сделать все тесты синхронными. Они не могут быть описаны внутри `setTimeout`, `setImmediate` и т.д.

Все тестовые файлы запускаются из текущего каталога, так что [`process.cwd()`](https://nodejs.org/api/process.html#process_process_cwd) тот же, что и [`__dirname`](https://nodejs.org/api/globals.html#globals_dirname). Вы можете использовать относительные пути вместо `path.join(__dirname, 'relative/path')`.

### Создание тестов

Для создания теста Вам необходимо импортировать AVA Api и вызвать функцию `test`. Заголовок теста является не обязательным и Вы можете его пропустить, тест-функция обязательна. Тест-функция будет вызвана во время запуска тест.
Тест-функция получает [выполняемый объект](#t) первым аргументом. Согласно соглашению этот аргумент называется `t`.

```js
  import test from 'ava';

  test('my passing test', t => {
    t.pass();
  });
```

#### Заголовки

Заголовки теста - не обязательны и Вы можете писать тесты без заголовка:

```js
test(t => {
	t.pass();
});
```

Рекомендуем указывать заголовки тестов, если у Вас больше одного теста.

Если Вы не указали заголовок теста, но передали именную функцию, то имя функции будет использоваться как заголовок:

```js
test(function name(t) {
	t.pass();
});
```

### План проверок

План проверок необходим для того, чтобы убедиться, что заданное количество проверок будет выполнено. В большинстве случаев, он гарантирует, что тест не прервется до того, как ожидаемое количество проверок выполнится. План может провалить тест, если будет выполнено большее число проверок - это удобно, если проверки выполняются в коллбеке или цикле.

Помните, что в отличие от `node-tap/tape`, AVA автоматически не завершит тест после достижения ожидаемого количества проверок.

В этом примере тест завершится успешно:

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

В этом примере тест завершится ошибкой:

```js
test(t => {
	t.plan(2);

	for (let i = 0; i < 3; i++) {
		t.true(i < 3);
	}
}); // ошибка, будет выполнено 3 сравнения, что не соответствует .plan(2)

test(t => {
	t.plan(1);

	someAsyncFunction(() => {
		t.pass();
	});
}); // ошибка, синхронный тест завершится до сравнения
```

### Последовательные тесты

Конкурентность - это прекрасно, но есть некоторые вещи, которые не могут выполняться одновременно.

В редких случаях, Вы можете использовать `test.serial`, для запуска тестов последовательно, один за другим.

```js
test.serial(t => {
	t.pass();
});
```

Обратите внимание, что это применимо только для конкретного тестового файла. AVA запустит все тестовые файлы одновременно, если Вы не передадите [`--serial` CLI флаг](#Командная-строка).

### Запуск определенных тестов

Во время разработки может появится необходимость вызвать только определённые тесты. Вы можете использовать `.only` модификатор для достижения этих целей.

```js
test('will not be run', t => {
	t.fail();
});

test.only('will be run', t => {
	t.pass();
});
```

`.only` применяется для всех тестовых файлов. Так что, если Вы используете его в одном файле, то тесты из других файлов не будут запущены.

### Запуск тестов с соответствующими заголовками

Флаг `--match` позволяет запускать только те тесты, заголовки которых соответствуют шаблону в флаге. Это работает с помощью шаблона поиска. Шаблоны не чувствительны к регистру. Посмотрите [`matcher`](https://github.com/sindresorhus/matcher) для подробной информации.

Совпадение заголовков, заканчивающихся на `foo`:

```console
$ ava --match='*foo'
```

Совпадение заголовков, начинающихся с `foo`:

```console
$ ava --match='foo*'
```

Совпадение заголовков, содержищах `foo`:

```console
$ ava --match='*foo*'
```

Совпадение заголовков, которые *называются* `foo` (регистро независимы):

```console
$ ava --match='foo'
```

Совпадение заголовков, которые не содержат `foo`:

```console
$ ava --match='!*foo*'
```

Совпадение заголовков, начинающихся с `foo` и заканчивающихся на `bar`:

```console
$ ava --match='foo*bar'
```

Совпадение заголовков, начинающихся с `foo` или заканчивающихся на `bar`:

```console
$ ava --match='foo*' --match='*bar'
```

Обратите внимание, что шаблон соответствия имеет приоритет над `.only` модификатором. Только тесты с явными заголовками будут совпадать. Тесты без заголовков или тесты, чьи заголовки происходят от имени коллбек функции будут пропущены в случае использования `--match`.

Вот что произойдет при запуске AVA с шаблоном `*oo*` для следующих тестов:

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

// не запуститься, нет заголовка
test(function (t) {
	t.fail();
});

// не запуститься, нет явного заголовка
test(function foo(t) {
	t.fail();
});
```

### Пропуск тестов

Иногда падающие тесты трудно исправить. Вы можете сказать AVA пропустить эти тесты, используя `.skip` модификатор. Пропущенные тесты показываются в выводе, но никогда не выполняются. Для пропуска тестов необходима коллбек функция.

```js
test.skip('will not be run', t => {
	t.fail();
});
```

### Todo тесты

Todo-tests, так же, как и пропуск тестов(skip-tests), показаны в выводе, но не никогда не выполняются. Они могут быть полезны для планирования будущих тестов. Todo тесты требуют наличия заголовка в тесте, коллбек функцию им передать нельзя.

```js
test.todo('will think about writing this later');
```


### Хуки "before" и "after"

AVA позволяет зарегистрировать хуки, которые будут вызваны до(before) и после(after) Ваших тестов.

`test.before()` регистрирует хук, который будет запущен перед первым тестом в Вашем тест-файле. Также `test.after()` регистрирует хук, который будет запущен после последнего теста.

`test.beforeEach()` регистрирует хук, который будет запущен перед каждым тестом в Вашем тест-файле. Также `test.afterEach()` регистрирует хук, который будет запущен после каждого теста.

Хуки принимают необязательный заголовок и коллбек функцию аналогично функции `test()`. Заголовок будет выведен, если выполнение хука завершилось ошибкой. В коллбек передается [выполняемый объект](#t).

`before` хук выполняется до `beforeEach` хуков. `afterEach` выполняется до `after` хуков. Каждая категория хуков выполняется в том порядке, в которым они были объявлены.

```js
test.before(t => {
	// выполнится перед всеми тестами
});

test.before(t => {
	// выполнится после предыдущего хука, но перед тестами
});

test.after('cleanup', t => {
	// выполнится после всех тестов
});

test.beforeEach(t => {
	// выполнится перед каждым тестом
});

test.afterEach(t => {
	// выполнится после каждого теста
});

test(t => {
	// обычный тест
});
```

Выполнение хуком может быть синхронным и асинхронным, так же как и тесты. Чтобы хук выполнялся асинхронно, возвращайте Promise или observable, используйте асинхронные функции или включите "коллбек мод" через `test.cb.before()`, `test.cb.beforeEach()`, и т.д.

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

Держите в голове, что `beforeEach` и `afterEach` хуки выполняются "до" и "после" запуска теста и по-умолчанию все тесты запускаются асинхронно. Если Вам необходимо выставить глобальное состояние для каждого теста (отслеживание `console.log` [к примеру](https://github.com/sindresorhus/ava/issues/560)), Вам необходимо запускать [тесты последовательно](#Последовательные-тесты).

Помните, что AVA запускает каждый тест-файл в отдельном процессе. Вы не можете обнулять глобальное состояние в `after`-хуке, так как он запускается перед завершением процесса.

Хуки `beforeEach` и `afterEach` могут делить context с тестом:

```js
test.beforeEach(t => {
	t.context.data = generateUniqueData();
});

test(t => {
	t.is(t.context.data + 'bar', 'foobar');
});
```

Context по умолчанию - объект, но также он может быть определен:

```js
test.beforeEach(t => {
	t.context = 'unicorn';
});

test(t => {
	t.is(t.context, 'unicorn');
});
```

Расшаривание контекста *не* доступно `before` и `after` хукам.

### Цепочка тестовых модификаторов

Вы можете использовать `.serial`, `.only` и `.skip` модификаторы в любом порядке с любым из - `test`, `before`, `after`, `beforeEach` и `afterEach`. Для примера:

```js
test.before.skip(...);
test.skip.after(...);
test.serial.only(...);
test.only.serial(...);
```

Это значит, что Вы можете временно добавить `.skip` или `.only` в конец Вашего объявления теста или хук без внесения дополнительных изменений.

### Собственный модуль проверок

Вы можете использовать любую библиотеку проверок вместо или с тем функционалом, что встроен в AVA, но в этом случае Вы не сможете использовать метод `.plan()`, [смотрите #25](https://github.com/sindresorhus/ava/issues/25).

```js
import assert from 'assert';

test(t => {
	assert(true);
});
```

### <a name="ES2015"></a>Поддержка ES2015

AVA поддерживает модули ES2015 через [Babel 6](https://babeljs.io). Просто пишите свои тесты в ES2015. Никакой дополнительной настройки не нужно. Вы можете использовать любую версию Babel в своем проекте. Мы используем наш собственный собранный Babel с предустановками (пресетами) [`es2015`](http://babeljs.io/docs/plugins/preset-es2015/), [`stage-2`](http://babeljs.io/docs/plugins/preset-stage-2/) и плагинами [`espower`](https://github.com/power-assert-js/babel-plugin-espower), [`transform-runtime`](https://babeljs.io/docs/plugins/transform-runtime/).

Пример стандартного `babel` конфиг-файла, который использует AVA:

```json
{
  "presets": [
    "es2015",
    "stage-0",
  ],
  "plugins": [
    "espower",
    "transform-runtime"
  ]
}
```

При необходимости вы можете внести изменение в поведение транспилера через опцию `babel` в вашем [`package.json`](#configuration). Например, вы можете переопределить пресеты следующим образом:

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

Вы также можете использовать ключевое слово `"inherit"`. В таком случае AVA будет ориентироваться на настройки Babel в ваших [`.babelrc` или `package.json` файлах](https://babeljs.io/docs/usage/babelrc/). Таким образом ваши тест файлы будут транспилированы аналогично вашим исходным файлам, в соответствии с вашими настройками и без необходимости создания отдельных настроек для AVA:

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

Важно помнить, что AVA *всегда* добавляет плагины [`espower`](https://github.com/power-assert-js/babel-plugin-espower) и [`transform-runtime`](https://babeljs.io/docs/plugins/transform-runtime/).

### Поддержка TypeScript

AVA поддерживает TypeScript. Вам необходимо настроить транспиллинг собственноручно. Когда Вы устанавливаете `module` в `commonjs` в Вашем файле `tsconfig.json`, TypeScript автоматически найдет определение типов для AVA. Вам необходимо выставить `target` в `es2015` для использования Promises и асинхронных функций.

#### Транспиллинг импортированных модулей

AVA в данный момент транспиллит только запущенные тесты. *AVA не делает транспиллинг импортированных модулей ```import``` вне теста.* Есть веские причины для этого подхода, это может быть не тем, что Вы ожидали!

Если Вы используете Babel, то посмотрите в [Babel's require hook](https://babeljs.io/docs/usage/require/) для транспиллинга импортированных модулей на лету. Запустите AVA с `--require babel-register` (смотрите [Командная строка](#Командная-строка)) или [настройте `package.json`](#Конфигурация).

### <a name="Promises"></a>Поддержка Promises

Если Вы возвращаете Promise в своем тесте, то Вам не нужно явно завершать тест, так как он завершится сразу после успешного выполнения Promise.

```js
test(t => {
	return somePromise().then(result => {
		t.is(result, 'unicorn');
	});
});
```

### Поддержка генераторов

AVA имеет встроенную поддержку [генераторов](https://developer.mozilla.org/ru/docs/Web/JavaScript/Reference/Statements/function*).

```js
test(function * (t) {
	const value = yield generatorFn();
	t.true(value);
});
```

### Поддержка асинхронных функций

AVA имеет поддержку [ансихронных функций](https://tc39.github.io/ecmascript-asyncawait/) *(async/await)*.

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

### <a name="Observable"></a>Поддержка Observable типов

AVA поддерживает [observables типы](https://github.com/zenparsing/es-observable). Если Вы возвращаете объект типа observable в тесте, AVA дождется завершения до окончания теста.

*Вам не нужно использовать "коллбек мод" или вызывать `t.end()`.*

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

### Поддержка коллбеков

AVA поддерживает использование `t.end` как финального коллбека, когда используется традиционный Node.js стиль. AVA ожидает, что любое положительное значение, переданное первым параметром в `t.end` является ошибкой. Обратите внимание, что `t.end` необходим "коллбек мод", который можно включить, используя модификатор `.cb`.

```js
test.cb(t => {
	// t.end автоматически проверит ошибку в первом аргументе
	fs.readFile('data.txt', t.end);
});
```

### <a name="tap-report"></a>Опциональный TAP вывод

AVA может сгенерировать TAP вывод через опцию `--tap` для любого из [TAP reporter](https://github.com/sindresorhus/awesome-tap#reporters).

```console
$ ava --tap | tap-nyan
```

<img src="https://github.com/sindresorhus/ava/blob/master/media/tap-output.png" width="398">


### <a name="stack-trace"></a>Чистый Stack trace

AVA автоматически удаляет ненужные строки в Stack trace, что позволить более быстро найти источник ошибки.

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

Тип: `string`

Заголовок теста.

#### `callback(t)`

Тип: `function`

Функция теста.

##### `t`

Тип: `object`

Выполняемый объект конкретного теста. Каждый коллбек теста получает разный объект. Содержит [проверки](#Проверки), а так же `.plan(count)` и `.end()` методы. `t.context` может содержать расшаренное состояние из `beforeEach` хуков.

###### `t.plan(count)`

План о количестве проверок в тесте. Тест завершится ошибкой, если количество выполненных проверок не будет соответствовать запланированным. Смотрите [план проверок](#План проверок).

###### `t.end()`

Завершить тест. Используется только с `test.cb()`.


## Проверки

Проверки встроены в [выполняемый объект](#t), который доступен в каждом коллбеке теста:

```js
test(t => {
	t.ok('unicorn'); // проверка
});
```

Если множество проверок завершились ошибкой в одном тесте, AVA отобразит лишь *первую* ошибку.

### `.pass([message])`

Успешная проверка.

### `.fail([message])`

Ошибочная проверка.

### `.ok(value, [message])`

Проверка, что значение `value` истинно.

### `.notOk(value, [message])`

Проверка, что значение `value` ложно.

### `.true(value, [message])`

Проверка, что значение `value` - `true`.

### `.false(value, [message])`

Проверка, что значение `value` - `false`.

### `.is(value, expected, [message])`

Проверка, что значение `value` эквивалентно `expected`.

### `.not(value, expected, [message])`

Проверка, что значение `value` не эквивалентно `expected`.

### `.same(value, expected, [message])`

Проверка, что значение `value` полностью совпадает(deep equal) с `expected`.

### `.notSame(value, expected, [message])`

Проверка, что значение `value` не полностью совпадает(not deep equal) с `expected`.

### `.throws(function|promise, [error, [message]])`

Проверка, что `function` вызовет ошибку или `promise` завершиться с ошибкой.

`error` может быть конструктором, регулярным выражением, сообщением об ошибке или функцией валидации.
Возвращает ошибку, которую вернула `function` или rejection из `promise`.

### `.notThrows(function|promise, [message])`

Проверка, что `function` не вызовет ошибку `error` или `promise` завершится успешно.

### `.regex(contents, regex, [message])`

Проверка, что `contents` подходит под `regex`.

### `.ifError(error, [message])`

Проверка, что `error` ложно.

### Пропуск проверок

Любую проверку можно пропустить, используя `skip` модификатор. Пропущенные проверки подсчитываются, так что Вам не нужно менять запланированное число проверок.

```js
test(t => {
	t.plan(2);
	t.skip.is(foo(), 5); // не нужно менять количество запланированных проверок
	t.is(1, 1);
});
```

### Улучшенные проверки

В AVA включена [`power-assert`](https://github.com/power-assert-js/power-assert) библиотека, предоставляющая более информативные сообщения проверок. Она читает Ваш тест и пытается получить дополнительную информацию из Вашего кода.

Посмотрим на пример, использующий стандартный Node.js [`assert` модуль](https://nodejs.org/api/assert.html):

```js
const a = /foo/;
const b = 'bar';
const c = 'baz';
require('assert').ok(a.test(b) || b === c);
```

Если Вы выполните этот код, будет возвращено:

```
AssertionError: false == true
```

В AVA этот тест:

```js
test(t => {
	const a = /foo/;
	const b = 'bar';
	const c = 'baz';
	t.ok(a.test(b) || b === c);
});
```

Вернет следующий результат:

```
t.ok(a.test(b) || b === c)
       |    |     |     |
       |    "bar" "bar" "baz"
       false
```

## Изолированное окружение для каждого теста

Каждый тестовый файл запускается в отдельном процессе Node.js. Это позволяет менять глобальное состояние или делать monkey-patching встроенных методов в одном тестовом файле, без влияния на другие тесты. Это также здорово влияет на производительность в современных мульти-процессорных системах, позволяя выполнять тесты параллельно.

## Советы

### Временные файлы

Запуск тестов одновременно имеет свои сложности, выполнение IO одна из них.

Обычно, последовательные тесты создают временный каталог в текущей директории тестов и очищают его после выполнения. Такая схема не будет работать с тестами, запущенными одновременно, так как каждый из тестов бует конфликтовать с другим. Более правильный путь - это создавать новый временный каталог для каждого теста. [`tempfile`](https://github.com/sindresorhus/tempfile) и [`temp-write`](https://github.com/sindresorhus/temp-write) модули могут помочь с этим.

### Отладка

AVA по умолчанию запускает тесты одновременно/параллельно, что не всегда оптимально для отладки. Вместо этого, можно запустить тесты последовательно с аргументом `--serial`:

```console
$ ava --serial
```

### Покрытие кода

Для покрытия кода Вы не можете использовать [`istanbul`](https://github.com/gotwarlost/istanbul), так как AVA использует [изолированное окружение для каждого файла](#Изолированное-окружение-для-каждого-теста), однако Вы можете использовать [`nyc`](https://github.com/bcoe/nyc), который похож на [`istanbul`](https://github.com/gotwarlost/istanbul) но с поддержкой запуска в отдельном окружении(sub-process).

Начиная с версии `5.0.0` используются source maps для отчета о покрытии кода, независимо от транспиллинга. Убедитесь, что код, который Вы тестируете содержит инлайны с source map или ссылку на source map файл. Если Вы используете `babel-register` Вы можете выставить опцию `sourceMaps` в Вашем `.babelrc` файле на `inline`.


## FAQ

### Почему не `mocha`, `tape`, `node-tap`?

Mocha требует использование неявных глобальных метоов `describe` и `it` в своем интерфейсе по умолчанию(большинство людей используют это), что слишком догматично, раздуто, синхронно по умолчанию, непригодно для программных API, последовательно и слишком медленно.

Tape и node-tap вполне хороши. AVA была вдохновлена их синтаксисом. Однако они обе выполняют тесты последовательно и они делают [TAP](https://testanything.org) своей основной идеей, что сделало их кодовую базу запутанной и связанной. TAP вывод слишком сложен для чтения, так что Вы всегда должны использовать сторонний tap обработчик.

AVA является достаточно качественным и конкурентным продуктом. В AVA включен стандартный и простой обработчик вывода, поддерживающий TAP через параметры командной строки.

### Как мне использовать собственные отчеты?

AVA поддерживает TAP формат и совместим с любым из [TAP reporter'ов](https://github.com/sindresorhus/awesome-tap#reporters).

### Как произносится и пишется название проекта?

AVA, не Ava или ava. Произносится как [`/ˈeɪvə/` ay-və](https://github.com/sindresorhus/ava/blob/master/media/pronunciation.m4a?raw=true).

### Что за картинка используется в логотипе?

Это [Галактика Андромеды.](https://ru.wikipedia.org/wiki/Галактика_Андромеды)

### Конкурентность и параллелизм

[Concurrency is not parallelism. It enables parallelism.](http://stackoverflow.com/q/1050222)


## Рецепты

- [Покрытие кода](docs/recipes/code-coverage.md)
- [Режим наблюдения](docs/recipes/watch-mode.md)
- [Тестирование HTTP серверов](docs/recipes/endpoint-testing.md)
- [Когда применять `t.plan()`](docs/recipes/when-to-use-plan.md)
- [Тестирование в браузере](docs/recipes/browser-testing.md)
- [TypeScript](docs/recipes/typescript.md)


## Поддержка

- [Stack Overflow](https://stackoverflow.com/questions/tagged/ava)
- [Gitter chat](https://gitter.im/sindresorhus/ava)
- [Twitter](https://twitter.com/ava__js)


## Похожее

- [sublime-ava](https://github.com/sindresorhus/sublime-ava) - Snippets for AVA tests
- [atom-ava](https://github.com/sindresorhus/atom-ava) - Snippets for AVA tests
- [vscode-ava](https://github.com/samverschueren/vscode-ava) - Snippets for AVA tests
- [eslint-plugin-ava](https://github.com/sindresorhus/eslint-plugin-ava) - Lint rules for AVA tests
- [gulp-ava](https://github.com/sindresorhus/gulp-ava) - Run tests with gulp
- [grunt-ava](https://github.com/sindresorhus/grunt-ava) - Run tests with grunt
- [fly-ava](https://github.com/pine613/fly-ava) - Run tests with fly
- [start-ava](https://github.com/start-runner/ava) - Run tests with start


## Ссылки

- [Купить наклейки AVA](https://www.stickermule.com/user/1070705604/stickers)
- [Awesome list](https://github.com/sindresorhus/awesome-ava)

## Команда

[![Sindre Sorhus](https://avatars.githubusercontent.com/u/170270?s=130)](http://sindresorhus.com) | [![Vadim Demedes](https://avatars.githubusercontent.com/u/697676?s=130)](https://github.com/vdemedes) | [![James Talmage](https://avatars.githubusercontent.com/u/4082216?s=130)](https://github.com/jamestalmage) | [![Mark Wubben](https://avatars.githubusercontent.com/u/33538?s=130)](https://novemberborn.net)
---|---|---|---|---
[Sindre Sorhus](http://sindresorhus.com) | [Vadim Demedes](https://github.com/vdemedes) | [James Talmage](https://github.com/jamestalmage) | [Mark Wubben](https://novemberborn.net)

### Бывший участник

- [Kevin Mårtensson](https://github.com/kevva)



<div align="center">
	<br>
	<br>
	<br>
	<img src="https://cdn.rawgit.com/sindresorhus/ava/fe1cea1ca3d2c8518c0cc39ec8be592beab90558/media/logo.svg" width="200" alt="AVA">
	<br>
	<br>
</div>
