___
**Note du traducteur**

C'est la traduction du fichier [assertion-arguments.md](https://github.com/avajs/eslint-plugin-ava/blob/main/docs/rules/assertion-arguments.md). Voici un [lien](https://github.com/avajs/eslint-plugin-ava/compare/dee1802d39e22aec0915d5067062356f5abfbd84...main#diff-9595412d72d85e04b16a023e18302b498cb57d6b9141ac533a45664003b038bc) vers les différences avec le master de eslint-plugin-ava (Si en cliquant sur le lien, vous ne trouvez pas le fichier `assertion-arguments` parmi les fichiers modifiés, vous pouvez donc en déduire que la traduction est à jour).
___
# Imposer le passage d'arguments corrects aux assertions

Traductions : [English](https://github.com/avajs/eslint-plugin-ava/blob/main/docs/rules/assertion-arguments.md)

Impose le passage du bon nombre d'arguments aux méthodes d'assertion comme `t.is ()`. Cette règle peut aussi imposer ou interdire l'utilisation des messages d'assertion.

Les messages d'assertion sont des arguments factultatifs qui peuvent être fournis lors de l'appel d'assertion afin d'améliorer le message d'erreur si l'affirmation échoue.

Cette règle tente également d'imposer le passage des valeurs actuelles avant les valeurs attendues. Si exactement un des deux premiers arguments d'une assertion à deux arguments est une expression statique telle que `{a: 1}`, alors l'expression statique doit être positionné en second. (`t.regex()` et `t.notRegex()` sont exclus de cette vérification, car leur argument `contents` ou leur argument `regex` pourrait plausiblement être la valeur actuelle ou attendue). Si l'argument d'une assertion à un argument est une relation binaire telle que `'static' === dynamic`, une vérification similaire est effectuée sur ses côtés gauche et droit. Les erreurs de ce type sont généralement corrigibles.

## Échoue

```js
const test = require('ava');

test('1', t => {
	t.is(value); // Pas assez d'arguments
	t.is(value, expected, message, extra); // Trop d'arguments
	t.is(value, expected, false); // Le message d'assertion n'est pas une chaîne
});

/* eslint ava/assertion-arguments: ["error", {"message": "always"}] */
test('2', t => {
	t.true(array.includes(value));
});

/* eslint ava/assertion-arguments: ["error", {"message": "never"}] */
test('3', t => {
	t.true(array.includes(value), 'value is not in array');
});
```

## Passe

```js
const test = require('ava');

test('1', t => {
	t.is(value, expected);
	t.is(value, expected, message);
});

/* eslint ava/assertion-arguments: ["error", {"message": "always"}] */
test('2', t => {
	t.true(array.includes(value), 'value is not in array');
});

/* eslint ava/assertion-arguments: ["error", {"message": "never"}] */
test('3', t => {
	t.true(array.includes(value));
});
```

## Options

Cette règle prend en charge les options suivantes :

`message`: Un string qui peut être `"always"` ou `"never"`. S'il est défini à `"always"`, toutes les assertions devront avoir un message d'assertion message. S'il est défini à `"never"`, aucune assertion ne doit avoir un message d'assertion. Si ce n'est pas renseigné, aucun rapport sur le message d'assertion sera faite.

Vous pouvez définir l'option dans la configuration de cette manière :

```js
"ava/assertion-arguments": ["error", {"message": "always"}]
```
