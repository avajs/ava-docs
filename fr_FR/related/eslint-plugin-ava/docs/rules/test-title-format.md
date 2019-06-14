___
**Note du traducteur**

C'est la traduction du fichier [test-title-format.md](https://github.com/avajs/eslint-plugin-ava/blob/master/docs/rules/test-title-format.md). Voici un [lien](https://github.com/avajs/eslint-plugin-ava/compare/a1846615ec4d8b7586b55a4faa2cf538d9e7b080...master#diff-b476dae00d584a873f775b322bcc81e7) vers les différences avec le master de eslint-plugin-ava (Si en cliquant sur le lien, vous ne trouvez pas le fichier `test-title-format.md` parmi les fichiers modifiés, vous pouvez donc en déduire que la traduction est à jour).
___
# S'assurer que les titres de test ont un certain format

Cette règle est utile lorsque vous souhaitez vous assurer que tous les titres de test correspondent à un format commun pour améliorer la lisibilité lorsque les tests échouent.

Par exemple, des titres comme `'Should throw when invalid.'`, `'Should fail when called.'` ou `'Should pass when using any number.'` pourrait être appliqué avec le motif suivant `'^Should (pass|fail|throw) when [\\w ]+\\.$'` (Notez les échappés `\`).


## Échoue

```js
/* eslint ava/test-title: ["error", {format: "^Devrait"}] */
import test from 'ava';
test('Ne commence pas par `Devrait`', t => {
	t.pass();
});
```

```js
/* eslint ava/test-title: ["error", {format: "\\.$"}] */
import test from 'ava';
test('Ne se termine pas par un point', t => {
	t.pass();
});
```


## Passe

```js
/* eslint ava/test-title: ["error", {format: "^Devrait"}] */
import test from 'ava';
test('Devrait passer les tests', t => {
	t.pass();
});
test('Devrait se comporter comme prévu', t => {
	t.pass();
});
```

```js
/* eslint ava/test-title: ["error", {format: "\\.$"}] */
import test from 'ava';
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
