___
**Note du traducteur**

C'est la traduction du fichier [no-import-test-files.md](https://github.com/avajs/eslint-plugin-ava/blob/master/docs/rules/no-import-test-files.md). Voici un [lien](https://github.com/avajs/eslint-plugin-ava/compare/fb3ac90912cea33a8259b2828e06fc05797b983b...master#diff-552c4066c39e7065f2f0ada5a41d9268) vers les différences avec le master de eslint-plugin-ava (Si en cliquant sur le lien, vous ne trouvez pas le fichier `no-import-test-files.md` parmi les fichiers modifiés, vous pouvez donc en déduire que la traduction est à jour).
___
# S'assurer qu'aucun fichier de test n'est importé de n'importe où

Traductions : [English](https://github.com/avajs/eslint-plugin-ava/blob/master/docs/rules/no-import-test-files.md)

Cette règle vérifiera que vous n'importez aucun fichier de test. Il considèrera la racine du projet avec le dossier le plus proche contenant un fichier `package.json` et ne fera rien s'il ne peut pas en trouver un. Les fichiers de test dans `node_modules` ne seront pas vérifiés car ils sont ignorés par ESLint.

Notez que cette règle ne pourra pas vous avertir correctement si vous utilisez AVA en spécifiant les fichiers par la ligne de commande ( `ava "lib/**/*.test.js"` ). Préférez la configuration de AVA comme décrit [ici](https://github.com/avajs/ava-docs/blob/master/fr_FR/readme.md#configuration).

## Échoue

```js
// Fichier : src/index.js
// Invalide car *.test.js est considéré comme un fichier de test.
import tests from './index.test.js';
```

```js
// Fichier : src/index.js
// Invalide car n'importe quel fichier dans __tests__ est considéré comme un fichier de test
import tests from './__tests__/index.js';

test('foo', t => {
	t.pass();
});
```

```js
// Fichier : utils/index.js
// avec { "files": ["lib/**/*.test.js", "utils/**/*.test.js"] }
// soit dans `package.json` sous la clé 'ava' ou soit dans les options
// Invalide car le fichier importé correspond à lib/**/*.test.js
import tests from '../lib/index.test.js';

test('foo', t => {
	t.pass();
});
```


## Passe

```js
// Fichier : src/index.js
import sinon from 'sinon';

```

```js
// Fichier : src/index.js
import utils from './utils';
```

```js
// Fichier : lib/index.js
// avec { "files": ["lib/**/*.test.js", "utils/**/*.test.js"] }
// soit dans `package.json` sous la clé 'ava' ou soit dans les options
import utils from '../utils/index.js';
```

## Options

Cette règle prend en charge les options suivantes :

`files`: Un tableau de strings représentant les fichiers glob que nous allons utiliser pour trouver des fichiers de test. Remplace la valeur par défaut et la configuration trouvée dans le fichier de package.json.

Vous pouvez définir les options comme ceci :

```js
"ava/no-ignored-test-files": ["error", {"files": ["lib/**/*.test.js", "utils/**/*.test.js"]}]
```
