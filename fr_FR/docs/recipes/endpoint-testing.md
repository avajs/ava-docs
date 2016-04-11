___
**Note du traducteur**

C'est la traduction du fichier [endpoint-testing.md](https://github.com/sindresorhus/ava/blob/master/docs/recipes/endpoint-testing.md). Voici un [lien](https://github.com/sindresorhus/ava/compare/349ee8177ae791362976be6b83690e1519ef64dc...master#diff-aee54ab6a703c02779edb3ebbb35e96f) vers les différences avec le master de AVA (Si en cliquant sur le lien, vous ne trouvez pas le fichier `endpoint-testing.md` parmi les fichiers modifiés, vous pouvez donc en déduire que la traduction est à jour).
___
# Tester un endpoint

Traductions : [English](https://github.com/sindresorhus/ava/blob/master/docs/recipes/endpoint-testing.md), [Español](https://github.com/sindresorhus/ava-docs/blob/master/es_ES/docs/recipes/endpoint-testing.md), [Italiano](https://github.com/sindresorhus/ava-docs/blob/master/it_IT/recipes/endpoint-testing.md),  [日本語](https://github.com/sindresorhus/ava-docs/blob/master/ja_JP/docs/recipes/endpoint-testing.md), [Português](https://github.com/sindresorhus/ava-docs/blob/master/pt_BR/docs/recipes/endpoint-testing.md), [Русский](https://github.com/sindresorhus/ava-docs/blob/master/ru_RU/docs/recipes/endpoint-testing.md), [简体中文](https://github.com/sindresorhus/ava-docs/blob/master/zh_CN/docs/recipes/endpoint-testing.md)

AVA n'a pas de méthodes internes pour tester les endpoints, mais nous pouvons le faire avec n'importe quelle bibliothèque. Utilisons [`supertest-as-promised`](https://github.com/WhoopInc/supertest-as-promised).

Étant donné que les tests sont exécutés simultanément, il est préférable de créer une nouvelle instance de serveur pour chaque test, car si nous les référençons à la même instance, elle pourrait être mutée entre les tests. Ceci peut être fait avec un `test.beforeEach` et `t.context`, ou simplement avec une fonction factory :

```js
function makeApp() {
	const app = express();
	app.post('/signup', signupHandler);
	return app;
}
```

Ensuite, il faut juste injecter votre instance de serveur dans supertest. Le seul subterfuge est d'utiliser une promesse ou la syntaxe async/await au lieu de la méthode `end` de supertest :

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
