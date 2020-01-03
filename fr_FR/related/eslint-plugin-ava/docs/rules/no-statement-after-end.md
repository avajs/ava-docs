___
**Note du traducteur**

C'est la traduction du fichier [no-statement-after-end.md](https://github.com/avajs/eslint-plugin-ava/blob/master/docs/rules/no-statement-after-end.md). Voici un [lien](https://github.com/avajs/eslint-plugin-ava/compare/7542453058c30ebbc79c7bfeb689492fce226d8f...master#diff-022e4562e2cef684c01e72e8a54af79f) vers les différences avec le master de eslint-plugin-ava (Si en cliquant sur le lien, vous ne trouvez pas le fichier `no-statement-after-end.md` parmi les fichiers modifiés, vous pouvez donc en déduire que la traduction est à jour).
___
# S'assurer que `t.end()` soit la dernière instruction exécutée

Traductions : [English](https://github.com/avajs/eslint-plugin-ava/blob/master/docs/rules/no-statement-after-end.md)

`t.end()` should mark the end of your test, and additional statements should not be executed.

## Échoue

```js
const test = require('ava');

test.cb(t => {
	t.end();
	t.is(1, 1);
});

test.cb(t => {
	t.end();
	console.log('à la fin');
});
```


## Passe

```js
const test = require('ava');

test.cb(t => {
	t.is(1, 1);
	t.end();
});
const test = require('ava');

test.cb(t => {
	if (a) {
		// Autorisé, car aucune nouvelle déclaration n'est réalisable
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
