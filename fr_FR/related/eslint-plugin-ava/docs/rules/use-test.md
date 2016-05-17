___
**Note du traducteur**

C'est la traduction du fichier [use-test.md](https://github.com/avajs/eslint-plugin-ava/blob/master/docs/rules/use-test.md). Voici un [lien](https://github.com/avajs/eslint-plugin-ava/compare/216cd688cded0f2b79f3f652dc2eb43353f08fc4...master#diff-fa3d5dae0f30fb7e10aa7481e3d528b1) vers les différences avec le master de eslint-plugin-ava (Si en cliquant sur le lien, vous ne trouvez pas le fichier `use-test.md` parmi les fichiers modifiés, vous pouvez donc en déduire que la traduction est à jour).
___
# S'assurer que AVA est importé avec la variable nommée `test`

Traductions : [English](https://github.com/avajs/eslint-plugin-ava/blob/master/docs/rules/use-test.md)

La convention est d'importer AVA et de l'affecter à une variable nommée `test`. La plupart des règles dans `eslint-plugin-ava` sont basées sur cette hypothèse.

### Échoue

```js
var ava = require('ava');
let ava = require('ava');
const ava = require('ava');
import ava from 'ava';
```

### Passe

```js
var test = require('ava');
let test = require('ava');
const test = require('ava');
import test from 'ava';

var test = require('foo');
import test from 'foo';
```
