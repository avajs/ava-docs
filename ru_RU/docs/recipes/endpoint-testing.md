___
**Примечание от переводчика**

Здесь содержится перевод файла [endpoint-testing.md](https://github.com/avajs/ava/blob/main/docs/recipes/endpoint-testing.md). Если при нажатии на [ссылку](https://github.com/avajs/ava/compare/4111f9483f2ff6a158b603735a712eee3ab074c4...master#diff-aee54ab6a703c02779edb3ebbb35e96f), содержащую последние правки в master ветке AVA, Вы не можете найти файл `endpoint-testing.md`, то можете считать что перевод актуален.
___
# Тестирование HTTP серверов

Переводы: [Español](https://github.com/avajs/ava-docs/blob/master/es_ES/docs/recipes/endpoint-testing.md), [Français](https://github.com/avajs/ava-docs/blob/master/fr_FR/docs/recipes/endpoint-testing.md), [Italiano](https://github.com/avajs/ava-docs/blob/master/it_IT/recipes/endpoint-testing.md), [日本語](https://github.com/avajs/ava-docs/blob/master/ja_JP/docs/recipes/endpoint-testing.md), [Português](https://github.com/avajs/ava-docs/blob/master/pt_BR/docs/recipes/endpoint-testing.md)

AVA не имеет встроенных средств для тестирования HTTP серверов, но Вы можете использовать любую библиотеку для этого. Воспользуемся [`supertest-as-promised`](https://github.com/WhoopInc/supertest-as-promised).

Так как все тесты запускаются одновременно, то лучше всего создавать новый сервер для каждого теста, потому что сервер может изменить свое состояние между тестами. Сделайте это при помощи `test.beforeEach` и `t.context`, или же воспользуйтесь factory методом:

```js
function makeApp() {
	const app = express();
	app.post('/signup', signupHandler);
	return app;
}
```

Далее, передайте объект сервера в supertest. Используйте Promises или async/await вместо метода `end` supertest'a:

```js
test('signup:Success', async t => {
	t.plan(2);

	const res = await request(makeApp())
		.post('/signup')
		.send({email: 'ava@rocks.com', password: '123123'});

	t.is(res.status, 200);
	t.is(res.body.email, 'ava@rocks.com');
});
```
