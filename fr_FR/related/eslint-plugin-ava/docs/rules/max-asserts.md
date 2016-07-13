___
**Note du traducteur**

C'est la traduction du fichier [max-asserts.md](https://github.com/avajs/eslint-plugin-ava/blob/master/docs/rules/max-asserts.md). Voici un [lien](https://github.com/avajs/eslint-plugin-ava/compare/c3d99fb076f5e579ba00f18fbedb92aeaf9df732...master#diff-fca60e7ef485498c6b37c6a950f9d59a) vers les différences avec le master de eslint-plugin-ava (Si en cliquant sur le lien, vous ne trouvez pas le fichier `max-asserts.md` parmi les fichiers modifiés, vous pouvez donc en déduire que la traduction est à jour).
___
# Limiter le nombre d'assertions dans un test

Traductions : [English](https://github.com/avajs/eslint-plugin-ava/blob/master/docs/rules/max-asserts.md)

Limite la quantité d'assertions dans un test afin d'appliquer le découpage des gros tests en plusieurs plus petits.

Les assertions passées (skip) sont comptés.


## Échoue

```js
/*eslint ava/max-asserts: ["error", 5]*/
import test = from 'ava';

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
import test = from 'ava';

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
