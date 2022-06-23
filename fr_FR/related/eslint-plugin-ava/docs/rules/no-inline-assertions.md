___
**Note du traducteur**

C'est la traduction du fichier [no-inline-assertions.md](https://github.com/avajs/eslint-plugin-ava/blob/main/docs/rules/no-inline-assertions.md). Voici un [lien](https://github.com/avajs/eslint-plugin-ava/compare/dee1802d39e22aec0915d5067062356f5abfbd84...main#diff-80ed70193196d1b08810a13abb23bc14b7e07bd648081258c965192d2fbe4443) vers les différences avec le master de eslint-plugin-ava (Si en cliquant sur le lien, vous ne trouvez pas le fichier `no-inline-assertions` parmi les fichiers modifiés, vous pouvez donc en déduire que la traduction est à jour).
___
# S'assurer que les assertions ne sont pas appelées depuis des fonctions fléchées sur une ligne

Traductions : [English](https://github.com/avajs/eslint-plugin-ava/blob/main/docs/rules/no-inline-assertions.md)

L'implémentation du test ne doit pas consister uniquement en une assertion sur une ligne, car les assertions ne renvoient pas de valeur et leur affichage sur une ligne rend également les tests moins lisibles.

Cette règle est réparable. Il encapsulera l'assertion entre accolades `{}`. Il ne fera aucun espace ou changement de style.

## Échoue

```js
const test = require('ava');
test('foo', t => t.true(fn()));
```

## Passe

```js
const test = require('ava');
test('foo', t => {
	t.true(fn());
});
```
