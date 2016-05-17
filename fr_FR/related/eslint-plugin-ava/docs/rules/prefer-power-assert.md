___
**Note du traducteur**

C'est la traduction du fichier [prefer-power-assert.md](https://github.com/avajs/eslint-plugin-ava/blob/master/docs/rules/prefer-power-assert.md). Voici un [lien](https://github.com/avajs/eslint-plugin-ava/compare/216cd688cded0f2b79f3f652dc2eb43353f08fc4...master#diff-1172258630db86b27c631226ec642918) vers les différences avec le master de eslint-plugin-ava (Si en cliquant sur le lien, vous ne trouvez pas le fichier `prefer-power-assert.md` parmi les fichiers modifiés, vous pouvez donc en déduire que la traduction est à jour).
___
# Autoriser uniquement l'utilisation des assertions qui n'ont pas d'alternatives dans power-assert

Traductions : [English](https://github.com/avajs/eslint-plugin-ava/blob/master/docs/rules/prefer-power-assert.md)

- [`t.truthy()`](https://github.com/avajs/ava-docs/blob/master/fr_FR/readme.md#truthyvalue-message) __(Vous pouvez faire la [plupart des choses](https://github.com/avajs/ava-docs/blob/master/fr_FR/readme.md#messages-dassertions-améliorés) avec celui-ci)__
- [`t.deepEqual()`](https://github.com/avajs/ava-docs/blob/master/fr_FR/readme.md#deepequalvalue-expected-message)
- [`t.notDeepEqual()`](https://github.com/avajs/ava-docs/blob/master/fr_FR/readme.md#notdeepequalvalue-expected-message)
- [`t.throws()`](https://github.com/avajs/ava-docs/blob/master/fr_FR/readme.md#throwsfunctionpromise-error-message)
- [`t.notThrows()`](https://github.com/avajs/ava-docs/blob/master/fr_FR/readme.md#notthrowsfunctionpromise-message)
- [`t.pass()`](https://github.com/avajs/ava-docs/blob/master/fr_FR/readme.md#passmessage)
- [`t.fail()`](https://github.com/avajs/ava-docs/blob/master/fr_FR/readme.md#failmessage)

Utile pour les personnes voulant complètement comprendre la puissance de [power-assert](https://github.com/power-assert-js/power-assert).


## Échoue

```js
import test from 'ava';

test(t => {
	t.is(foo, bar);
});
```


## Passe

```js
import test from 'ava';

test(t => {
	t.truthy(foo === bar);
});
```
