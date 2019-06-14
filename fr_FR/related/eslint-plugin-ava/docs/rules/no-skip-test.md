___
**Note du traducteur**

C'est la traduction du fichier [no-skip-test.md](https://github.com/avajs/eslint-plugin-ava/blob/master/docs/rules/no-skip-test.md). Voici un [lien](https://github.com/avajs/eslint-plugin-ava/compare/5bdf745701159cd82d4bc125c1e685524b5066b6...master#diff-c9c8947bbb8248704dc64c3f0418475c) vers les différences avec le master de eslint-plugin-ava (Si en cliquant sur le lien, vous ne trouvez pas le fichier `no-skip-test.md` parmi les fichiers modifiés, vous pouvez donc en déduire que la traduction est à jour).
___
# S'assurer qu'aucun test soit passé (skip)

Traductions : [English](https://github.com/avajs/eslint-plugin-ava/blob/master/docs/rules/no-skip-test.md)

Il est facile de faire sauter un test avec `test.skip()`, puis de l'oublier. Il est visible dans les résultats, mais on peut encore facilement le manquer.

Cette règle est réparable. Cela supprimera le modificateur de test `.skip`.


## Échoue

```js
import test from 'ava';

test('foo', t => {
	t.pass();
});

test.skip('bar', t => {
	t.pass();
});
```


## Passe

```js
import test from 'ava';

test('foo', t => {
	t.pass();
});

test('bar', t => {
	t.pass();
});
```
