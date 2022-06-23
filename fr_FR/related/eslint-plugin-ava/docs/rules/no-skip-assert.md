___
**Note du traducteur**

C'est la traduction du fichier [no-skip-assert.md](https://github.com/avajs/eslint-plugin-ava/blob/main/docs/rules/no-skip-assert.md). Voici un [lien](https://github.com/avajs/eslint-plugin-ava/compare/dee1802d39e22aec0915d5067062356f5abfbd84...main#diff-a3233e68087574562bec60844745aa58638faba21b1a9da717153c2efdf80667) vers les différences avec le master de eslint-plugin-ava (Si en cliquant sur le lien, vous ne trouvez pas le fichier `no-skip-assert.md` parmi les fichiers modifiés, vous pouvez donc en déduire que la traduction est à jour).
___
# S'assurer qu'aucune assertion soit passée (skip)

Traductions : [English](https://github.com/avajs/eslint-plugin-ava/blob/main/docs/rules/no-skip-assert.md)

C'est facile de sauter une assertion avec `t.skip.xyz()` et de l'oublier.

## Échoue

```js
const test = require('ava');

test('un titre', t => {
	t.skip.is(1, 1);
});
```

## Passe

```js
const test = require('ava');

test('un titre', t => {
	t.is(1, 1);
});
```
