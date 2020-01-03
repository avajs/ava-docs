___
**Note du traducteur**

C'est la traduction du fichier [no-async-fn-without-await.md](https://github.com/avajs/eslint-plugin-ava/blob/master/docs/rules/no-async-fn-without-await.md). Voici un [lien](https://github.com/avajs/eslint-plugin-ava/compare/7542453058c30ebbc79c7bfeb689492fce226d8f...master#diff-ba145accc88fcda06145597bb3b1d565) vers les différences avec le master de eslint-plugin-ava (Si en cliquant sur le lien, vous ne trouvez pas le fichier `no-async-fn-without-await.md` parmi les fichiers modifiés, vous pouvez donc en déduire que la traduction est à jour).
___
# S'assurer que les tests async utilisent `await`

Traductions : [English](https://github.com/avajs/eslint-plugin-ava/blob/master/docs/rules/no-async-fn-without-await.md)

AVA est livré avec une prise en charge intégré des [fonctions async](http://www.2ality.com/2016/02/async-functions.html) (async/await). Cela permet d'écrire des tests plus courts et plus clairs.

Déclarer un test async sans utiliser le mot-clé `await` signifie soit qu'une promesse n'est pas attendue comme prévu ou que la fonction aurait pu être déclarée comme une fonction régulière, ce qui est source de confusion et c'est plus lent.

Cette règle signale une erreur quand il trouve un test async qui n'utilise pas le mot-clé `await`.


## Échoue

```js
const test = require('ava');

test(async t => {
	return foo().then(res => {
		t.is(res, 1);
	});
});
```


## Passe

```js
const test = require('ava');

test(async t => {
	t.is(await foo(), 1);
});
```
