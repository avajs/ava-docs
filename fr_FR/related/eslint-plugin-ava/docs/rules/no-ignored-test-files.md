___
**Note du traducteur**

C'est la traduction du fichier [no-ignored-test-files.md](https://github.com/avajs/eslint-plugin-ava/blob/master/docs/rules/no-ignored-test-files.md). Voici un [lien](https://github.com/avajs/eslint-plugin-ava/compare/7542453058c30ebbc79c7bfeb689492fce226d8f...main#diff-dfdc73f3a1c1fc001ea0161104cf3d13) vers les différences avec le master de eslint-plugin-ava (Si en cliquant sur le lien, vous ne trouvez pas le fichier `no-ignored-test-files.md` parmi les fichiers modifiés, vous pouvez donc en déduire que la traduction est à jour).
___
# S'assurer qu'aucun test ne soit écrit dans des fichiers ignorés

Traductions : [English](https://github.com/avajs/eslint-plugin-ava/blob/master/docs/rules/no-ignored-test-files.md)

Cette règle vérifiera que les fichiers qui créent des tests sont traités comme des fichiers de test par AVA. Il examinera à partir de la racine du projet pour être dans le dossier le plus proche contenant un fichier `package.json`, et ne fera rien s'il n'en trouve pas. Les fichiers de test dans `node_modules` ne seront pas vérifiés car ils sont ignorés par ESLint.


## Échoue

```js
// Fichier : test/_helper.js
// Invalide car c'est un helper
const test = require('ava');

test('foo', t => {
	t.pass();
});

// Fichier : lib/foo.js
// Invalide car ce n'est pas un fichier de test
const test = require('ava');

test('foo', t => {
	t.pass();
});
```


## Passe

```js
// Fichier : test/foo.js
const test = require('ava');

test('foo', t => {
	t.pass();
});
```

## Options

Cette règle prend en charge les options suivantes :

* `extensions` : un tableau des extensions des fichiers reconnus par AVA en tant que fichiers tests ou helpers. Remplace *les deux configurations* `babel.extensions` *et* `extensions` utilisées par AVA.
* `files` : un tableau de glob patterns pour sélectionner les fichiers de test. Remplace la configuration `files` utilisées par AVA.
* `helpers` : un tableau de glob patterns pour sélectionner les fichiers de helper. Remplace la configuration `helpers` utilisées par AVA.

Consultez aussi la [configuration de AVA](https://github.com/avajs/ava-docs/blob/main/fr_FR/docs/06-configuration.md#options).

Vous pouvez définir l'option de cette manière :

```js
"ava/no-ignored-test-files": ["error", {"files": ["lib/**/*.test.js", "utils/**/*.test.js"]}]
```
