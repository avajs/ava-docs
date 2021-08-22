___
**Примечание от переводчика**

Здесь содержится перевод файла [typescript.md](https://github.com/avajs/ava/blob/main/docs/recipes/typescript.md). Если при нажатии на [ссылку](https://github.com/avajs/ava/compare/8e2f3dca177a4283ad882596d3c1425cabb998ef...main#diff-60cce07a584082115d230f2e3d571ad6), содержащую последние правки в master ветке AVA, Вы не можете найти файл `typescript.md`, то можете считать что перевод актуален.
___
# TypeScript

Переводы: [Français](https://github.com/avajs/ava-docs/blob/main/fr_FR/docs/recipes/typescript.md), [Italiano](https://github.com/avajs/ava-docs/blob/main/it_IT/recipes/typescript.md), [Русский](https://github.com/avajs/ava-docs/blob/main/ru_RU/docs/recipes/typescript.md)

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
