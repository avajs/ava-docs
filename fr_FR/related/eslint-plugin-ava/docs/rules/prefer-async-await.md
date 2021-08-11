___
**Note du traducteur**

C'est la traduction du fichier [prefer-async-await.md](https://github.com/avajs/eslint-plugin-ava/blob/master/docs/rules/prefer-async-await.md). Voici un [lien](https://github.com/avajs/eslint-plugin-ava/compare/7542453058c30ebbc79c7bfeb689492fce226d8f...main#diff-7d767b0886e0705701069153a1c4015b) vers les différences avec le master de eslint-plugin-ava (Si en cliquant sur le lien, vous ne trouvez pas le fichier `prefer-async-await.md` parmi les fichiers modifiés, vous pouvez donc en déduire que la traduction est à jour).
___
# Préférer l'utilisation de async/await au lieu de retourner une Promesse

Traductions : [English](https://github.com/avajs/eslint-plugin-ava/blob/master/docs/rules/prefer-async-await.md)

AVA est livré avec une prise en charge des [fonctions async](http://www.2ality.com/2016/02/async-functions.html) (async/await). Cela vous permet d'écrire des tests courts et clairs.

Cette règle signale une erreur quand il trouve un test qui retourne une expression qui ressemble à une promesse (contenant un appel `.then()`), qui pourrait être simplifiée en utilisant la syntaxe async/await.


## Échoue

```js
const test = require('ava');

test(t => {
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
