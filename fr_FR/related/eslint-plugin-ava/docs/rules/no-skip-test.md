___
**Note du traducteur**

C'est la traduction du fichier [no-skip-test.md](https://github.com/avajs/eslint-plugin-ava/blob/main/docs/rules/no-skip-test.md). Voici un [lien](https://github.com/avajs/eslint-plugin-ava/compare/dee1802d39e22aec0915d5067062356f5abfbd84...main#diff-5cf09f15e5cd3e69d32bed8f0c541206d34637767d6274bdcd3de11de2df9385) vers les différences avec le master de eslint-plugin-ava (Si en cliquant sur le lien, vous ne trouvez pas le fichier `no-skip-test.md` parmi les fichiers modifiés, vous pouvez donc en déduire que la traduction est à jour).
___
# S'assurer qu'aucun test soit passé (skip)

Traductions : [English](https://github.com/avajs/eslint-plugin-ava/blob/main/docs/rules/no-skip-test.md)

Il est facile de faire sauter un test avec `test.skip()`, puis de l'oublier. Il est visible dans les résultats, mais on peut encore facilement le manquer.

## Échoue

```js
const test = require('ava');

test('foo', t => {
	t.pass();
});

test.skip('bar', t => {
	t.pass();
});
```

## Passe

```js
const test = require('ava');

test('foo', t => {
	t.pass();
});

test('bar', t => {
	t.pass();
});
```
