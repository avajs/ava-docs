___
**Note du traducteur**

C'est la traduction du fichier [babel.md](https://github.com/avajs/ava/blob/main/docs/recipes/babel.md). Voici un [lien](https://github.com/avajs/ava/compare/b208d143ad852dc95aa8b44eed94ac1f404a25f4...main#diff-012bb93393257e9f0c08e4808d0925fcb3c7ebbf9178186ce1493afee8f2af69) vers les différences avec le master de AVA (Si en cliquant sur le lien, vous ne trouvez pas le fichier `babel.md` parmi les fichiers modifiés, vous pouvez donc en déduire que la traduction est à jour).
___
# Configuration de Babel avec AVA

Traductions : [English](https://github.com/avajs/ava/raw/main/docs/recipes/babel.md)

Vous pouvez activer la prise en charge de Babel en installant [`@babel/register`](https://babeljs.io/docs/en/babel-register) et `@babel/core`, et ensuite dans la configuration de AVA, en exigeant `@babel/register` :

**`package.json` :**

```json
{
	"ava": {
		"require": [
			"@babel/register"
		]
	}
}
```

`@babel/register` est compatible uniquement avec CommonJS. Il intercepte l'appel `require()` et compile les fichiers à la volée. Cela compilera les fichiers source, les helpers et les tests.

Pour plus d'informations, visitez la [documentation Babel](https://babeljs.io/docs/en/babel-register).
