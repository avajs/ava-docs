___
**Note du traducteur**

C'est la traduction du fichier [hooks-order.md.md](https://github.com/avajs/eslint-plugin-ava/blob/master/docs/rules/hooks-order.md.md). Voici un [lien](https://github.com/avajs/eslint-plugin-ava/compare/7e20c648d52df11a3515b289196e9075cd9ecdac...master#diff-12864ccea1129a0091d7a534c26e113a) vers les différences avec le master de eslint-plugin-ava (Si en cliquant sur le lien, vous ne trouvez pas le fichier `hooks-order.md` parmi les fichiers modifiés, vous pouvez donc en déduire que la traduction est à jour).
___
# Imposer l'ordre des hooks de test

Traductions : [English](https://github.com/avajs/eslint-plugin-ava/blob/master/docs/rules/hooks-order.md)

Les hooks doivent être placés avant les tests et dans le bon ordre sémantique :

- `test.before(…);`
- `test.after(…);`
- `test.after.always(…);`
- `test.beforeEach(…);`
- `test.afterEach(…);`
- `test.afterEach.always(…);`
- `test(…);`

Cette règle est réparable tant qu'aucun autre code ne se trouve entre les hooks qui doivent être réorganisés.


## Échoue

```js
import test from 'ava';

test.after(t => {
	doFoo();
});

test.before(t => {
	doFoo();
});

test('foo', t => {
	t.true(true);
});
```

```js
import test from 'ava';

test('foo', t => {
	t.true(true);
});

test.before(t => {
	doFoo();
});
```


## Passe

```js
import test from 'ava';

test.before(t => {
	doFoo();
});

test.after(t => {
	doFoo();
});

test.after.always(t => {
	doFoo();
});

test.beforeEach(t => {
	doFoo();
});

test.afterEach(t => {
	doFoo();
});

test.afterEach.always(t => {
	doFoo();
});

test('foo', t => {
	t.true(true);
});
```
