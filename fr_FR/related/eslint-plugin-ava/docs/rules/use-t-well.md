___
**Note du traducteur**

C'est la traduction du fichier [use-t-well.md](https://github.com/avajs/eslint-plugin-ava/blob/master/docs/rules/use-t-well.md). Voici un [lien](https://github.com/avajs/eslint-plugin-ava/compare/0ded4b5c3cd09504e846309760566c9499a24196...master#diff-732d970806d109613f5519b63e9056d8) vers les différences avec le master de eslint-plugin-ava (Si en cliquant sur le lien, vous ne trouvez pas le fichier `use-t-well.md` parmi les fichiers modifiés, vous pouvez donc en déduire que la traduction est à jour).
___
# Empêcher une mauvaise utilisation de `t`

Traductions : [English](https://github.com/avajs/eslint-plugin-ava/blob/master/docs/rules/use-t-well.md)

Empêche l'utilisation de méthodes d'assertion inconnues et empêche d'accèder à des méthodes différentes des assertions et de `context`, ainsi que certaines utilisations incorrectes connues de `t`.


## Échoue

```js
import test from 'ava';

test('main', t => {
	t(value); // `t` n'est pas une fonction
	t.depEqual(value, [2]); // Méthode d'assertion `depEqual` inconnue
	t.contxt.foo = 100; // Membre `contxt` inconnu. Utilisez à la place `context.contxt`
	t.foo = 1000; // Membre `foo` inconnu. Utilisez à la place `context.foo`
	t.deepEqual.is(value, value); // Ne peut pas chainer des méthodes d'assertion
	t.skip(); // Manque la méthode d'assertion
	t.deepEqual.skip.skip(); // Trop nombreuses utilisations enchaînées de `skip`
});
```


## Passe

```js
import test from 'ava';

test('main', t => {
	t.deepEqual(value, [2]);
	t.context.a = 100;
	require(`fixtures/${t.title}`);
	t.deepEqual.skip();
});
```
