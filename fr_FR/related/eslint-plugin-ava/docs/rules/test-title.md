___
**Note du traducteur**

C'est la traduction du fichier [test-title.md](https://github.com/avajs/eslint-plugin-ava/blob/main/docs/rules/test-title.md). Voici un [lien](https://github.com/avajs/eslint-plugin-ava/compare/dee1802d39e22aec0915d5067062356f5abfbd84...main#diff-23d161c2b190dc8b21da4a6a68578bc5f9d7d1f9523039677e170e9c6747afc0) vers les différences avec le master de eslint-plugin-ava (Si en cliquant sur le lien, vous ne trouvez pas le fichier `test-title.md` parmi les fichiers modifiés, vous pouvez donc en déduire que la traduction est à jour).
___
# S'assurer que les tests ont un titre

Traductions : [English](https://github.com/avajs/eslint-plugin-ava/blob/main/docs/rules/test-title.md)

Les tests devraient avoir un titre. La version [v1.0.1](https://github.com/avajs/ava/releases/tag/v1.0.1) de AVA et supérieure l'applique à l'exécution.

## Échoue

```js
const test = require('ava');

test(t => {
	t.pass();
});
```

## Passe

```js
const test = require('ava');

test('foo', t => {
	t.pass();
});
```
