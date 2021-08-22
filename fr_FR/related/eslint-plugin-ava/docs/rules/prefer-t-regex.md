___
**Note du traducteur**

C'est la traduction du fichier [prefer-t-regex.md](https://github.com/avajs/eslint-plugin-ava/blob/master/docs/rules/prefer-t-regex.md). Voici un [lien](https://github.com/avajs/eslint-plugin-ava/compare/7542453058c30ebbc79c7bfeb689492fce226d8f...main#diff-9372c2f5b0ed65e5b60411abbca8bb71) vers les différences avec le master de eslint-plugin-ava (Si en cliquant sur le lien, vous ne trouvez pas le fichier `prefer-t-regex.md` parmi les fichiers modifiés, vous pouvez donc en déduire que la traduction est à jour).
___
# Préférer l'utilisation de `t.regex()` au lieu de tests d'expressions régulières

Traductions : [English](https://github.com/avajs/eslint-plugin-ava/blob/master/docs/rules/prefer-t-regex.md)

L'[assertion `t.regex()`](https://github.com/avajs/ava-docs/blob/main/fr_FR/docs/03-assertions.md#regexcontents-regex-message) de AVA peut tester une chaîne par rapport à une expression régulière.

Cette règle imposera l'utilisation de `t.regex()` au lieu d'utiliser manuellement `RegExp#test()`, ce qui rendra votre code plus clair et produira un meilleure affichage lors d'un échec.

Cette règle est réparable. Cela remplacera l'utilisation de `RegExp#test()`, `String#match()`, ou `String#search()` avec `t.regex()`.


## Échoue

```js
const test = require('ava');

test('main', t => {
	t.true(/\w+/.test('foo'));
});
```

```js
const test = require('ava');

test('main', t => {
	t.truthy('foo'.match(/\w+/));
});
```


## Passe

```js
const test = require('ava');

test('main', async t => {
	t.regex('foo', /\w+/);
});
```
