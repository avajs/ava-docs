# Ensure no tests are written in ignored files

When searching for tests, AVA ignores files contained in `node_modules` or folders named `fixtures` or `helpers`. By default, it will search in `test.js test-*.js test/**/*.js`, which you can override by specifying a path when launching AVA or in the [AVA configuration in the `package.json` file](https://github.com/sindresorhus/ava#configuration).

This rule will verify that files which create tests are in the searched files and not in ignored folders. It will consider the root of the project to be the closest folder containing a `package.json` file, and will not do anything if it can't find one. Test files in `node_modules` will not be linted as they are ignored by ESLint.

Note that this rule will not be able to warn correctly if you use AVA by specifying the files in the command line ( `ava "lib/**/*.test.js"` ). Prefer configuring AVA as described in the link above.

## Échoue

```js
// File: test/foo/fixtures/bar.js
// Invalid because in `fixtures` folder
import test from 'ava';

test('foo', t => {
	t.pass();
});

// File: test/foo/helpers/bar.js
// Invalid because in `helpers` folder
import test from 'ava';

test('foo', t => {
	t.pass();
});

// File: lib/foo.test.js
// Invalid because not in the searched files
import test from 'ava';

test('foo', t => {
	t.pass();
});

// File: test.js
// with { "files": ["lib/**/*.test.js", "utils/**/*.test.js"] }
// in either `package.json` under 'ava key' or in the rule options
// Invalid because not in the searched files
import test from 'ava';

test('foo', t => {
	t.pass();
});
```


## Passe

```js
// File: test/foo/not-fixtures/bar.js
import test from 'ava';

test('foo', t => {
	t.pass();
});

// File: test/foo/not-helpers/bar.js
import test from 'ava';

test('foo', t => {
	t.pass();
});

// File: test.js
import test from 'ava';

test('foo', t => {
	t.pass();
});

// File: lib/foo.test.js
// with { "files": ["lib/**/*.test.js", "utils/**/*.test.js"] }
// in either `package.json` under 'ava key' or in the rule options
import test from 'ava';

test('foo', t => {
	t.pass();
});
```

## Options

This rule supports the following options:

`files`: An array of strings representing the files glob that AVA will use to find test files. Overrides the default and the configuration found in the `package.json` file.

You can set the options like this:

```js
"ava/no-ignored-test-files": ["error", {"files": ["lib/**/*.test.js", "utils/**/*.test.js"]}]
```
