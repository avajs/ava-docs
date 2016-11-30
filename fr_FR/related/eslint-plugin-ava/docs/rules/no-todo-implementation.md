___
**Note du traducteur**

C'est la traduction du fichier [no-todo-implementation.md](https://github.com/avajs/eslint-plugin-ava/blob/master/docs/rules/no-todo-implementation.md). Voici un [lien](https://github.com/avajs/eslint-plugin-ava/compare/8e06ac80669622d5a85353994042de86cf27774c...master#diff-88dccb24456c40d7bcaf1c8e7517dcd3) vers les différences avec le master de eslint-plugin-ava (Si en cliquant sur le lien, vous ne trouvez pas le fichier `no-todo-implementation.md` parmi les fichiers modifiés, vous pouvez donc en déduire que la traduction est à jour).
___
# S'assurer que `test.todo()` n'a pas une fonction d'implémentation

Traductions : [English](https://github.com/avajs/eslint-plugin-ava/blob/master/docs/rules/no-todo-implementation.md)

[`test.todo()`](https://github.com/avajs/ava#test-placeholders-todo) est destiné aux futurs tests. Il n’est pas destiné à recevoir une fonction pour implémenter le test qu'on lui donne, AVA lancera une erreur. Si vous avez ajouté une implémentation, vous vouliez sans doute supprimer le modificateur `todo`.


## Échoue

```js
import test from 'ava';

test.todo('title', t => {
	// ...
});

test.todo(t => {
	// ...
});
```


## Passe

```js
import test from 'ava';

test.todo('title');

test(t => {
	// ...
});
```
