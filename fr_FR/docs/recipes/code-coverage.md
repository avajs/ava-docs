___
**Note du traducteur**

C'est la traduction du fichier [code-coverage.md](https://github.com/avajs/ava/blob/master/docs/recipes/code-coverage.md). Voici un [lien](https://github.com/avajs/ava/compare/b4ea43529a6d058a96055735cfa6e7056c009112...master#diff-b3aa0c81a407f54f636a1cf5a619a4a6) vers les différences avec le master de AVA (Si en cliquant sur le lien, vous ne trouvez pas le fichier `code-coverage.md` parmi les fichiers modifiés, vous pouvez donc en déduire que la traduction est à jour).
___
# Couverture de code

Traductions : [English](https://github.com/avajs/ava/blob/master/docs/recipes/code-coverage.md), [Español](https://github.com/avajs/ava-docs/blob/master/es_ES/docs/recipes/code-coverage.md), [Italiano](https://github.com/avajs/ava-docs/blob/master/it_IT/docs/recipes/code-coverage.md),  [日本語](https://github.com/avajs/ava-docs/blob/master/ja_JP/docs/recipes/code-coverage.md), [Português](https://github.com/avajs/ava-docs/blob/master/pt_BR/docs/recipes/code-coverage.md), [Русский](https://github.com/avajs/ava-docs/blob/master/ru_RU/docs/recipes/code-coverage.md), [简体中文](https://github.com/avajs/ava-docs/blob/master/zh_CN/docs/recipes/code-coverage.md)

Utilisez [`nyc`] pour calculer la couverture de code de vos tests.

Tout d'abord, installez [`nyc`] :

```
$ npm install --save-dev nyc
```

Le plus simple, exécutez AVA à travers [`nyc`]. Dans notre fichier `package.json` :

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

[`nyc`]: https://github.com/istanbuljs/nyc
