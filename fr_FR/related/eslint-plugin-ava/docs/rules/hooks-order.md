___
**Note du traducteur**

C'est la traduction du fichier [hooks-order.md.md](https://github.com/avajs/eslint-plugin-ava/blob/main/docs/rules/hooks-order.md.md). Voici un [lien](https://github.com/avajs/eslint-plugin-ava/compare/7542453058c30ebbc79c7bfeb689492fce226d8f...main#diff-7475dc463133f15a5a23f4d1ceb1aa3e7c467857a7568f926d1de69fb1ce908b) vers les différences avec le master de eslint-plugin-ava (Si en cliquant sur le lien, vous ne trouvez pas le fichier `hooks-order.md` parmi les fichiers modifiés, vous pouvez donc en déduire que la traduction est à jour).
___
# Imposer l'ordre des hooks de test

Traductions : [English](https://github.com/avajs/eslint-plugin-ava/blob/main/docs/rules/hooks-order.md)

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
const test = require('ava');

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
const test = require('ava');

test('foo', t => {
	t.true(true);
});

test.before(t => {
	doFoo();
});
```

## Passe

```js
const test = require('ava');

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
