___
**Nota do tradutor**

Esta é a tradução de [endpoint-testing.md](https://github.com/avajs/ava/blob/main/docs/recipes/endpoint-testing.md). [Este link](https://github.com/avajs/ava/compare/0e6db13a918f0116927299271d91fff590328cb9...master) compara a versão em que se baseou esta tradução com a última versão disponível no branch `master` do AVA. Se não houver mudanças em `endpoint-testing.md`, então a tradução está atualizada.
___

# Teste de Endpoint

Traduções: [Español](https://github.com/avajs/ava-docs/blob/master/es_ES/docs/recipes/endpoint-testing.md), [Français](endpoint-testing.md), [Italiano](https://github.com/avajs/ava-docs/blob/master/it_IT/recipes/endpoint-testing.md), [日本語](https://github.com/avajs/ava-docs/blob/master/ja_JP/docs/recipes/endpoint-testing.md)

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
