___
**Note du traducteur**

C'est la traduction du fichier [no-unknown-modifiers.md](https://github.com/avajs/eslint-plugin-ava/blob/master/docs/rules/no-unknown-modifiers.md). Voici un [lien](https://github.com/avajs/eslint-plugin-ava/compare/216cd688cded0f2b79f3f652dc2eb43353f08fc4...master#diff-a0ba13ebe08ce474c12d590c29c27bb5) vers les différences avec le master de eslint-plugin-ava (Si en cliquant sur le lien, vous ne trouvez pas le fichier `no-unknown-modifiers.md` parmi les fichiers modifiés, vous pouvez donc en déduire que la traduction est à jour).
___
# Empêcher l'utilisation de modificateurs de test inconnus

Traductions : [English](https://github.com/avajs/eslint-plugin-ava/blob/master/docs/rules/no-unknown-modifiers.md)

Empêche l'utilisation de modificateurs de test inconnus.


## Échoue

```js
import test = from 'ava';

test.onlu(t => {});
test.seril(t => {});
test.cb.onlu(t => {});
test.beforeeach(t => {});
test.unknown(t => {});
```


## Passe

```js
import test = from 'ava';

test.only(t => {});
test.serial(t => {});
test.cb.only(t => {});
test.beforeEach(t => {});
```
