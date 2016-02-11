___
**Примечание от переводчика**

Здесь содержится перевод файл [code-coverage.md](https://github.com/sindresorhus/ava/blob/master/docs/recipes/code-coverage.md). Вот [ссылка]() если при нажатии на ссылку, Вы не можете найти файл readme.md, то можете считать что перевод актуален).
___
# Покрытие кода

Переводы: [Español](https://github.com/sindresorhus/ava-docs/blob/master/es_ES/docs/recipes/code-coverage.md), [Français](https://github.com/sindresorhus/ava-docs/blob/master/fr_FR/docs/recipes/code-coverage.md), [日本語](https://github.com/sindresorhus/ava-docs/blob/master/ja_JP/docs/recipes/code-coverage.md), [Português](https://github.com/sindresorhus/ava-docs/blob/master/pt_BR/docs/recipes/code-coverage.md)

Так как AVA [запускает тестовые файлы в отдельном окружении][isolated-env], Вы не можете использовать [`istanbul`] для покрытия кода; вместо этого, воспользуйтесь [`nyc`], который похож на [`istanbul`] но с поддержкой запуска в отдельном окружении(sub-process).

## Установка

Установите NYC:

```
$ npm install nyc --save-dev
```

Затем добавьте `.nyc_output` и `coverage` каталоги в Ваш `.gitignore` файл.

`.gitignore`:

```
node_modules
coverage
.nyc_output
```

## ES5 покрытие

Использовать NYC для покрытия кода, написанного в ES5 весьма просто. Просто добавьте в начало Вашего тестового скрипта `nyc`:

```json
{
	"scripts": {
		"test": "nyc ava"
	}
}
```

Это все!

Если Вам нужно сделать HTML отчет о покрытии кода или залить данные о покрытии кода в Coveralls, Вы должны пролистать до нужных секций в этом документе.

## ES2015 покрытие

Использование Babel транспиллера немного более сложно. Мы разбили описание на несколько этапов.

### Конфигурирование Babel

Во-первых, нам нужна Babel конфигурация. Пример ниже должен быть изменен под Ваши нужды.

`package.json`:
```json
{
	"babel": {
		"presets": ["es2015"],
		"plugins": ["transform-runtime"],
		"ignore": "test.js",
		"env": {
			"development": {
				"sourceMaps": "inline"
			}
		}
	}
}
```

На заметку - в примере выше есть две важные вещи.

1. Мы игнорируем тестовые файлы, так как AVA уже обрабатывает тесты, прошедшие через транспиллер.

2. Мы указали `inline` source maps для разработки. Это важно для корретной генерации покрытия кода. Использование `env` секции в конфигурации Babel позволяет нам отключить source maps для сборки на продуктиве.


### Создание скрипта сборки

Поскольку Вам вряд ли потребуется `inline` source maps в продуктиве, Вам необходимо указать альтернативное окружение в Ваших сборочных скриптах:

`package.json`

```json
{
	"scripts": {
		"build": "BABEL_ENV=production babel --out-dir=dist index.js"
	}
}
```

> ВНИМАНИЕ: `BABEL_ENV=production` не работает на машинах с ОС Windows, Вы должны использовать `set` (`set BABEL_ENV=production`). Для кросс-платформенных сборок, смотрите [`cross-env`].

Обратите внимание, что скрипт сборки имеет очень мало общего с AVA и это просто примеры, демонстрирующие установку 'env' для совместимости конфига Babel с AVA.

### Использование require хука в Babel

Для использование require хука в Babel, добавьте `babel-core/register` в секцию `require` конфига AVA в файле `package.json`.

```json
{
	"ava": {
		"require": ["babel-core/register"]
	}
}
```

*Замечание*: Вы так же можете установить require хук используя командную строку: `ava --require=babel-core/register`. Однако, конфигурирование через файл `package.json` спасет Вас от постоянного набор этой строки в терминале.

### Суммарно

Объединяя предыдущие шаги выше, Ваш файл `package.json` должен быть похож на:

```json
{
	"scripts": {
		"test": "nyc ava",
		"build": "BABEL_ENV=production babel --out-dir=dist index.js"
	},
	"babel": {
		"presets": ["es2015"],
		"plugins": ["transform-runtime"],
		"ignore": "test.js",
		"env": {
			"development": {
				"sourceMaps": "inline"
			}
		}
	},
	"ava": {
		"require": ["babel-core/register"]
	}
}
```


## HTML отчеты

NYC создает `json` файл с покрытием кода для каждого форкнутого процесса в каталоге `.nyc_ouput`.

Для объединения этих файлов в читабельным HTML отчет, сделайте следующее:

```
$ ./node_modules/.bin/nyc report --reporter=html
```

Или используйте npm script:

```json
{
	"scripts": {
		"report": "nyc report --reporter=html"
	}
}
```

Эти команды создадут HTML отчет в каталоге `coverage`.


## Внешний хостинг покрытия кода

### Travis CI & Coveralls

Во-первых, Вы должны залогиниться в [coveralls.io] и активировать Ваш репозитарий кода.

После этого добавьте [`coveralls`] как development зависимость:

```
$ npm install coveralls --save-dev
```

Добавьте следующие строки в Ваш `.travis.yml` файл:

```yaml
after_success:
	- './node_modules/.bin/nyc report --reporter=text-lcov | ./node_modules/.bin/coveralls'
```

Ваш отчет о покрытии появиться в Coveralls вскоре, после тогда как завершится сборка в Travis.

[`babel`]:      https://github.com/babel/babel
[coveralls.io]: https://coveralls.io
[`coveralls`]:  https://github.com/nickmerwin/node-coveralls
[`cross-env`]:  https://github.com/kentcdodds/cross-env
[isolated-env]: https://github.com/sindresorhus/ava#isolated-environment
[`istanbul`]:   https://github.com/gotwarlost/istanbul
[`nyc`]:        https://github.com/bcoe/nyc
