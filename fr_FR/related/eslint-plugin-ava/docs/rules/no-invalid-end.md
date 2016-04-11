___
**Note du traducteur**

C'est la traduction du fichier [no-invalid-end.md](https://github.com/sindresorhus/eslint-plugin-ava/blob/master/docs/rules/no-invalid-end.md). Voici un [lien](https://github.com/sindresorhus/eslint-plugin-ava/compare/36e287f467bb01b1877ba9698190a9897434e5e8...master#diff-791776202d9a355f02b808f9b5f4a0fb) vers les différences avec le master de AVA (Si en cliquant sur le lien, vous ne trouvez pas le fichier `no-invalid-end.md` parmi les fichiers modifiés, vous pouvez donc en déduire que la traduction est à jour).
___
# S'assurer que `t.end()` est seulement appelé dans `test.cb()`

AVA échouera si `t.end()` est appelé dans une fonction de test qui n'est pas `cb`.


## Échoue

```js
import test from 'ava';

test('quelques tests', t => {
	t.pass();
	t.end();
});
```


## Passe

```js
import test from 'ava';

test('quelques tests', t => {
	t.pass();
});

test.cb('quelques tests', t => {
	t.pass();
	t.end();
});
```
