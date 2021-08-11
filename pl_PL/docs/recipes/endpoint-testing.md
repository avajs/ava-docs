# Testy endpoint

Tłumaczenie: [Español](https://github.com/avajs/ava-docs/blob/main/es_ES/docs/recipes/endpoint-testing.md), [Français](https://github.com/avajs/ava-docs/blob/main/fr_FR/docs/recipes/endpoint-testing.md), [Italiano](https://github.com/avajs/ava-docs/blob/main/it_IT/docs/recipes/endpoint-testing.md), [日本語](https://github.com/avajs/ava-docs/blob/main/ja_JP/docs/recipes/endpoint-testing.md), [Português](https://github.com/avajs/ava-docs/blob/main/pt_BR/docs/recipes/endpoint-testing.md), [Русский](https://github.com/avajs/ava-docs/blob/main/ru_RU/docs/recipes/endpoint-testing.md), [简体中文](https://github.com/avajs/ava-docs/blob/main/zh_CN/docs/recipes/endpoint-testing.md)

AVA nie ma wbudowanej metody testowania punktów końcowych, ale możesz na przykład użyć dowolnego wybranego klienta HTTP [`got`](https://github.com/sindresorhus/got). Będziesz także musiał uruchomić serwer HTTP, najlepiej na unikalnym porcie, abyś mógł równolegle uruchamiać testy. Do tego zalecamy [`test-listen`](https://github.com/zeit/test-listen).

Ponieważ testy są uruchamiane jednocześnie, najlepiej jest utworzyć nową instancję serwera przynajmniej dla każdego pliku testowego, ale być może nawet dla każdego testu. Można to osiągnąć za pomocą hooków `test.before()` i `test.beforeEach()` oraz `t.context`. Jeśli uruchomisz serwer za pomocą hooka `test.before()` powinieneś upewnić się, że testy wykonujesz szeregowo.

Sprawdź poniższy przykład:

```js
const http = require('http');
const test = require('ava');
const got = require('got');
const listen = require('test-listen');
const app = require('../app');

test.before(async t => {
	t.context.server = http.createServer(app);
	t.context.baseUrl = await listen(t.context.server);
});

test.after.always(t => {
	t.context.server.close();
});

test.serial('get /user', async t => {
	const res = await got('/user', { baseUrl: t.context.baseUrl, json: true });
	t.is(res.body.email, 'ava@rocks.com');
});
```

Inne biblioteki, które mogą Ci się przydać:

- [`supertest`](https://github.com/visionmedia/supertest)
- [`get-port`](https://github.com/sindresorhus/get-port)

