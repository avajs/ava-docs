# Ensure `t.end()` is the last statement executed.

`t.end()` should mark the end of your test, and additional statements should not be executed.

## Échoue

```js
import test from 'ava';

test.cb(t => {
	t.end();
	t.is(1, 1);
});

test.cb(t => {
	t.end();
	console.log('at the end');
});
```


## Passe

```js
import test from 'ava';

test.cb(t => {
	t.is(1, 1);
	t.end();
});
import test from 'ava';

test.cb(t => {
	if (a) {
		// Allowed because no further statements are reachable.
		return t.end();
	}
	if (b) {
		t.end();
		return;
	}
	t.is(1, 1);
	t.end();
});

```
