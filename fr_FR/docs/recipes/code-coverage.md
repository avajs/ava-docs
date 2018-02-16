___
**Note du traducteur**

C'est la traduction du fichier [code-coverage.md](https://github.com/avajs/ava/blob/master/docs/recipes/code-coverage.md). Voici un [lien](https://github.com/avajs/ava/compare/af2c2b67a16bc4884797cd8267448fb48e24ba1e...master#diff-b3aa0c81a407f54f636a1cf5a619a4a6) vers les différences avec le master de AVA (Si en cliquant sur le lien, vous ne trouvez pas le fichier `code-coverage.md` parmi les fichiers modifiés, vous pouvez donc en déduire que la traduction est à jour).
___
# Couverture de code

Traductions : [English](https://github.com/avajs/ava/blob/master/docs/recipes/code-coverage.md), [Español](https://github.com/avajs/ava-docs/blob/master/es_ES/docs/recipes/code-coverage.md), [Italiano](https://github.com/avajs/ava-docs/blob/master/it_IT/docs/recipes/code-coverage.md),  [日本語](https://github.com/avajs/ava-docs/blob/master/ja_JP/docs/recipes/code-coverage.md), [Português](https://github.com/avajs/ava-docs/blob/master/pt_BR/docs/recipes/code-coverage.md), [Русский](https://github.com/avajs/ava-docs/blob/master/ru_RU/docs/recipes/code-coverage.md), [简体中文](https://github.com/avajs/ava-docs/blob/master/zh_CN/docs/recipes/code-coverage.md)

Utilisez la ligne de commande client [nyc] pour [Istanbul] pour calculer la couverture de code de vos tests.

Tout d'abord, installez [nyc] :

```
$ npm install --save-dev nyc
```

Le plus simple, exécutez AVA à travers [nyc]. Dans notre fichier `package.json` :

```json
{
	"scripts": {
		"test": "nyc ava"
	}
}
```

Vous souhaitez exclure les répertoires `.nyc_output` et `coverage` du contrôle des sources. En supposant que vous utilisez Git, ajoutez ce qui suit à votre fichier `.gitignore` :

```
.nyc_output
coverage
```

Si vous compilez vos fichiers source en utilisant Babel, vous pouvez appliquer l'instrumentation d'Istanbul dans le cadre de la compilation du fichier source. Cela donnera de meilleurs résultats que l'instrumentation de la sortie de Babel. Consultez le [tutoriel *Utilisation d'Istanbul avec ES2015+*](https://istanbul.js.org/docs/tutorials/es2015/) d'Istanbul. AVA définit `NODE_ENV=test` pour vous. Notez que depuis février 2018, ce tutoriel n'a pas encore été mis à jour pour Babel 7.

[Istanbul]: https://istanbul.js.org/
[nyc]: https://github.com/istanbuljs/nyc
