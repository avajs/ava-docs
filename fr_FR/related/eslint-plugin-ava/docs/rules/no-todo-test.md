___
**Note du traducteur**

C'est la traduction du fichier [no-todo-test.md](https://github.com/avajs/eslint-plugin-ava/blob/main/docs/rules/no-todo-test.md). Voici un [lien](https://github.com/avajs/eslint-plugin-ava/compare/dee1802d39e22aec0915d5067062356f5abfbd84...main#diff-f00e52a4fc7d00f25b55f4063ced0302166ed1ea09cd7f7fcb9cfc1a02ffdf13) vers les différences avec le master de eslint-plugin-ava (Si en cliquant sur le lien, vous ne trouvez pas le fichier `no-todo-test.md` parmi les fichiers modifiés, vous pouvez donc en déduire que la traduction est à jour).
___
# S'assurer qu'aucun `test.todo()` soit utilisé

Traductions : [English](https://github.com/avajs/eslint-plugin-ava/blob/main/docs/rules/no-todo-test.md)

Interdit l'utilisation de `test.todo()`. Vous pouvez effectuer cette opération pour seulement embarquer des fonctionnalités avec des spécifications entièrement écrites et qui passent.

## Échoue

```js
const test = require('ava');

test.todo('quelques tests');
```

## Passe

```js
const test = require('ava');

test('quelques tests', t => {
	// quelques implémentations
});
```
