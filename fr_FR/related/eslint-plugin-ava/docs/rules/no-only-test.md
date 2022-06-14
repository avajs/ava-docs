___
**Note du traducteur**

C'est la traduction du fichier [no-only-test.md](https://github.com/avajs/eslint-plugin-ava/blob/main/docs/rules/no-only-test.md). Voici un [lien](https://github.com/avajs/eslint-plugin-ava/compare/dee1802d39e22aec0915d5067062356f5abfbd84...main#diff-98bea2b47c96d645e4cddc93895fb0a4d7a4ecedd9513bd8def7d4d0305df9ae) vers les différences avec le master de eslint-plugin-ava (Si en cliquant sur le lien, vous ne trouvez pas le fichier `no-only-test.md` parmi les fichiers modifiés, vous pouvez donc en déduire que la traduction est à jour).
___
# S'assurer qu'aucun `test.only()` soit présent

Traductions : [English](https://github.com/avajs/eslint-plugin-ava/blob/main/docs/rules/no-only-test.md)

Il est facile d'exécuter un unique test avec `test.only()` et puis de l'oublier. Il est visible dans les résultats, mais on peut encore facilement le manquer. En oubliant d'enlever `.only`, cela signifie que seul ce test dans le fichier complet s'exécutera, et si on ne fait rien, ceci peut permettre de glisser de sérieux bugs dans votre code.

## Échoue

```js
const test = require('ava');

test.only('test 1', t => {
	t.pass();
});

// test 2 ne s'exécutera pas
test('test 2', t => {
	t.pass();
});
```

## Passe

```js
const test = require('ava');

test('test 1', t => {
	t.pass();
});

// test 2 s'exécutera
test('test 2', t => {
	t.pass();
});
```
