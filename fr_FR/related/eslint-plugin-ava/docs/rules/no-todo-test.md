___
**Note du traducteur**

C'est la traduction du fichier [no-todo-test.md](https://github.com/sindresorhus/eslint-plugin-ava/blob/master/docs/rules/no-todo-test.md). Voici un [lien](https://github.com/sindresorhus/eslint-plugin-ava/compare/2908de51778b76f5398be0e566baae23cab5fa1d...master#diff-ee78ff1db3f63a4c5de48c6c6c18fcdd) vers les différences avec le master de eslint-plugin-ava (Si en cliquant sur le lien, vous ne trouvez pas le fichier `no-todo-test.md` parmi les fichiers modifiés, vous pouvez donc en déduire que la traduction est à jour).
___
# S'assurer qu'aucun `test.todo()` soit utilisé

Interdit l'utilisation de `test.todo()`. Vous pouvez effectuer cette opération pour seulement embarquer des fonctionnalités avec des spécifications entièrement écrites et qui passent.


## Échoue

```js
import test from 'ava';

test.todo('quelques tests');
```


## Passe

```js
import test from 'ava';

test('quelques tests', t => {
	// quelques implémentations
});
```
