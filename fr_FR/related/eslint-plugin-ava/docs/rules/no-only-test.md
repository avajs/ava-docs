___
**Note du traducteur**

C'est la traduction du fichier [no-only-test.md](https://github.com/sindresorhus/eslint-plugin-ava/blob/master/docs/rules/no-only-test.md). Voici un [lien](https://github.com/sindresorhus/eslint-plugin-ava/compare/0c58a71767ca67097321b91f0332f543fc719389...master#diff-b7318d07931729c026fb1f61e9734468) vers les différences avec le master de AVA (Si en cliquant sur le lien, vous ne trouvez pas le fichier `no-only-test.md` parmi les fichiers modifiés, vous pouvez donc en déduire que la traduction est à jour).
___
# S'assurer qu'aucun `test.only()` soit présent

Il est facile d'exécuter un unique test avec `test.only()` et puis de l'oublier. Il est visible dans les résultats, mais on peut encore facilement le manquer. En oubliant d'enlever `.only`, cela signifie que seul ce test dans le fichier complet s'exécutera, et si on ne fait rien, ceci peut permettre de glisser de sérieux bugs dans votre code.


## Échoue

```js
import test from 'ava';

test.only('test 1', t => {
	t.pass();
});

// test 2 ne s'exécutera pas
test('test 2', t => {
	t.pass();
});
```


## Passe

```js
import test from 'ava';

test('test 1', t => {
	t.pass();
});

// test 2 s'exécutera
test('test 2', t => {
	t.pass();
});
```
