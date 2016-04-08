# Ensure no tests are skipped

It's easy to make a test skipped with test.skip() and then forget about it. It's visible in the results, but still easily missed.


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
