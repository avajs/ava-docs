# Ensure test functions use `t` as their parameter

The convention is to have the parameter in AVA's test function be named `t`. Most rules in `eslint-plugin-ava` are based on that assumption.

### Échoue

```js
import test from 'ava';

test(foo => { // Incorrect name
	t.pass();
});

test((t, bar) => { // too many arguments
	t.pass();
});

test((bar, t) => { // too many arguments
	t.pass();
});
```

### Passe

```js
import test from 'ava';

test(() => {
	// ...
});

test(t => {
	t.pass();
});
```
