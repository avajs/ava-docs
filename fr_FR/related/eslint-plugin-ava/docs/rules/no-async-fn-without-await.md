___
**Note du traducteur**

C'est la traduction du fichier [no-async-fn-without-await.md](https://github.com/avajs/eslint-plugin-ava/blob/main/docs/rules/no-async-fn-without-await.md). Voici un [lien](https://github.com/avajs/eslint-plugin-ava/compare/dee1802d39e22aec0915d5067062356f5abfbd84...main#diff-53a3ae911fe8bd0587b8ff5153577004dea7644b96d6a0cf894ca1a7f6ca8771) vers les différences avec le master de eslint-plugin-ava (Si en cliquant sur le lien, vous ne trouvez pas le fichier `no-async-fn-without-await.md` parmi les fichiers modifiés, vous pouvez donc en déduire que la traduction est à jour).
___
# S'assurer que les tests async utilisent `await`

Traductions : [English](https://github.com/avajs/eslint-plugin-ava/blob/main/docs/rules/no-async-fn-without-await.md)

AVA est livré avec une prise en charge intégré des [fonctions async](http://www.2ality.com/2016/02/async-functions.html) (async/await). Cela permet d'écrire des tests plus courts et plus clairs.

Déclarer un test async sans utiliser le mot-clé `await` signifie soit qu'une promesse n'est pas attendue comme prévu ou que la fonction aurait pu être déclarée comme une fonction régulière, ce qui est source de confusion et c'est plus lent.

Cette règle signale une erreur quand il trouve un test async qui n'utilise pas le mot-clé `await`.

## Échoue

```js
const test = require('ava');

test('foo', async t => {
	return foo().then(res => {
		t.is(res, 1);
	});
});
```

## Passe

```js
const test = require('ava');

test('foo', async t => {
	t.is(await foo(), 1);
});
```
