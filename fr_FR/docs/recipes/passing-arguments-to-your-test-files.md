___
**Note du traducteur**

C'est la traduction du fichier [passing-arguments-to-your-test-files.md](https://github.com/avajs/ava/blob/main/docs/recipes/passing-arguments-to-your-test-files.md). Voici un [lien](https://github.com/avajs/ava/compare/79b2ea30c125f44e4d47bdafdeec351cddb5911a...master#diff-6b09519f1a3599f39e02aec0e613151c) vers les différences avec le master de AVA (Si en cliquant sur le lien, vous ne trouvez pas le fichier `passing-arguments-to-your-test-files.md` parmi les fichiers modifiés, vous pouvez donc en déduire que la traduction est à jour).
___
# Passer des arguments à vos fichiers de test

Traductions : [English](https://github.com/avajs/ava/raw/main/docs/recipes/passing-arguments-to-your-test-files.md)

Vous pouvez passer des arguments à ligne de commande pour vos fichiers de test. Utilisez l'argument `--` pour les séparer des arguments de AVA :

```js
// test.js
const test = require('ava');

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
