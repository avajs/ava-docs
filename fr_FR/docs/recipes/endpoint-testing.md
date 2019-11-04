___
**Note du traducteur**

C'est la traduction du fichier [endpoint-testing.md](https://github.com/avajs/ava/blob/master/docs/recipes/endpoint-testing.md). Voici un [lien](https://github.com/avajs/ava/compare/67e4deac2857503e5fac7d38da2d23014eb29724...master#diff-aee54ab6a703c02779edb3ebbb35e96f) vers les différences avec le master de AVA (Si en cliquant sur le lien, vous ne trouvez pas le fichier `endpoint-testing.md` parmi les fichiers modifiés, vous pouvez donc en déduire que la traduction est à jour).
___
# Tester un endpoint

Traductions : [English](https://github.com/avajs/ava/blob/master/docs/recipes/endpoint-testing.md), [Español](https://github.com/avajs/ava-docs/blob/master/es_ES/docs/recipes/endpoint-testing.md), [Italiano](https://github.com/avajs/ava-docs/blob/master/it_IT/docs/recipes/endpoint-testing.md),  [日本語](https://github.com/avajs/ava-docs/blob/master/ja_JP/docs/recipes/endpoint-testing.md), [Português](https://github.com/avajs/ava-docs/blob/master/pt_BR/docs/recipes/endpoint-testing.md), [Русский](https://github.com/avajs/ava-docs/blob/master/ru_RU/docs/recipes/endpoint-testing.md), [简体中文](https://github.com/avajs/ava-docs/blob/master/zh_CN/docs/recipes/endpoint-testing.md)

AVA n'a pas de méthodes internes pour tester les endpoints, mais vous pouvez utiliser n'importe quel client HTTP de votre choix, par exemple [`got`](https://github.com/sindresorhus/got). Vous devrez également démarrer un serveur HTTP, de préférence sur un port unique, afin de pouvoir exécuter des tests en parallèle. Pour cela, nous recommandons [`test-listen`](https://github.com/zeit/test-listen).

Étant donné que les tests sont exécutés simultanément, il est préférable de créer une nouvelle instance de serveur au moins pour chaque fichier de test, voire même pour chaque test. Ceci peut être accompli avec les hooks `test.before()` et `test.beforeEach()` et `t.context`. Si vous démarrez votre serveur à l’aide d’un hook `test.before()`, vous devez vous assurer d’exécuter vos tests en série.

Découvrez l'exemple ci-dessous :

```js
const http = require('http');
const test = require('ava');
const got = require('got');
const listen = require('test-listen');
const app = require('../app');

test.before(async t => {
	t.context.server = http.createServer(app);
	t.context.baseURL = await listen(t.context.server);
});

test.after.always(t => {
	t.context.server.close();
});

test.serial('get /user', async t => {
	const res = await got('/user', { baseURL: t.context.baseURL, json: true });
	t.is(res.body.email, 'ava@rocks.com');
});
```

D'autres bibliothèques pourraient vous être utiles : 

- [`supertest`](https://github.com/visionmedia/supertest)
- [`get-port`](https://github.com/sindresorhus/get-port)
