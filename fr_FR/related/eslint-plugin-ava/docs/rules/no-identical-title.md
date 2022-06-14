___
**Note du traducteur**

C'est la traduction du fichier [no-identical-title.md](https://github.com/avajs/eslint-plugin-ava/blob/main/docs/rules/no-identical-title.md). Voici un [lien](https://github.com/avajs/eslint-plugin-ava/compare/dee1802d39e22aec0915d5067062356f5abfbd84...main#diff-6d716ac64f662c61633a212f113b741efe1087433d172d6b5069ed9795cbeae2) vers les différences avec le master de eslint-plugin-ava (Si en cliquant sur le lien, vous ne trouvez pas le fichier `no-identical-title.md` parmi les fichiers modifiés, vous pouvez donc en déduire que la traduction est à jour).
___
# S'assurer que chaque test a un titre différent

Traductions : [English](https://github.com/avajs/eslint-plugin-ava/blob/main/docs/rules/no-identical-title.md)

Interdit les tests avec des titres identiques car il est difficile de les différencier.

## Échoue

```js
const test = require('ava');

test('foo', t => {
	t.pass();
});

test('foo', t => {
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
