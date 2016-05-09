___
**Note du traducteur**

C'est la traduction du fichier [assertion-arguments.md](https://github.com/sindresorhus/eslint-plugin-ava/blob/master/docs/rules/assertion-arguments.md). Voici un [lien](https://github.com/sindresorhus/eslint-plugin-ava/compare/29a89e1e32a5631ae5e35813f761358024b9a146...master#diff-02806b2ce1a1f920f52c6ca18a3481a1) vers les différences avec le master de eslint-plugin-ava (Si en cliquant sur le lien, vous ne trouvez pas le fichier `assertion-arguments` parmi les fichiers modifiés, vous pouvez donc en déduire que la traduction est à jour).
___
# Imposer le passage d'arguments corrects aux assertions

Impose le passage du bon nombre d'arguments aux méthodes d'assertion comme `t.is ()`. Cette règle peut aussi imposer ou interdire l'utilisation des messages d'assertion.

Les messages d'assertion sont des arguments factultatifs qui peuvent être fournis lors de l'appel d'assertion afin d'améliorer le message d'erreur si l'affirmation échoue.

## Échoue

```js
import test from 'ava';

test(t => {
	t.is(value); // Pas assez d'arguments
	t.is(value, expected, message, extra); // Trop d'arguments
});

/* eslint ava/assertion-arguments: ["error", {"message": "always"}] */
test(t => {
	t.true(array.indexOf(value) !== -1);
});

/* eslint ava/assertion-arguments: ["error", {"message": "never"}] */
test(t => {
	t.true(array.indexOf(value) !== -1, 'value is not in array');
});
```


## Passe

```js
import test from 'ava';

test(t => {
	t.is(value, expected);
	t.is(value, expected, message);
});

/* eslint ava/assertion-arguments: ["error", {"message": "always"}] */
test(t => {
	t.true(array.indexOf(value) !== -1, 'value is not in array');
});

/* eslint ava/assertion-arguments: ["error", {"message": "never"}] */
test(t => {
	t.true(array.indexOf(value) !== -1);
});
```


## Options

Cette règle prend en charge les options suivantes :

`message`: Un string qui peut être `"always"` ou `"never"`. S'il est défini à `"always"`, toutes les assertions devront avoir un message d'assertion message. S'il est défini à `"never"`, aucune assertion ne doit avoir un message d'assertion. Si ce n'est pas renseigné, aucun rapport sur le message d'assertion sera faite.

Vous pouvez définir l'option dans la configuration de cette manière :

```js
"ava/assertion-arguments": ["error", {"message": "always"}]
```
