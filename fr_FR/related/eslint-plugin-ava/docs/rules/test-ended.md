___
**Note du traducteur**

C'est la traduction du fichier [test-ended.md](https://github.com/avajs/eslint-plugin-ava/blob/master/docs/rules/test-ended.md). Voici un [lien](https://github.com/avajs/eslint-plugin-ava/compare/c3d99fb076f5e579ba00f18fbedb92aeaf9df732...master#diff-1a18b5ae66bcbafffe3be0ff7f18fa26) vers les différences avec le master de eslint-plugin-ava (Si en cliquant sur le lien, vous ne trouvez pas le fichier `test-ended.md` parmi les fichiers modifiés, vous pouvez donc en déduire que la traduction est à jour).
___
# S'assurer que les tests de callback soient explicitement terminés

Traductions : [English](https://github.com/avajs/eslint-plugin-ava/blob/master/docs/rules/test-ended.md)

Si vous oubliez un `t.end();` dans un `test.cb()`, le test sera suspendu indéfiniment.


## Échoue

```js
import test from 'ava';

test.cb(t => {
	t.pass();
});
```


## Passe

```js
import test from 'ava';

test.cb(t => {
	t.pass();
	t.end();
});

test.cb(t => {
	acceptsCallback(t.end);
});
```
