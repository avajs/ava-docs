# Ensure no `test.todo()` is used

Disallow the use of `test.todo()`. You might want to do this to only ship features with specs fully written and passing.


## Échoue

```js
import test from 'ava';

test.todo('quelques tests');
```


## Passe

```js
import test from 'ava';

test('quelques tests', t => {
	// Some implementation
});
```
