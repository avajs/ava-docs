___
**Note du traducteur**

C'est la traduction du fichier [no-duplicate-modifiers.md](https://github.com/avajs/eslint-plugin-ava/blob/main/docs/rules/no-duplicate-modifiers.md). Voici un [lien](https://github.com/avajs/eslint-plugin-ava/compare/2f8a22677fc542e71f344afcef687d63cfa5f1bb...main#diff-ac99af7555fdc135598c0ab9cd7d0ae32bc12a11394dec14379a56aa2e51755d) vers les différences avec le master de eslint-plugin-ava (Si en cliquant sur le lien, vous ne trouvez pas le fichier `no-duplicate-modifiers.md` parmi les fichiers modifiés, vous pouvez donc en déduire que la traduction est à jour).
___
# S'assurer qu'aucun test n'a de modificateur en doublon

Traductions : [English](https://github.com/avajs/eslint-plugin-ava/blob/main/docs/rules/no-duplicate-modifiers.md)

Interdit l'utilisation de [modificateur de test](https://github.com/avajs/ava-docs/blob/main/fr_FR/docs/01-writing-tests.md) en doublon.

## Échoue

```js
const test = require('ava');

test.only.only(t => {});
test.serial.serial(t => {});
test.beforeEach.beforeEach(t => {});
```

## Passe

```js
const test = require('ava');

test.only(t => {});
test.serial(t => {});
test.beforeEach(t => {});
```
