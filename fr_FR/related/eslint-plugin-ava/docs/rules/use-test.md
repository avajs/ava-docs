# Ensure that AVA is imported with `test` as the variable name

The convention is to import AVA and assign it to a variable named `test`. Most rules in `eslint-plugin-ava` are based on that assumption.

### Échoue

```js
var ava = require('ava');
let ava = require('ava');
const ava = require('ava');
import ava from 'ava';
```

### Passe

```js
var test = require('ava');
let test = require('ava');
const test = require('ava');
import test from 'ava';

var test = require('foo');
import test from 'foo';
```
