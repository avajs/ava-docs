___
**Note du traducteur**

C'est la traduction du fichier [passing-arguments-to-your-test-files.md](https://github.com/avajs/ava/blob/main/docs/recipes/passing-arguments-to-your-test-files.md). Voici un [lien](https://github.com/avajs/ava/compare/b208d143ad852dc95aa8b44eed94ac1f404a25f4...main#diff-0fec390b042c2bb40f272fed2d1bd695f95995a60764bb7f3801f58babfe8cd0) vers les différences avec le master de AVA (Si en cliquant sur le lien, vous ne trouvez pas le fichier `passing-arguments-to-your-test-files.md` parmi les fichiers modifiés, vous pouvez donc en déduire que la traduction est à jour).
___
# Passer des arguments à vos fichiers de test

Traductions : [English](https://github.com/avajs/ava/blob/main/docs/recipes/passing-arguments-to-your-test-files.md)

Vous pouvez passer des arguments à ligne de commande pour vos fichiers de test. Utilisez l'argument `--` pour les séparer des arguments de AVA :

```js
// test.js
import test from 'ava';

test('argv', t => {
	t.deepEqual(process.argv.slice(2), ['--hello', 'world']);
});
```

```console
$ npx ava -- --hello world
```

Vous avez besoin de deux arguments `--` si vous invoquez AVA via un script `npm test` :

```json
{
	"scripts": {
		"test": "ava"
	}
}
```

```console
$ npm test -- -- --hello world
```
