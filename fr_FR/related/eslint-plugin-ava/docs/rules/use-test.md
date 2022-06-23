___
**Note du traducteur**

C'est la traduction du fichier [use-test.md](https://github.com/avajs/eslint-plugin-ava/blob/main/docs/rules/use-test.md). Voici un [lien](https://github.com/avajs/eslint-plugin-ava/compare/9cd3fdf6b0b3a8fc9fc6a4a65aab297318d56226...main#diff-a19be9217f57aebf39df7fd205b406cbe0448a78f716fc8ea0cb64afcb171e90) vers les différences avec le master de eslint-plugin-ava (Si en cliquant sur le lien, vous ne trouvez pas le fichier `use-test.md` parmi les fichiers modifiés, vous pouvez donc en déduire que la traduction est à jour).
___
# S'assurer que AVA est importé avec la variable nommée `test`

Traductions : [English](https://github.com/avajs/eslint-plugin-ava/blob/main/docs/rules/use-test.md)

La convention est d'importer AVA et de l'affecter à une variable nommée `test`. La plupart des règles dans `eslint-plugin-ava` sont basées sur cette hypothèse.
Dans un fichier TypeScript (`.ts` ou `.tsx`), AVA peut être assigné à une variable nommée `anyTest` afin de définir les types de `t.context` (Consultez [Typage de t.context](https://github.com/avajs/ava-docs/blob/main/fr_FR/docs/recipes/typescript.md#typage-de-tcontext)). Les importations de type unique (`import type ... from 'ava'`) ne sont pas lintées.

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
const test = require('foo');
```

```ts
import anyTest from 'ava';
import type {TestInterface} from 'ava';

const test = anyTest as TestInterface<{foo: string}>;
```
