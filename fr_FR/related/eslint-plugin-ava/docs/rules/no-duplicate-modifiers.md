___
**Note du traducteur**

C'est la traduction du fichier [no-duplicate-modifiers.md](https://github.com/avajs/eslint-plugin-ava/blob/master/docs/rules/no-duplicate-modifiers.md). Voici un [lien](https://github.com/avajs/eslint-plugin-ava/compare/417307800a6e5571666ff6becdecdea97b24873c...master#diff-470cd3a8a42da4be025dc7828b27072f) vers les différences avec le master de eslint-plugin-ava (Si en cliquant sur le lien, vous ne trouvez pas le fichier `no-duplicate-modifiers.md` parmi les fichiers modifiés, vous pouvez donc en déduire que la traduction est à jour).
___
# S'assurer qu'aucun test n'a de modificateur en doublon

Traductions : [English](https://github.com/avajs/eslint-plugin-ava/blob/master/docs/rules/no-duplicate-modifiers.md)

Interdit l'utilisation de [modificateur de test](https://github.com/avajs/ava-docs/blob/master/fr_FR/docs/01-writing-tests.md) en doublon.


## Échoue

```js
import test from 'ava';

test.only.only(t => {});
test.serial.serial(t => {});
test.cb.cb(t => {});
test.beforeEach.beforeEach(t => {});
test.only.only.cb(t => {});
```


## Passe

```js
import test from 'ava';

test.only(t => {});
test.serial(t => {});
test.cb.only(t => {});
test.beforeEach(t => {});
```
