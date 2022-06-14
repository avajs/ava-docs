___
**Note du traducteur**

C'est la traduction du fichier [use-t-throws-async-well.md](https://github.com/avajs/eslint-plugin-ava/blob/main/docs/rules/use-t-throws-async-well.md). Voici un [lien](https://github.com/avajs/eslint-plugin-ava/compare/eea71c3eddb5608b04e07fcb48da8acdafff1525...main#diff-b5be2c5b59a3767d881be8cd0e969c9b43b8be07fd3202366cd302ae9d6369e0) vers les différences avec le master de eslint-plugin-ava (Si en cliquant sur le lien, vous ne trouvez pas le fichier `use-t-throws-async-well.md` parmi les fichiers modifiés, vous pouvez donc en déduire que la traduction est à jour).
___
# S'assurer que `t.throwsAsync()` et `t.notThrowsAsync()` sont attendus

Lorsque vous utilisez les assertions `t.throwsAsync()` et `t.notThrowsAsync()`, vous devez attendre la promesse qu'elles renvoient. Si la fonction de test se termine avant que les assertions soient faites, le test échouera.

Cette règle est corrigible à l'intérieur des fonctions `async`. Cela inséra `await` avant `t.throwsasync()` et `t.notthrorsasync()`.

## Échoue

```js
import test from 'ava';

test('main', t => {
	t.throwsAsync(somePromise);
	t.notThrowsAsync(somePromise);
});
```

## Passe

```js
import test from 'ava';

test('main', t => {
	await t.throwsAsync(somePromise);
	await t.notThrowsAsync(somePromise);
	const p = t.throwsAsync(somePromise);
	t.throwsAsync(somePromise).then(…);
});
```
