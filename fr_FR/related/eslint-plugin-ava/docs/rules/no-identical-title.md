___
**Note du traducteur**

C'est la traduction du fichier [no-identical-title.md](https://github.com/sindresorhus/eslint-plugin-ava/blob/master/docs/rules/no-cb-test.md). Voici un [lien](https://github.com/sindresorhus/eslint-plugin-ava/compare/39bf10ba7ffe7b0b54c7fe636a6d240cd9a48d88...master#diff-71a0207aeafb9ca6c33c3d06982b1baa) vers les différences avec le master de AVA (Si en cliquant sur le lien, vous ne trouvez pas le fichier `no-identical-title.md` parmi les fichiers modifiés, vous pouvez donc en déduire que la traduction est à jour).
___
# S'assurer que chaque test a un titre différent

Interdit les tests avec des titres identiques car il est difficile de les différencier.


## Échoue

```js
import test from 'ava';

test('foo', t => {
	t.pass();
});

test('foo', t => {
	t.pass();
});
```


## Passe

```js
import test from 'ava';

test(t => {
	t.pass();
});

test('foo', t => {
	t.pass();
});

test('bar', t => {
	t.pass();
});
```
