___
**Note du traducteur**

C'est la traduction du fichier [prefer-async-await.md](https://github.com/avajs/eslint-plugin-ava/blob/main/docs/rules/prefer-async-await.md). Voici un [lien](https://github.com/avajs/eslint-plugin-ava/compare/dee1802d39e22aec0915d5067062356f5abfbd84...main#diff-570eb0e2454a90668c01e2de527292f162168b302a9cbf2e769b2bdbbcf90499) vers les différences avec le master de eslint-plugin-ava (Si en cliquant sur le lien, vous ne trouvez pas le fichier `prefer-async-await.md` parmi les fichiers modifiés, vous pouvez donc en déduire que la traduction est à jour).
___
# Préférer l'utilisation de async/await au lieu de retourner une Promesse

Traductions : [English](https://github.com/avajs/eslint-plugin-ava/blob/main/docs/rules/prefer-async-await.md)

AVA est livré avec une prise en charge des fonctions async (async/await). Cela vous permet d'écrire des tests courts et clairs.

Cette règle signale une erreur quand il trouve un test qui retourne une expression qui ressemble à une promesse (contenant un appel `.then()`), qui pourrait être simplifiée en utilisant la syntaxe async/await.

## Échoue

```js
const test = require('ava');

test('foo', t => {
	return foo().then(res => {
		t.is(res, 1);
	});
});
```

## Passe

```js
const test = require('ava');

test('foo', async t => {
	t.is(await foo(), 1);
});
```
