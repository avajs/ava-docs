___
**Note du traducteur**

C'est la traduction du fichier [code-coverage.md](https://github.com/avajs/ava/blob/main/docs/recipes/code-coverage.md). Voici un [lien](https://github.com/avajs/ava/compare/b208d143ad852dc95aa8b44eed94ac1f404a25f4...main#diff-6cb4ccf1c34425b96a5b584514e057f0bfbd27e99656fc4ad49b0d4f5c63ac17) vers les différences avec le master de AVA (Si en cliquant sur le lien, vous ne trouvez pas le fichier `code-coverage.md` parmi les fichiers modifiés, vous pouvez donc en déduire que la traduction est à jour).
___
# Couverture de code

Traductions : [English](https://github.com/avajs/ava/raw/main/docs/recipes/code-coverage.md), [Español](https://github.com/avajs/ava-docs/blob/main/es_ES/docs/recipes/code-coverage.md), [Italiano](https://github.com/avajs/ava-docs/blob/main/it_IT/docs/recipes/code-coverage.md),  [日本語](https://github.com/avajs/ava-docs/blob/main/ja_JP/docs/recipes/code-coverage.md), [Português](https://github.com/avajs/ava-docs/blob/main/pt_BR/docs/recipes/code-coverage.md), [Русский](https://github.com/avajs/ava-docs/blob/main/ru_RU/docs/recipes/code-coverage.md), [简体中文](https://github.com/avajs/ava-docs/blob/main/zh_CN/docs/recipes/code-coverage.md)

Utilisez [`8`] pour calculer la couverture de code de vos tests.

Tout d'abord, installez [`c8`] :

```
$ npm install --save-dev c8
```

Le plus simple, exécutez AVA à travers [`c8`]. Dans notre fichier `package.json` :

```json
{
	"scripts": {
		"test": "c8 ava"
	}
}
```

Vous souhaitez exclure le répertoire `coverage` du contrôle des sources. En supposant que vous utilisez Git, ajoutez ce qui suit à votre fichier `.gitignore` :

```
coverage
```

[`c8`]: https://github.com/bcoe/c8
