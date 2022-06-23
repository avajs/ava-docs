___
**Note du traducteur**

C'est la traduction du fichier [max-asserts.md](https://github.com/avajs/eslint-plugin-ava/blob/main/docs/rules/max-asserts.md). Voici un [lien](https://github.com/avajs/eslint-plugin-ava/compare/dee1802d39e22aec0915d5067062356f5abfbd84...main#diff-01a069e556e28c486d139a3458114fd7334c2a056f50481339a458577a0d2aa9) vers les différences avec le master de eslint-plugin-ava (Si en cliquant sur le lien, vous ne trouvez pas le fichier `max-asserts.md` parmi les fichiers modifiés, vous pouvez donc en déduire que la traduction est à jour).
___
# Limiter le nombre d'assertions dans un test

Traductions : [English](https://github.com/avajs/eslint-plugin-ava/blob/main/docs/rules/max-asserts.md)

Limite la quantité d'assertions dans un test afin d'appliquer le découpage des gros tests en plusieurs plus petits.

Les assertions passées (skip) sont comptés.

## Échoue

```js
/*eslint ava/max-asserts: ["error", 5]*/
const test = require('ava');

test('getSomeObject devrait définir le nom du joueur', t => {
	const object = lib.getSomeObject();

	t.is(typeof object, 'object');
	t.is(typeof object.player, 'object');
	t.is(object.player.firstName, 'Luke');
	t.is(object.player.lastName, 'Skywalker');
	t.is(typeof object.opponent, 'object');
	t.is(object.opponent.firstName, 'Darth');
	t.is(object.opponent.lastName, 'Vader');
});
```

## Passe

```js
const test = require('ava');

test('getSomeObject devrait définir le nom du joueur', t => {
	const object = lib.getSomeObject();

	t.is(typeof object, 'object');
	t.is(typeof object.player, 'object');
	t.is(object.player.firstName, 'Luke');
	t.is(object.player.lastName, 'Skywalker');
});

test('getSomeObject devrait définir le nom du joueur', t => {
	const object = lib.getSomeObject();

	t.is(typeof object, 'object');
	t.is(typeof object.opponent, 'object');
	t.is(object.opponent.firstName, 'Darth');
	t.is(object.opponent.lastName, 'Vader');
});
```

## Options

La règle prend une option, un nombre, qui est le nombre maximum d'assertions pour chaque test. La valeur par défaut est 5.

Vous pouvez définir l'option dans la configuration comme ceci :

```js
"ava/max-asserts": ["error", 5]
```
