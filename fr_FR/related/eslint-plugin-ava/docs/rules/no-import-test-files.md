___
**Note du traducteur**

C'est la traduction du fichier [no-import-test-files.md](https://github.com/avajs/eslint-plugin-ava/blob/master/docs/rules/no-import-test-files.md). Voici un [lien](https://github.com/avajs/eslint-plugin-ava/compare/7542453058c30ebbc79c7bfeb689492fce226d8f...main#diff-552c4066c39e7065f2f0ada5a41d9268) vers les différences avec le master de eslint-plugin-ava (Si en cliquant sur le lien, vous ne trouvez pas le fichier `no-import-test-files.md` parmi les fichiers modifiés, vous pouvez donc en déduire que la traduction est à jour).
___
# S'assurer qu'aucun fichier de test n'est importé de n'importe où

Traductions : [English](https://github.com/avajs/eslint-plugin-ava/blob/master/docs/rules/no-import-test-files.md)

Cette règle vérifiera que vous n'importez aucun fichier de test. Il considèrera la racine du projet avec le dossier le plus proche contenant un fichier `package.json` et ne fera rien s'il ne peut pas en trouver un. Les fichiers de test dans `node_modules` ne seront pas vérifiés car ils sont ignorés par ESLint.


## Échoue

```js
// Fichier : src/index.js
// Invalide car *.test.js est considéré comme un fichier de test.
const tests = require('./index.test.js');
```

```js
// Fichier : src/index.js
// Invalide car n'importe quel fichier dans __tests__ est considéré comme un fichier de test
const tests = require('./__tests__/index.js');

test('foo', t => {
	t.pass();
});
```


## Passe

```js
// Fichier : src/index.js
const sinon = require('sinon');

```

```js
// Fichier : src/index.js
const utils = require('./utils');
```

## Options

Cette règle prend en charge les options suivantes :

* `extensions` : un tableau des extensions des fichiers reconnus par AVA en tant que fichiers tests ou helpers. Remplace *les deux configurations* `babel.extensions` *et* `extensions` utilisées par AVA.
* `files` : un tableau de glob patterns pour sélectionner les fichiers de test. Remplace la configuration `files` utilisées par AVA.

Consultez aussi la [configuration de AVA](https://github.com/avajs/ava-docs/blob/main/fr_FR/docs/06-configuration.md#options).

Vous pouvez définir l'option de cette manière :

```js
"ava/no-import-test-files": ["error", {"files": ["lib/**/*.test.js", "utils/**/*.test.js"]}]
```
