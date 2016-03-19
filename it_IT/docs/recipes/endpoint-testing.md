___
**Nota del traductor**

Esta es la traducción del archivo [endpoint-testing.md](https://github.com/sindresorhus/ava/blob/master/docs/recipes/endpoint-testing.md). Aquí hay un [enlace](https://github.com/sindresorhus/ava/compare/4111f9483f2ff6a158b603735a712eee3ab074c4...master#diff-aee54ab6a703c02779edb3ebbb35e96f) a las diferencias con la rama master de AVA (Si al hacer clic en el enlace no se encuentran modificaciones en el archivo `endpoint-testing.md`, será por que la traducción está actualizada).
___
# Endpoint testing

Translations: [Español](https://github.com/sindresorhus/ava-docs/blob/master/es_ES/docs/recipes/endpoint-testing.md), [Français](https://github.com/sindresorhus/ava-docs/blob/master/fr_FR/docs/recipes/endpoint-testing.md), [日本語](https://github.com/sindresorhus/ava-docs/blob/master/ja_JP/docs/recipes/endpoint-testing.md), [Português](https://github.com/sindresorhus/ava-docs/blob/master/pt_BR/docs/recipes/endpoint-testing.md), [Русский](https://github.com/sindresorhus/ava-docs/blob/master/ru_RU/docs/recipes/endpoint-testing.md)

AVA doesn't have a builtin method for testing endpoints, but you can use any assertion library with it. Let's use [`supertest-as-promised`](https://github.com/WhoopInc/supertest-as-promised).

Since tests run concurrently, it's best to create a fresh server instance for each test, because if we referenced the same instance, it could be mutated between tests. This can be accomplished with a `test.beforeEach` and `t.context`, or with simply a factory function:

```js
function makeApp() {
	const app = express();
	app.post('/signup', signupHandler);
	return app;
}
```

Next, just inject your server instance into supertest. The only gotcha is to use a promise or async/await syntax instead of the supertest `end` method:

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
