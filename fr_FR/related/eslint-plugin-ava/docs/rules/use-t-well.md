___
**Note du traducteur**

C'est la traduction du fichier [use-t-well.md](https://github.com/avajs/eslint-plugin-ava/blob/main/docs/rules/use-t-well.md). Voici un [lien](https://github.com/avajs/eslint-plugin-ava/compare/dee1802d39e22aec0915d5067062356f5abfbd84...main#diff-eef782cbcc5b495f41ea81eab8af21e986f56bafb1fe515f041fe9e22fe7cf57) vers les différences avec le master de eslint-plugin-ava (Si en cliquant sur le lien, vous ne trouvez pas le fichier `use-t-well.md` parmi les fichiers modifiés, vous pouvez donc en déduire que la traduction est à jour).
___
# Empêcher une mauvaise utilisation de `t`

Traductions : [English](https://github.com/avajs/eslint-plugin-ava/blob/main/docs/rules/use-t-well.md)

Empêche l'utilisation de méthodes d'assertion inconnues et empêche d'accéder à des méthodes différentes des assertions et de `.context`, ainsi que certaines utilisations incorrectes connues de `t`.

Cette règle est réparable de façon partielle. Il peut corriger la plupart des noms de méthodes d'assertion mal orthographiés et des usages incorrects de `.skip`.

## Échoue

```js
const test = require('ava');

test('main', t => {
	t(value); // `t` n'est pas une fonction
	t.depEqual(value, [2]); // Mal orthographié : `.deepEqual` en `.depEqual`, réparable
	t.contxt.foo = 100; // Mal orthographié : `.context` en `.contxt`, réparable
	t.deepEqual.skip.skip(); // Trop d'utilisations enchaînées de `.skip`, réparable
	t.skip.deepEqual(1, 1); //Le modificateur `.skip` doit être le dernier de la chaîne, réparable
	t.foo = 1000; // Membre `.foo` inconnu. Utilisez à la place `.context.foo`
	t.deepEqual.is(value, value); // Ne peut pas chainer des méthodes d'assertion
	t.skip(); // Manque la méthode d'assertion
});
```

## Passe

```js
const test = require('ava');

test('main', t => {
	t.deepEqual(value, [2]);
	t.context.a = 100;
	require(`fixtures/${t.title}`);
	t.deepEqual.skip();
});
```
