___
**Note du traducteur**

C'est la traduction du fichier [no-cb-test.md](https://github.com/sindresorhus/eslint-plugin-ava/blob/master/docs/rules/no-cb-test.md). Voici un [lien](https://github.com/sindresorhus/eslint-plugin-ava/compare/216cd688cded0f2b79f3f652dc2eb43353f08fc4...master#diff-53d512160f47892d199cb93040332ff7) vers les différences avec le master de eslint-plugin-ava (Si en cliquant sur le lien, vous ne trouvez pas le fichier `no-cb-test.md` parmi les fichiers modifiés, vous pouvez donc en déduire que la traduction est à jour).
___
# S'assurer qu'aucun `test.cb()` n'est utilisé

Traductions : [English](https://github.com/sindresorhus/eslint-plugin-ava/blob/master/docs/rules/no-cb-test.md)

Interdit l'utilisation de `test.cb()`. Nous recommandons plutôt l'utilisation de `test()` avec une fonction async ou une fonction qui retourne une promesse.


## Échoue

```js
import test from 'ava';

test.cb('quelques tests', t => {
	t.pass();
	t.end();
});
```


## Passe

```js
import test from 'ava';

test('quelques tests', async t => {
	t.pass();
});
```
