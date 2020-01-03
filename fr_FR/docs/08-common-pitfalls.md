___
**Note du traducteur**

C'est la traduction du fichier [common-pitfalls.md](https://github.com/avajs/ava/blob/master/docs/common-pitfalls.md). Voici un [lien](https://github.com/avajs/ava/compare/79b2ea30c125f44e4d47bdafdeec351cddb5911a...master#diff-ea157780fd005702cef8a1ddf5ec347b) vers les différences avec le master de AVA (Si en cliquant sur le lien, vous ne trouvez pas le fichier `common-pitfalls.md` parmi les fichiers modifiés, vous pouvez donc en déduire que la traduction est à jour).
___
# Pièges classiques

Traductions : [English](https://github.com/avajs/ava/blob/master/docs/08-common-pitfalls.md)

Si vous utilisez [ESLint](http://eslint.org/), vous pouvez installer [eslint-plugin-ava](https://github.com/avajs/eslint-plugin-ava). Il vous aidera à utiliser correctement AVA et ainsi vous évitera certains pièges classiques.

## AVA dans Docker

Si vous exécutez AVA dans Docker comme faisant partie de votre CI, vous devez ajuster les variables d'environnement adéquates. Plus précisément, en ajoutant `-e CI=true` dans la commande `docker exec`. Regardez [#751](https://github.com/avajs/ava/issues/751).

AVA utilise [is-ci](https://github.com/watson/is-ci) pour décider s'il se trouve ou pas dans un environnement de CI en utilisant [ces variables](https://github.com/watson/ci-info/blob/master/index.js).

## AVA et les limites du nombre de clients connectés

Vous utilisez peut-être un service qui permet seulement un nombre de connexions simultanées limitées. Par exemple, de nombreux services de base de données d'entreprises offrent un forfait gratuit avec une limite sur le nombre de clients pouvant être utiliser en même temps. AVA peut atteindre ces limites car il exécute plusieurs processus, mais les services bien écrit doivent émettre une erreur ou créer un goulot d'étranglement dans ces cas. Si celui que vous utilisez ne le fait pas, les tests se bloqueront.

Par défaut, AVA utilisera autant de processus qu'il y a de [cœur logique](https://superuser.com/questions/1105654/logical-vs-physical-cpu-performance) sur votre machine. Ceci est plafonné à deux dans un environnement CI.

Utilisez l'option `concurrency` pour limiter le nombre de processus exécutés. Par exemple, si votre abonnement au service permet 5 clients, vous devez exécuter AVA avec `concurrency=5` ou moins.

## Opérations asynchrones

Vous pouvez exécuter une opération asynchrone à l'intérieur d'un test et vous demander pourquoi elle ne se finit pas. Si votre opération asynchrone utilise des promesses, vous devez retourner la promesse :

```js
test('fetches foo', t => {
	return fetch().then(data => {
		t.is(data, 'foo');
	});
});
```

Mieux encore, l'utilisation de `async` / `await`:

```js
test('fetches foo', async t => {
	const data = await fetch();
	t.is(data, 'foo');
});
```

Si l'on utilise des callbacks, veuillez utilisez [`test.cb`](./01-writing-tests.md#prise-en-charge-du-callback):

```js
test.cb('fetches foo', t => {
	fetch((err, data) => {
		t.is(data, 'foo');
		t.end();
	});
});
```

Alternativement, vous pouvez transformer une fonction callback en promesse en utilisant quelque chose comme [`pify`](https://github.com/sindresorhus/pify).

```js
test('fetches foo', async t => {
	const data = await pify(fetch)();
	t.is(data, 'foo');
});
```

## Affectation des exceptions non interceptées par les tests

AVA [ne peut pas tracer les exceptions non interceptées](https://github.com/avajs/ava/issues/214) par le retour du test qui les déclenche. Les fonctions de prise de callback peuvent conduire à des exceptions non interceptées qui peuvent ensuite être difficiles à déboguer. Utilisez la transformation d'un callback en promesse et l'utilisation de `async`/`await`, comme dans l'exemple ci-dessus. Cela devrait permettre à AVA d'attraper l'exception et de l'attribuer au test correctement.

## Pourquoi les messages d'assertion améliorés ne s'affichent pas ?

Assurez-vous que le premier paramètre passé dans votre test est nommé `t`. C'est une exigence de [`power-assert`](https://github.com/power-assert-js/power-assert), la bibliothèque qui fournit les [messages améliorés](./03-assertions.md#messages-dassertions-améliorés).

```js
test('un est un', t => {
	t.assert(1 === 1);
});
```

Assurez-vous d'activer également [Babel](./recipes/babel.md).

## Partage de variables entre tests asynchrones

Par défaut, AVA exécute les tests simultanément. Cela peut poser problème si vos tests sont asynchrones et partagent des variables.

Prenons cet exemple bidon :

```js
const test = require('ava');

let count = 0;
const incr = async () => {
	await true;
	count = count + 1;
};

test.beforeEach('réinitialise le compteur', () => {
	count = 0;
});

test('incrémente une fois', async t => {
	await incr();
	t.is(count, 1);
});

test('incrémente deux fois', async t => {
	await incr();
	await incr();
	t.is(count, 2);
});
```

Les tests simultanés permettent aux tests asynchrones de s'exécuter plus rapidement, mais s'ils reposent sur un état partagé, cela peut entraîner des échecs de test inattendus. Si l'état partagé ne peut pas être évité, vous pouvez exécuter vos tests en série :

```js
const test = require('ava');

let count = 0;
const incr = async () => {
	await true;
	count = count + 1;
};

test.beforeEach('réinitialise le compteur', () => {
	count = 0;
});

test.serial('incrémente une fois', async t => {
	await incr();
	t.is(count, 1);
});

test.serial('incrémente deux fois', async t => {
	await incr();
	await incr();
	t.is(count, 2);
});
```

---

Votre problème n'est pas listé ici ? Soumettez un pull request ou commentez sur [cette issue](https://github.com/avajs/ava/issues/404).
