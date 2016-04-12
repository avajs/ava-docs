___
**Note du traducteur**

C'est la traduction du fichier [no-statement-after-end.md](https://github.com/sindresorhus/eslint-plugin-ava/blob/master/docs/rules/no-statement-after-end.md). Voici un [lien](https://github.com/sindresorhus/eslint-plugin-ava/compare/4a7f0b5357d35c4c56917832e53f36f93582fed1...master#diff-022e4562e2cef684c01e72e8a54af79f) vers les différences avec le master de eslint-plugin-ava (Si en cliquant sur le lien, vous ne trouvez pas le fichier `no-statement-after-end.md` parmi les fichiers modifiés, vous pouvez donc en déduire que la traduction est à jour).
___
# S'assurer que `t.end()` soit la dernière instruction exécutée

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
	console.log('à la fin');
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
