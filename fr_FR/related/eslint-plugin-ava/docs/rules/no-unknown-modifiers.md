___
**Note du traducteur**

C'est la traduction du fichier [no-unknown-modifiers.md](https://github.com/avajs/eslint-plugin-ava/blob/master/docs/rules/no-unknown-modifiers.md). Voici un [lien](https://github.com/avajs/eslint-plugin-ava/compare/7cfdf3444ad3567ab40644953e754a49cd5fce78...master#diff-a0ba13ebe08ce474c12d590c29c27bb5) vers les différences avec le master de eslint-plugin-ava (Si en cliquant sur le lien, vous ne trouvez pas le fichier `no-unknown-modifiers.md` parmi les fichiers modifiés, vous pouvez donc en déduire que la traduction est à jour).
___
# Empêcher l'utilisation de modificateurs de test inconnus

Traductions : [English](https://github.com/avajs/eslint-plugin-ava/blob/master/docs/rules/no-unknown-modifiers.md)

Empêche l'utilisation de [modificateurs de test inconnus](https://github.com/avajs/ava-docs/blob/master/fr_FR/readme.md#api).


## Échoue

```js
import test from 'ava';

test.onlu(t => {});
test.seril(t => {});
test.cb.onlu(t => {});
test.beforeeach(t => {});
test.unknown(t => {});
```


## Passe

```js
import test from 'ava';

test.only(t => {});
test.serial(t => {});
test.cb.only(t => {});
test.beforeEach(t => {});
```
