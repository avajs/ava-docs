# Ensure callback tests are explicitly ended

If you forget a `t.end();` in `test.cb()` the test will hang indefinitely.


## Échoue

```js
import test from 'ava';

test.cb(t => {
	t.pass();
});
```


## Passe

```js
import test from 'ava';

test.cb(t => {
	t.pass();
	t.end();
});

test.cb(t => {
	acceptsCallback(t.end);
});
```
