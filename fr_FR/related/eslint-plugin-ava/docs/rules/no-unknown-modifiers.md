___
**Note du traducteur**

C'est la traduction du fichier [no-unknown-modifiers.md](https://github.com/avajs/eslint-plugin-ava/blob/main/docs/rules/no-unknown-modifiers.md). Voici un [lien](https://github.com/avajs/eslint-plugin-ava/compare/2f8a22677fc542e71f344afcef687d63cfa5f1bb...main#diff-938c307e29702701f0a0409aa6b0bdc45a11013751ca0c736e8d8b2a162ada70) vers les différences avec le master de eslint-plugin-ava (Si en cliquant sur le lien, vous ne trouvez pas le fichier `no-unknown-modifiers.md` parmi les fichiers modifiés, vous pouvez donc en déduire que la traduction est à jour).
___
# Empêcher l'utilisation de modificateurs de test inconnus

Traductions : [English](https://github.com/avajs/eslint-plugin-ava/blob/main/docs/rules/no-unknown-modifiers.md)

Empêche l'utilisation de [modificateurs de test inconnus](https://github.com/avajs/ava-docs/blob/main/fr_FR/docs/01-writing-tests.md).

## Échoue

```js
const test = require('ava');

test.onlu(t => {});
test.seril(t => {});
test.beforeeach(t => {});
test.unknown(t => {});
```

## Passe

```js
const test = require('ava');

test.only(t => {});
test.serial(t => {});
test.beforeEach(t => {});
```
