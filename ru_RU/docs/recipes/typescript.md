# TypeScript

Переводы: [Français](https://github.com/sindresorhus/ava-docs/blob/master/fr_FR/docs/recipes/typescript.md)

В AVA включена поддержка TypeScript файлов. Это позволяет разработчикам использовать TypeScript для написания тестов.

## Установка

Установите TypeScript компилятор [tsc](https://github.com/Microsoft/TypeScript).

```
$ npm install --save-dev tsc
```

Создайте [`tsconfig.json`](https://github.com/Microsoft/TypeScript/wiki/tsconfig.json) файл. Этот файл определяет параметры компилятора, необходимые для компиляции проекта или тестового файла.

```json
{
	"compilerOptions": {
		"module": "commonjs",
		"target": "es2015"
	}
}
```

Добавьте `test` скрипт в Ваш `package.json` файл. Сперва будет скомпилирован проект и после будет произведет запуск AVA.

```json
{
  "scripts": {
    "test": "tsc && ava"
  }
}
```


## Добавьте тесты

Создайте `test.ts` файл.

```ts
import test from 'ava';

async function fn() {
    return Promise.resolve('foo');
}

test(async (t) => {
    t.is(await fn(), 'foo');
});
```


## Выполните тесты

```
$ npm test
```
