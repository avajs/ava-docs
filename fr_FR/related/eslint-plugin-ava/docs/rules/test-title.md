___
**Note du traducteur**

C'est la traduction du fichier [test-title.md](https://github.com/avajs/eslint-plugin-ava/blob/master/docs/rules/test-title.md). Voici un [lien](https://github.com/avajs/eslint-plugin-ava/compare/7542453058c30ebbc79c7bfeb689492fce226d8f...main#diff-ed02b0af8dd256929d27a1e08192e303) vers les différences avec le master de eslint-plugin-ava (Si en cliquant sur le lien, vous ne trouvez pas le fichier `test-title.md` parmi les fichiers modifiés, vous pouvez donc en déduire que la traduction est à jour).
___
# S'assurer que les tests ont un titre

Traductions : [English](https://github.com/avajs/eslint-plugin-ava/blob/master/docs/rules/test-title.md)

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
