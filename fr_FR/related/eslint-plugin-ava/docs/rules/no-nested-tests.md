___
**Note du traducteur**

C'est la traduction du fichier [no-nested-tests.md](https://github.com/avajs/eslint-plugin-ava/blob/main/docs/rules/no-nested-tests.md). Voici un [lien](https://github.com/avajs/eslint-plugin-ava/compare/dee1802d39e22aec0915d5067062356f5abfbd84...main#diff-6383a972100eb6ef808f4aea8959b38bad794e4810e23086b3488452def8f022) vers les différences avec le master de eslint-plugin-ava (Si en cliquant sur le lien, vous ne trouvez pas le fichier `no-nested-tests.md` parmi les fichiers modifiés, vous pouvez donc en déduire que la traduction est à jour).
___
# S'assurer qu'aucun test soit imbriqué

Traductions : [English](https://github.com/avajs/eslint-plugin-ava/blob/main/docs/rules/no-nested-tests.md)

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
