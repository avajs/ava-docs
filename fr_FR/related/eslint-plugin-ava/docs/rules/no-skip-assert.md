___
**Note du traducteur**

C'est la traduction du fichier [no-skip-assert.md](https://github.com/avajs/eslint-plugin-ava/blob/master/docs/rules/no-skip-assert.md). Voici un [lien](https://github.com/avajs/eslint-plugin-ava/compare/c3d99fb076f5e579ba00f18fbedb92aeaf9df732...master#diff-5a0e2d6d6e66efe6059e266a9b4f5456) vers les différences avec le master de eslint-plugin-ava (Si en cliquant sur le lien, vous ne trouvez pas le fichier `no-skip-assert.md` parmi les fichiers modifiés, vous pouvez donc en déduire que la traduction est à jour).
___
# S'assurer qu'aucune assertion soit passée (skip)

Traductions : [English](https://github.com/avajs/eslint-plugin-ava/blob/master/docs/rules/no-skip-assert.md)

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
