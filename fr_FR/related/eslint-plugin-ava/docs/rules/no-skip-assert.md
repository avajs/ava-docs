# Ensure no assertions are skipped

It's easy to make an assertion skipped with `t.skip.xyz()` and then forget about it.


## Échoue

```js
import test from 'ava';

test('some title', t => {
	t.skip.is(1, 1);
});
```


## Passe

```js
import test from 'ava';

test('some title', t => {
	t.is(1, 1);
});
```
