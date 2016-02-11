___
**Примечание от переводчика**

Здесь содержится перевод файл [endpoint-testing.md](https://github.com/sindresorhus/ava/blob/master/docs/recipes/endpoint-testing.md). Если при нажатии на [ссылку](), Вы не можете найти файл `endpoint-testing.md`, то можете считать что перевод актуален).
___
# Тестирование "конечных точек"

Переводы: [Español](https://github.com/sindresorhus/ava-docs/blob/master/es_ES/docs/recipes/endpoint-testing.md), [Français](https://github.com/sindresorhus/ava-docs/blob/master/fr_FR/docs/recipes/endpoint-testing.md), [日本語](https://github.com/sindresorhus/ava-docs/blob/master/ja_JP/docs/recipes/endpoint-testing.md), [Português](https://github.com/sindresorhus/ava-docs/blob/master/pt_BR/docs/recipes/endpoint-testing.md)

AVA не имеет встроенных средств для тестирования "конечных точек", но Вы можете использовать любую библиотеку для этого. Воспользуемся [`supertest-as-promised`](https://github.com/WhoopInc/supertest-as-promised).

Так как все тесты запускаются конкурентно, то лучше всего создавать отдельный инстанс сервера для каждого теста, потому как, инстанс может изменить свое состояние между тестами. Сделайте это при помощи `test.beforeEach` и `t.context`, или же воспользуйтесь фабричным методом:

```js
function makeApp() {
	const app = express();
	app.post('/signup', signupHandler);
	return app;
}
```

Далее, передайте инстанс сервера в supertest. Используйте синтаксис обещаний или async/await вместо метода `end` supertest'a:

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
