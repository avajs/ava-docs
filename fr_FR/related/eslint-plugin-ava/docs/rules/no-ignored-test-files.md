___
**Note du traducteur**

C'est la traduction du fichier [no-ignored-test-files.md](https://github.com/sindresorhus/eslint-plugin-ava/blob/master/docs/rules/no-ignored-test-files.md). Voici un [lien](https://github.com/sindresorhus/eslint-plugin-ava/compare/ed4057b003e8b1becc085b114830a0177714b7bf...master#diff-dfdc73f3a1c1fc001ea0161104cf3d13) vers les différences avec le master de AVA (Si en cliquant sur le lien, vous ne trouvez pas le fichier `no-ignored-test-files.md` parmi les fichiers modifiés, vous pouvez donc en déduire que la traduction est à jour).
___
# S'assurer qu'aucun test ne soit écrit dans des fichiers ignorés

Lors de la recherche des tests, AVA ignore les fichiers contenus dans `node_modules` ou dans les dossiers nommés `fixtures` ou `helpers`. Par défaut, il va chercher dans `test.js test-*.js test/**/*.js **/__tests__/**/*.js **/*.test.js`, que vous pouvez remplacer en spécifiant un chemin lors du lancement de AVA ou dans la [configuration de AVA dans le fichier `package.json`](https://github.com/sindresorhus/ava#configuration).

Cette règle vérifira que les fichiers qui créent des tests sont dans les fichiers recherchés et non dans des dossiers ignorés. Il examinera ç partir de la racine du projet pour être dans le dossier le plus proche contenant un fichier `package.json`, et ne fera rien s'il n'en trouve pas. Les fichiers de test dans `node_modules` ne seront pas vérifiés car ils sont ignorés par ESLint.

Notez que cette règle ne pourra pas vous avertir correctement si vous utilisez AVA en spécifiant les fichiers dans la ligne de commande ( `ava "lib/**/*.test.js"` ). Préférez la configuration AVA comme décrite dans le lien ci-dessus.

## Échoue

```js
// Fichier : test/foo/fixtures/bar.js
// Invalide car dans le répertoire `fixtures`
import test from 'ava';

test('foo', t => {
	t.pass();
});

// Fichier : test/foo/helpers/bar.js
// Invalide car dans le répertoire `helpers`
import test from 'ava';

test('foo', t => {
	t.pass();
});

// Fichier : lib/foo.test.js
// Invalide car il n'est pas dans les fichiers recherchés
import test from 'ava';

test('foo', t => {
	t.pass();
});

// Fichier : test.js
// avec { "files": ["lib/**/*.test.js", "utils/**/*.test.js"] }
// soit dans `package.json` sous la 'clef ava' ou dans les options de règle
// Invalide car il n'est pas dans les fichiers recherchés
import test from 'ava';

test('foo', t => {
	t.pass();
});
```


## Passe

```js
// Fichier : test/foo/not-fixtures/bar.js
import test from 'ava';

test('foo', t => {
	t.pass();
});

// Fichier : test/foo/not-helpers/bar.js
import test from 'ava';

test('foo', t => {
	t.pass();
});

// Fichier : test.js
import test from 'ava';

test('foo', t => {
	t.pass();
});

// Fichier : lib/foo.test.js
// avec { "files": ["lib/**/*.test.js", "utils/**/*.test.js"] }
// soit dans `package.json` sous la 'clef ava' ou dans les options de règle
import test from 'ava';

test('foo', t => {
	t.pass();
});
```

## Options

Cette règle prend en charge les options suivantes :

`files`: Un tableau de strings représentant les fichiers glob que nous allons utiliser pour trouver des fichiers de test. Remplace la valeur par défaut et la configuration trouvée dans le fichier de package.json.

Vous pouvez définir les options comme ceci :

```js
"ava/no-ignored-test-files": ["error", {"files": ["lib/**/*.test.js", "utils/**/*.test.js"]}]
```
