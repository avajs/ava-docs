# Ensure `t.end()` is only called inside `test.cb()`

AVA will fail if `t.end()` is called in a non-`cb` test function.


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
