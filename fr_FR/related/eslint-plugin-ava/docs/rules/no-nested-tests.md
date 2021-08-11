___
**Note du traducteur**

C'est la traduction du fichier [no-nested-tests.md](https://github.com/avajs/eslint-plugin-ava/blob/master/docs/rules/no-nested-tests.md). Voici un [lien](https://github.com/avajs/eslint-plugin-ava/compare/7542453058c30ebbc79c7bfeb689492fce226d8f...main#diff-b4a6895e60079f247fc18ca94c002857) vers les différences avec le master de eslint-plugin-ava (Si en cliquant sur le lien, vous ne trouvez pas le fichier `no-nested-tests.md` parmi les fichiers modifiés, vous pouvez donc en déduire que la traduction est à jour).
___
# S'assurer qu'aucun test soit imbriqué

Traductions : [English](https://github.com/avajs/eslint-plugin-ava/blob/master/docs/rules/no-nested-tests.md)

Dans AVA, vous ne pouvez pas imbriquer des tests, c'est à dire créer des tests à l'intérieur d'autres tests. Cela va conduire à un comportement étrange.


## Échoue

```js
const test = require('ava');

test('foo', t => {
	const result = foo();
	t.true(result.foo);

	test('bar', t => {
		t.true(result.bar);
	});
});
```


## Passe

```js
const test = require('ava');

test('foo', t => {
	const result = foo();
	t.true(result.foo);
});

test('bar', t => {
	const result = foo();
	t.true(result.bar);
});
```
