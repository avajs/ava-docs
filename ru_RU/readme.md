___
**Примечание от переводчика**

Здесь содержится перевод файла [readme.md](https://github.com/sindresorhus/ava/blob/master/readme.md). Если при нажатии на [ссылку](https://github.com/sindresorhus/ava/compare/65ae07c76b2a5927e6bcc00630a691c98f6e7c46...master#diff-0730bb7c2e8f9ea2438b52e419dd86c9), содержащую последние правки в master ветке AVA, Вы не можете найти файл `readme.md`, то можете считать что перевод актуален.
___
# ![AVA](https://github.com/sindresorhus/ava/blob/master/media/header.png)

> Футуристичный инструмент для тестирования

[![Build Status: Linux](https://travis-ci.org/sindresorhus/ava.svg?branch=master)](https://travis-ci.org/sindresorhus/ava) [![Build status: Windows](https://ci.appveyor.com/api/projects/status/igogxrcmhhm085co/branch/master?svg=true)](https://ci.appveyor.com/project/sindresorhus/ava/branch/master) [![Coverage Status](https://coveralls.io/repos/sindresorhus/ava/badge.svg?branch=master&service=github)](https://coveralls.io/github/sindresorhus/ava?branch=master) [![Gitter](https://img.shields.io/badge/Gitter-Join_the_AVA_chat_%E2%86%92-00d06f.svg)](https://gitter.im/sindresorhus/ava)

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
- [Проверик](#Проверки)
- [FAQ](#faq)
- [Рецепты](#Рецепты)


## Почему AVA?

- Минимально и быстро
- Простой синтаксис тестов
- Запуск тестов параллельно
- Атомарные тесты
- Нет неявных глобальных переменных
- [Изолированное окружение для каждого теста](#Изолированное-окружение-для-каждого-теста)
- [Поддержка ES2015](#ES2015)
- [Promise](#Promise)
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

#### Инициализация

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

<img src="https://github.com/sindresorhus/ava/raw/master/screenshot.png" width="150" align="right">

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

Каталоги по умолчанию рекурсивны. Каталоги с названием `fixtures` и `helpers` игнорируются, также как и те, что начинаются с `_`. Это весьма полезно, если Ваши вспомогательные файлы находятся в том же каталоге, что и тестовые файлы.

Когда используется команда `npm test`, Вы можете передать дополнительные аргументы `npm test test2.js`, но флаги должны быть переданы вот так - `npm test -- --verbose`.

*ВНИМАНИЕ - Нестандартное поведение:* Командная строка AVA CLI всегда будет искать локальную установку AVA в Ваших проектах. Поведение сохраняется даже при использовании глобальной команды `ava`. Это нестандартное поведение решает важную [проблему](https://github.com/sindresorhus/ava/issues/157), и не должно влиять на повседневное использование.

## Конфигурация

Все опции командной строки также могут быть установлены в секции `ava` вашего `package.json` файла. Это позволяет изменять стандартное поведение команды `ava`, так что Вам не нужно постоянно писать одни и те же опции в командной строке.

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

Тесты запускаются асинхронно и требуют, чтобы был возвращен объект, поддерживающий асинхронность(Promise, или [observable](https://github.com/zenparsing/zen-observable)). Мы *настойчиво* рекомендуем использовать [асинхронные функции](#Поддержка-асинхронных-функций); Они делают асинхронный код кратким и читабельным, неявно возвращая Promise.

Если в коде Вы не возвращаете объект, поддерживающий асинхронность, тест будет считаться синхронным и завершится незамедлительно.

Если использовать Promises или любой другой объект, поддерживающий асинхронность, Вы можете включить "коллбек мод" таким образом `test.cb([title], fn)`. Все тесты, которые написаны с "коллбек модом" должны заканчиваться с `t.end()`. Этот режим в основном предназначен для тестирование API в коллбек стиле.

Вы должны сделать все тесты синхронными. Они не могут быть описаны внутри `setTimeout`, `setImmediate` и т.д.

Все тестовые файлы запускаются из текущего каталога, так что [`process.cwd()`](https://nodejs.org/api/process.html#process_process_cwd) тот же, что и [`__dirname`](https://nodejs.org/api/globals.html#globals_dirname). Вы можете использовать относительные пути вместо `path.join(__dirname, 'relative/path')`.

### Анатомия тестов

Для создания теста, Вы можете вызвать функцию `test` которую Вы импортировали `require` из AVA и передать опциональное имя теста и функцию, содержажую выполнение теста. Переданной функции задается контекст первым аргументом, так что Вы можете вызывать методы AVA и [проверки](#Проверки).

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

### План проверок

План проверок необходим для того, чтобы убедиться, что заданное количество проверок будет выполнено. В большинстве случаев, он гарантирует, что тест не прервется до того, как ожидаемое количество проверок выполнится. План может провалить тест, если будет выполнено большее число проверок - это удобно, если проверки выполняются в коллбеке или цикле.

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

AVA больше не поддерживает автоматическое завершение тестов через `t.plan(...)`. Это помогает избежать ложных срабатываний, если Вы добавили проверки, но забыли увеличить ожидаемое количество в `t.plan(...)`.

```js
// Это больше не работает

test('auto ending is dangerous', t => {
	t.plan(2);

	t.pass();
	t.pass();

	// После достижений запланированных проверок этот блок не сработает
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

Конкурентность - это прекрасно, но есть некоторые вещи, которые не могут выполняться конкурентно. В редких случаях, Вы можете вызвать `test.serial`, для запуска тестов последовательно, один за другим.

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

### Хуки "до" и "после"

Когда необходима допольнительная настройка, Вы можете использовать `test.before()` и `test.after()`,
в той же манере, что и `test()`. Функция, переданная в `test.before()` и `test.after()`, будет вызвана до/после всех тестов. Вы так же можете использовать `test.beforeEach()` и `test.afterEach()`, если Вам нужно выполнить конфигурацию или подготовку для каждого теста. Хуки запускаются последовательно. Вы можете добавить множество хуков. Опционально Вы можете указать заголовок, который будет показан при ошибке.


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

Вы можете использовать асинхронные функции, возвращать асинхронные объекты или включить "коллбек мод" в любом хуке.

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

### Цепочка тестовых модификаторов

Вы можете сцепить тестовые модификаторы вместе следующим образом:

```js
test.before.skip([title], testFn);
test.skip.after(....);
test.serial.only(...);
test.only.serial(...);
```

Это особенно полезно, когда необходимо временно использовать `skip` и `only` для теста, без потери информативности и поведения других модификаторов.

### Собственный модуль проверок

Вы можете использовать любую библиотеку проверок вместо или с тем функционалом, что встроен в AVA, но в этом случае Вы не сможете использовать метод `.plan()`, [пока что](https://github.com/sindresorhus/ava/issues/25).

```js
import assert from 'assert';

test(t => {
	assert(true);
});
```

### <a name="ES2015"></a>Поддержка ES2015

AVA поддерживает модули ES2015 через [Babel 6](https://babeljs.io). Просто пишите свои тесты в ES2015. Никакой дополнительной настройки не нужно. Вы можете использовать любую версию Babel в своем проекте. Мы используем наш собственный собранный Babel с [`es2015`](http://babeljs.io/docs/plugins/preset-es2015/) и [`stage-2`](http://babeljs.io/docs/plugins/preset-stage-2/) предустановками.

#### Транспиллинг импортированных модулей

AVA в данный момент транспиллит только запущенные тесты. *AVA не делает транспиллинг импортированных модулей ```import``` вне теста.* Есть веские причины для этого подхода, это может быть не тем, что Вы ожидали!

Простым решением может быть [Babel's require hook](https://babeljs.io/docs/usage/require/) для транспиллинга импортированных модулей на лету. Потому как AVA поддерживает модульный синтаксис ES2015, Вы можете использовать это для импорта необходимого хука:

```js
import test from 'ava';
import 'babel-core/register';
import foo from './foo'; // <-- foo может быть написан в ES2015!

test('foo bar', t => {
	t.same('baz', foo('bar'));
});
```

[#111](https://github.com/sindresorhus/ava/issues/111) взгляните на это как на возможное улучшение.

### Promise

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

AVA поддерживает [observables типы](https://github.com/zenparsing/es-observable).
Если Вы возвращаете объект типа observable в тесте, AVA дождется завершения до окончания теста.

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

AVA поддерживает использование `t.end` как финального коллбека, когда используется традиционный Node.js стиль. AVA ожидает, что любое положительное значение, переданное первым параметром в `t.end` является ошибкой. Обратите внимание, что `t.end` необходим "коллбек мод", который можно включить, используя цепочку `test.cb`.

```js
test.cb(t => {
	// t.end автоматически проверить ошибку в первом аргументе
	fs.readFile('data.txt', t.end);
});
```

### <a name="tap-report"></a>Опциональный TAP вывод

AVA может сгенерировать TAP вывод через опцию `--tap` для любого из [TAP reporter](https://github.com/sindresorhus/awesome-tap#reporters).

```
$ ava --tap | tap-nyan
```

<img src="https://github.com/sindresorhus/ava/blob/master/media/tap-output.png" width="398">


### <a name="stack-trace"></a>Чистый Stack trace

AVA автоматически удаляет ненужные строки в Stack trace, что позволить более быстро найти источник ошибки.

<img src="https://github.com/sindresorhus/ava/blob/master/media/stack-traces.png" width="300">


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

Тип: `string`

Заголовок теста.

#### body(context)

Тип: `function`

Функция теста.

##### context

Передается в тестовую функцию и содержить различные AVA методы и [проверки](#Проверки).

###### .plan(count)

План о количестве проверок в тесте. Тест завершится ошибкой, если количество выполненных проверок не будет соответствовать запланированным.

###### .end()

Завершить тест. Используется только с `test.cb()`.


## Проверки

Проверки встроены в [context](#context) теста:

```js
test(t => {
	t.ok('unicorn'); // проверка
});
```

Если множество проверок завершились ошибкой в одном тесте, AVA отобразит лишь *первую* ошибку.

### .pass([message])

Успешная проверка.

### .fail([message])

Ошибочная проверка.

### .ok(value, [message])

Проверка, что значение `value` истинно.

### .notOk(value, [message])

Проверка, что значение `value` ложно.

### .true(value, [message])

Проверка, что значение `value` - `true`.

### .false(value, [message])

Проверка, что значение `value` - `false`.

### .is(value, expected, [message])

Проверка, что значение `value` эквивалентно `expected`.

### .not(value, expected, [message])

Проверка, что значение `value` не эквивалентно `expected`.

### .same(value, expected, [message])

Проверка, что значение `value` полностью совпадает(deep equal) с `expected`.

### .notSame(value, expected, [message])

Проверка, что значение `value` не полностью совпадает(not deep equal) с `expected`.

### .throws(function|promise, [error, [message]])

Проверка, что `function` вызовет ошибку или `promise` завершиться с ошибкой.

`error` может быть конструктором, регулярным выражением, сообщением об ошибке или функцией валидации.

### .notThrows(function|promise, [message])

Проверка, что `function` не вызовет ошибку `error` или `promise` завершится успешно.

### .regex(contents, regex, [message])

Проверка, что `contents` подходит под `regex`.

### .ifError(error, [message])

Проверка, что `error` ложно.

## Пропуск проверок

Любую проверку можно пропустить, используя `skip` модификатор. Пропущенные проверки подсчитываются, так что Вам не нужно менять запланированное число проверок.

```js
test(t => {
	t.plan(2);
	t.skip.is(foo(), 5); // не нужно менять количество запланированных проверок
	t.is(1, 1);
});
```

## Улучшенные проверки

В AVA включена [`power-assert`](https://github.com/power-assert-js/power-assert) библиотека, предоставляющая более информативные сообщения проверок. Она читает Ваш тест и пытается получить дополнительную информацию из Вашего кода.

Следующий тест:

```js
test(t => {
	const x = 'foo';
	t.ok(x === 'bar');
});
```

Обычно выдает бесполезный вывод:

```
false === true
```

С улучшенными assert'ами Вы получите на выходе:

```
t.ok(x === 'bar')
     |
     "foo"
```

Можете воспользоваться `t.is()` в этом случае, и, вероятно, должны, но это просто пример.

Попробуем более сложный пример:

```js
test(t => {
	const a = /foo/;
	const b = 'bar';
	const c = 'baz';
	t.ok(a.test(b) || b === c);
});
```

На выходе:

```
t.ok(a.test(b) || b === c)
       |    |     |     |
       |    "bar" "bar" "baz"
       false
```

Все методы проверок улучшены.

Веселитесь!


## Изолированное окружение для каждого теста

Каждый тестовый файл запускается в отдельном процессе Node.js. В этом есть много преимуществ. Разные тестовые файлы больше не влияют друг на друга. Например, тестовые файлы меняющие глобальное окружение, переопределение встроенных методов и значений атрибутов(monkey patch) и т.д. Однако изолированный запуск тестов сделан из соображений производительности. Даже с учетом того, что Node.js может запускать параллельно асинхронные IO, это не сильно поможет в тестах с большим количеством синхронных операций, что в свою очередь приведет к блокировке основного потока. Запуская тесты конкурентно и тестовые файлы параллельно мы получаем в полной мере можем задействовать современные системы.


## Советы

### Временные файлы

Запуск тестов одновременно имеет свои сложности, выполнение IO одна из них. Обычно, последовательные тесты создают временный каталог в текущей директории тестов и очищают его после выполнения. Такая схема не будет работать с тестами, запущенными одновременно, так как каждый из тестов бует конфликтовать с другим. Более правильный путь - это создавать новый временный каталог для каждого теста. [`tempfile`](https://github.com/sindresorhus/tempfile) и [`temp-write`](https://github.com/sindresorhus/temp-write) модули могут помочь с этим.

### Отладка

AVA по умолчанию запускает тесты одновременно/параллельно, что не всегда оптимально для отладки. Вместо этого, можно запустить тесты последовательно с опцией `--serial`:

```
$ ava --serial
```

### Покрытие кода

Для покрытия кода Вы не можете использовать [`istanbul`](https://github.com/gotwarlost/istanbul) покрытия кода, так как AVA [изолированное окружение для каждого файла](#Изолированное-окружение-для-каждого-теста), однако Вы можете использовать [`nyc`](https://github.com/bcoe/nyc), который похож на [`istanbul`](https://github.com/gotwarlost/istanbul) но с поддержкой запуска в отдельном окружении(sub-process).

Начиная с версии `5.0.0` используются source maps для отчета о покрытии кода, независимо от транспиллинга. Убедитесь, что код, который Вы тестируете содержит инлайны с source map или ссылку на source map файл. Если Вы используете `babel/register` Вы можете выставить опцию `sourceMaps` в Вашем `.babelrc` файле на `inline`.


## FAQ

### Почему не `mocha`, `tape`, `node-tap`?

Mocha требует использование неявных глобальных метоов `describe` и `it` в своем интерфейсе по умолчанию(большинство людей используют это), что слишком догматично, раздуто, синхронно по умолчанию, непригодно для программных API, последовательно и слишком медленно. Tape и node-tap вполне хороши. AVA была вдохновлена их синтаксисом. Однако они обе выполняют тесты последовательно и они делают [TAP](https://testanything.org) своей основной идеей, что сделало их кодовую базу запутанной и связанной. TAP вывод слишком сложен для чтения, так что Вы всегда должны использовать сторонний tap обработчик. AVA является достаточно качественным и конкурентным продуктом. В AVA включен стандартный и простой обработчик вывода, поддерживающий TAP через параметры командной строки.

### Как мне использовать собственные отчеты?

Используйте [`--tap` флаг](#tap-report) с любым из [TAP reporter'ов](https://github.com/sindresorhus/awesome-tap#reporters).

### Как произносится и пишется название проекта?

AVA, не Ava или ava. Произносится как [`/ˈeɪvə/` ay-və](https://github.com/sindresorhus/ava/blob/master/media/pronunciation.m4a?raw=true).

### Что за картинка используется в логотипе?

[Галактика Андромеды.](https://ru.wikipedia.org/wiki/Галактика_Андромеды)

### Конкурентность и параллелизм

[Concurrency is not parallelism. It enables parallelism.](http://stackoverflow.com/q/1050222)


## Рецепты

- [Покрытие кода](docs/recipes/code-coverage.md)
- [Тестирование HTTP серверов](docs/recipes/endpoint-testing.md)
- [Когда применять `t.plan()`](docs/recipes/when-to-use-plan.md)


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

#### Бывший участник

- [Kevin Mårtensson](https://github.com/kevva)



<div align="center">
	<br>
	<br>
	<br>
	<img src="https://cdn.rawgit.com/sindresorhus/ava/fe1cea1ca3d2c8518c0cc39ec8be592beab90558/media/logo.svg" width="200" alt="AVA">
	<br>
	<br>
</div>
