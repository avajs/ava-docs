___
**Note du traducteur**

C'est la traduction du fichier [no-import-test-files.md](https://github.com/avajs/eslint-plugin-ava/blob/master/docs/rules/no-import-test-files.md). Voici un [lien](https://github.com/avajs/eslint-plugin-ava/compare/c5994700e90e5fef78c1bf50991bb84a92d202b4...master#diff-552c4066c39e7065f2f0ada5a41d9268) vers les différences avec le master de eslint-plugin-ava (Si en cliquant sur le lien, vous ne trouvez pas le fichier `no-import-test-files.md` parmi les fichiers modifiés, vous pouvez donc en déduire que la traduction est à jour).
___
# S'assurer qu'aucun fichier de test n'est importé de n'importe où

Traductions : [English](https://github.com/avajs/eslint-plugin-ava/blob/master/docs/rules/no-import-test-files.md)

Cette règle vérifiera que vous n'importez aucun fichier de test. Il considèrera la racine du projet avec le dossier le plus proche contenant un fichier `package.json` et ne fera rien s'il ne peut pas en trouver un. Les fichiers de test dans `node_modules` ne seront pas vérifiés car ils sont ignorés par ESLint.


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


## Passe

```js
// Fichier : src/index.js
import sinon from 'sinon';

```

```js
// Fichier : src/index.js
import utils from './utils';
```

## Options

Cette règle prend en charge les options suivantes :

* `extensions` : un tableau des extensions des fichiers reconnus par AVA en tant que fichiers tests ou helpers. Remplace *les deux configurations* `babel.extensions` *et* `extensions` utilisées par AVA.
* `files` : un tableau de glob patterns pour sélectionner les fichiers de test. Remplace la configuration `files` utilisées par AVA.

Consultez aussi la [configuration de AVA](https://github.com/avajs/ava-docs/blob/master/fr_FR/docs/06-configuration.md#options).

Vous pouvez définir l'option de cette manière :

```js
"ava/no-import-test-files": ["error", {"files": ["lib/**/*.test.js", "utils/**/*.test.js"]}]
```
