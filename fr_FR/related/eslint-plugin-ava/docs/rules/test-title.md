___
**Note du traducteur**

C'est la traduction du fichier [test-title.md](https://github.com/avajs/eslint-plugin-ava/blob/master/docs/rules/test-title.md). Voici un [lien](https://github.com/avajs/eslint-plugin-ava/compare/216cd688cded0f2b79f3f652dc2eb43353f08fc4...master#diff-ed02b0af8dd256929d27a1e08192e303) vers les différences avec le master de eslint-plugin-ava (Si en cliquant sur le lien, vous ne trouvez pas le fichier `test-title.md` parmi les fichiers modifiés, vous pouvez donc en déduire que la traduction est à jour).
___
# S'assurer que les tests ont un titre

Traductions : [English](https://github.com/avajs/eslint-plugin-ava/blob/master/docs/rules/test-title.md)

Les tests devraient avoir un titre.


## Échoue

```js
/*eslint ava/test-title: ["error", "if-multiple"]*/
import test from 'ava';

test(t => {
	t.pass();
});

test(t => {
	t.pass();
});

/*eslint ava/test-title: ["error", "always"]*/
import test from 'ava';

test(t => {
	t.pass();
});
```


## Passe

```js
/*eslint ava/test-title: ["error", "if-multiple"]*/
import test from 'ava';

test(t => {
	t.pass();
});

test('foo', t => {
	t.pass();
});

/*eslint ava/test-title: ["error", "always"]*/
import test from 'ava';

test('foo', t => {
	t.pass();
});
```

## Options

La règle prend une option, un string, qui peut être soit `"always"` (toujours) ou soit `"if-multiple"` (si-plusieurs). La valeur par défaut est `"if-multiple"`. Si l'option est définie à `"if-multiple"`, la règle se déclenche que s'il y a plusieurs tests dans un fichier.

Vous pouvez définir l'option dans la configuration comme ceci :

```js
"ava/test-title": ["error", "always"]
```
