___
**Note du traducteur**

C'est la traduction du fichier [no-inline-assertions.md](https://github.com/avajs/eslint-plugin-ava/blob/master/docs/rules/no-inline-assertions.md). Voici un [lien](https://github.com/avajs/eslint-plugin-ava/compare/b829dbcc38859f302283103b3c9a4684d94c0eec...master#diff-34a8aadc3509fbb43643865e0ae78b86) vers les différences avec le master de eslint-plugin-ava (Si en cliquant sur le lien, vous ne trouvez pas le fichier `no-inline-assertions` parmi les fichiers modifiés, vous pouvez donc en déduire que la traduction est à jour).
___
# S'assurer que les assertions ne sont pas appelées depuis des fonctions fléchées sur une ligne

L'implémentation du test ne doit pas consister uniquement en une assertion sur une ligne, car les assertions ne renvoient pas de valeur et leur affichage sur une ligne rend également les tests moins lisibles.

Cette règle est réparable. Il encapsulera l'assertion entre accolades `{}`. Il ne fera aucun espace ou changement de style.


## Échoue

```js
import test from 'ava';
test('foo', t => t.true(fn()));
```


## Passe

```js
import test from 'ava';
test('foo', t => {
	t.true(fn());
});
```
