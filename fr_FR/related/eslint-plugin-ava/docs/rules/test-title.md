# Ensure tests have a title

Tests should have a title.


## Échoue

```js
/*eslint ava/test-title: ["error", "if-multiple"]*/
import test from 'ava';

test(t => {
	t.pass();
});

test(t => {
	t.pass();
});

/*eslint ava/test-title: ["error", "always"]*/
import test from 'ava';

test(t => {
	t.pass();
});
```


## Passe

```js
/*eslint ava/test-title: ["error", "if-multiple"]*/
import test from 'ava';

test(t => {
	t.pass();
});

test('foo', t => {
	t.pass();
});

/*eslint ava/test-title: ["error", "always"]*/
import test from 'ava';

test('foo', t => {
	t.pass();
});
```

## Options

The rule takes one option, a string, which could be either `"always"` or `"if-multiple"`. The default is `"if-multiple"`. If the option is set to `"if-multiple"`, the rule will only trigger if there are multiple tests in a file.

You can set the option in configuration like this:

```js
"ava/test-title": ["error", "always"]
```
