___
**Note du traducteur**

C'est la traduction du fichier [prefer-power-assert.md](https://github.com/sindresorhus/eslint-plugin-ava/blob/master/docs/rules/prefer-power-assert.md). Voici un [lien](https://github.com/sindresorhus/eslint-plugin-ava/compare/9eaf1cde7f4789cc2f59a7627a1c21c0d827a2bd...master#diff-1172258630db86b27c631226ec642918) vers les différences avec le master de eslint-plugin-ava (Si en cliquant sur le lien, vous ne trouvez pas le fichier `prefer-power-assert.md` parmi les fichiers modifiés, vous pouvez donc en déduire que la traduction est à jour).
___
# Autoriser uniquement l'utilisation des assertions qui n'ont pas d'alternatives dans power-assert

- [`t.truthy()`](https://github.com/sindresorhus/ava#truthyvalue-message) __(Vous pouvez faire la [plupart des choses](https://github.com/sindresorhus/ava-docs/blob/master/fr_FR/readme.md#messages-dassertions-améliorés) avec celui-ci)__
- [`t.deepEqual()`](https://github.com/sindresorhus/ava-docs/blob/master/fr_FR/readme.md#deepequalvalue-expected-message)
- [`t.notDeepEqual()`](https://github.com/sindresorhus/ava-docs/blob/master/fr_FR/readme.md#notdeepequalvalue-expected-message)
- [`t.throws()`](https://github.com/sindresorhus/ava-docs/blob/master/fr_FR/readme.md#throwsfunctionpromise-error-message)
- [`t.notThrows()`](https://github.com/sindresorhus/ava-docs/blob/master/fr_FR/readme.md#notthrowsfunctionpromise-message)
- [`t.pass()`](https://github.com/sindresorhus/ava-docs/blob/master/fr_FR/readme.md#passmessage)
- [`t.fail()`](https://github.com/sindresorhus/ava-docs/blob/master/fr_FR/readme.md#failmessage)

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
