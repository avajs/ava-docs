# Teste de Endpoint

Traduções: [Español](https://github.com/sindresorhus/ava-docs/blob/master/es_ES/docs/recipes/endpoint-testing.md), [Français](endpoint-testing.md), [日本語](https://github.com/sindresorhus/ava-docs/blob/master/ja_JP/docs/recipes/endpoint-testing.md)

AVA não possui um método incorporado para testar endpoints, mas você pode usar qualquer biblioteca de asserção com ele. Vamos usar [`supertest-as-promised`](https://github.com/WhoopInc/supertest-as-promised).

Uma vez que os testes são executados simultaneamente, é melhor criar uma nova instância de servidor para cada teste, porque se nós referenciarmos a mesma instância, poderia ser mutada entre os testes. Isso pode ser feito com um `test.beforeEach` e um `t.context`, ou simplesmente com uma função factory:

```js
function makeApp() {
	const app = express();
	app.post('/signup', signupHandler);
	return app;
}
```

Em seguida, apenas injete sua instância de servidor em supertest. A única pegadinha é usar uma promessa ou sintaxe async/await em vez do método de supertest `end`:

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
