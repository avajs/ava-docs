___
**Note du traducteur**

C'est la traduction du fichier [prefer-power-assert.md](https://github.com/avajs/eslint-plugin-ava/blob/main/docs/rules/prefer-power-assert.md). Voici un [lien](https://github.com/avajs/eslint-plugin-ava/compare/dee1802d39e22aec0915d5067062356f5abfbd84...main#diff-fe8033f9589d5fecab65926d436fa69c6744f4cb7858406ef6a6ce65226854cb) vers les différences avec le master de eslint-plugin-ava (Si en cliquant sur le lien, vous ne trouvez pas le fichier `prefer-power-assert.md` parmi les fichiers modifiés, vous pouvez donc en déduire que la traduction est à jour).
___
# Autoriser uniquement l'utilisation des assertions qui n'ont pas d'alternatives dans power-assert

Traductions : [English](https://github.com/avajs/eslint-plugin-ava/blob/main/docs/rules/prefer-power-assert.md)

- [`t.assert()`](https://github.com/avajs/ava-docs/blob/main/fr_FR/docs/03-assertions.md#assertvalue-message)
- [`t.deepEqual()`](https://github.com/avajs/ava-docs/blob/main/fr_FR/docs/03-assertions.md#deepequalvalue-expected-message)
- [`t.notDeepEqual()`](https://github.com/avajs/ava-docs/blob/main/fr_FR/docs/03-assertions.md#notdeepequalvalue-expected-message)
- [`t.like()`](https://github.com/avajs/ava-docs/blob/main/fr_FR/docs/03-assertions.md#likevalue-selector-message)
- [`t.throws()`](https://github.com/avajs/ava-docs/blob/main/fr_FR/docs/03-assertions.md#throwsfn-expected-message)
- [`t.notThrows()`](https://github.com/avajs/ava-docs/blob/main/fr_FR/docs/03-assertions.md#notthrowsfn-message)
- [`t.pass()`](https://github.com/avajs/ava-docs/blob/main/fr_FR/docs/03-assertions.md#passmessage)
- [`t.fail()`](https://github.com/avajs/ava-docs/blob/main/fr_FR/docs/03-assertions.md#failmessage)

Utile pour les personnes voulant complètement comprendre la puissance de [power-assert](https://github.com/power-assert-js/power-assert).

## Échoue

```js
const test = require('ava');

test('foo', t => {
	t.truthy(foo);
	t.falsy(foo);
	t.true(foo === bar);
	t.false(foo === bar);
	t.is(foo, bar);
	t.not(foo, bar);
	t.regex(foo, bar);
	t.ifError(error);
});
```

## Passe

```js
const test = require('ava');

test('foo', t => {
	t.assert(foo === bar);
	t.deepEqual(foo, bar);
	t.notDeepEqual(foo, bar);
	t.throws(foo);
	t.notThrows(bar);
});
```
