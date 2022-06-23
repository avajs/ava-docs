___
**Note du traducteur**

C'est la traduction du fichier [test-title-format.md](https://github.com/avajs/eslint-plugin-ava/blob/main/docs/rules/test-title-format.md). Voici un [lien](https://github.com/avajs/eslint-plugin-ava/compare/dee1802d39e22aec0915d5067062356f5abfbd84...main#diff-09bf6de604c58c6cd691ae47508dd7e3eaf64e9c446131949ad1ea3f7b176923) vers les différences avec le master de eslint-plugin-ava (Si en cliquant sur le lien, vous ne trouvez pas le fichier `test-title-format.md` parmi les fichiers modifiés, vous pouvez donc en déduire que la traduction est à jour).
___
# S'assurer que les titres de test ont un certain format

Traductions : [English](https://github.com/avajs/eslint-plugin-ava/blob/main/docs/rules/test-title-format.md)

Cette règle est utile lorsque vous souhaitez vous assurer que tous les titres de test correspondent à un format commun pour améliorer la lisibilité lorsque les tests échouent.

Par exemple, des titres comme `'Should throw when invalid.'`, `'Should fail when called.'` ou `'Should pass when using any number.'` pourrait être appliqué avec le motif suivant `'^Should (pass|fail|throw) when [\\w ]+\\.$'` (Notez les échappés `\`).

## Échoue

```js
/* eslint ava/test-title-format: ["error", {format: "^Devrait"}] */
const test = require('ava');

test('Ne commence pas par `Devrait`', t => {
	t.pass();
});
```

```js
/* eslint ava/test-title-format: ["error", {format: "\\.$"}] */
const test = require('ava');

test('Ne se termine pas par un point', t => {
	t.pass();
});
```

## Passe

```js
/* eslint ava/test-title-format: ["error", {format: "^Devrait"}] */
const test = require('ava');

test('Devrait passer les tests', t => {
	t.pass();
});

test('Devrait se comporter comme prévu', t => {
	t.pass();
});
```

```js
/* eslint ava/test-title-format: ["error", {format: "\\.$"}] */
const test = require('ava');

test('Se termine par un point.', t => {
	t.pass();
});
```

## Options

Cette règle prend en charge les options suivantes :

`format` : Une chaîne d'expression régulière qui doit correspondre aux titres des tests.

Vous pouvez définir l'option de cette manière :

```json
"ava/test-title-format": [
	"error",
	{
		"format": "^Should"
	}
]
```
