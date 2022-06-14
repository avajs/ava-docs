___
**Note du traducteur**

C'est la traduction du fichier [use-t.md](https://github.com/avajs/eslint-plugin-ava/blob/main/docs/rules/use-t.md). Voici un [lien](https://github.com/avajs/eslint-plugin-ava/compare/c1cd7da74ac55abee856637c8bb75b385a444d1a...main#diff-19886e7d1ec7c93e9c2c06314561e8cdecc24ef445c2c7cca8b6a84b9798d73f) vers les différences avec le master de eslint-plugin-ava (Si en cliquant sur le lien, vous ne trouvez pas le fichier `use-t.md` parmi les fichiers modifiés, vous pouvez donc en déduire que la traduction est à jour).
___
# S'assurer que les fonctions de test utilisent `t` comme paramètre

Traductions : [English](https://github.com/avajs/eslint-plugin-ava/blob/main/docs/rules/use-t.md)

La convention veut que le paramètre dans la fonction de test de AVA soit nommé `t`. La plupart des règles dans `eslint-plugin-ava` sont basées sur cette hypothèse.

### Échoue

```js
const test = require('ava');

test('foo', foo => { // Nom incorrect
	t.pass();
});
```

### Passe

```js
const test = require('ava');

test('foo', () => {
	// ...
});

test('bar', t => {
	t.pass();
});
```
