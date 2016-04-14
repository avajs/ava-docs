___
**Note du traducteur**

C'est la traduction du fichier [no-skip-assert.md](https://github.com/sindresorhus/eslint-plugin-ava/blob/master/docs/rules/no-skip-assert.md). Voici un [lien](https://github.com/sindresorhus/eslint-plugin-ava/compare/199b7c1ba81d5a92c13440de49d69c3cf49d34dc...master#diff-5a0e2d6d6e66efe6059e266a9b4f5456) vers les différences avec le master de eslint-plugin-ava (Si en cliquant sur le lien, vous ne trouvez pas le fichier `no-skip-assert.md` parmi les fichiers modifiés, vous pouvez donc en déduire que la traduction est à jour).
___
# S'assurer qu'aucune assertion soit passée (skip)

C'est facile de sauter une assertion avec `t.skip.xyz()` et de l'oublier.


## Échoue

```js
import test from 'ava';

test('un titre', t => {
	t.skip.is(1, 1);
});
```


## Passe

```js
import test from 'ava';

test('un titre', t => {
	t.is(1, 1);
});
```
