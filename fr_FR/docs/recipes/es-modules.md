___
**Note du traducteur**

C'est la traduction du fichier [es-modules.md](https://github.com/avajs/ava/blob/master/docs/recipes/es-modules.md). Voici un [lien](https://github.com/avajs/ava/compare/16f474242880ea325fea792dbd11ae43bbe17c06...master#diff-1da5d6bfe1c97b416498e18caafe90fc) vers les différences avec le master de AVA (Si en cliquant sur le lien, vous ne trouvez pas le fichier `es-modules.md` parmi les fichiers modifiés, vous pouvez donc en déduire que la traduction est à jour).
___
# Utilisation des ES modules dans AVA

Traductions : [English](https://github.com/avajs/ava/blob/master/docs/recipes/es-modules.md)

À partir de Node.js 8.5.0, les [ES modules](http://2ality.com/2017/09/native-esm-node.html) sont nativement pris en charge, mais en utilisant le flag de la ligne de commande `--experimental-modules`. Ça marche en utilisant l'extension de fichier `.mjs`. AVA ne prend actuellement pas en charge l'option de ligne de commande ou la nouvelle extension de fichier, mais vous *pouvez* utiliser le module [`esm`](https://github.com/standard-things/esm) pour utiliser la nouvelle syntaxe.

Voici comment vous devez travailler avec AVA.

D'abord, installez [`esm`](https://github.com/standard-things/esm):

```
$ npm install esm
```

Configurez-le dans votre fichier `package.json` ou `ava.config.js` et ajoutez-le à l'option `"require"` de AVA. Assurez-vous de l'ajouter en tant que premier élément :

```json
{
	"ava": {
		"require": [
			"esm"
		]
	}
}
```

Par défaut, AVA convertit la syntaxe du ES module en CommonJS. [Vous pouvez le désactiver](./babel.md#préserver-la-syntaxe-du-module-es).

Vous pouvez maintenant utiliser les modules ES natifs avec AVA :

```js
// sum.mjs
export default function sum(a, b) {
	return a + b;
};
```

```js
// test.js
import test from 'ava';
import sum from './sum.mjs';

test('2 + 2 = 4', t => {
	t.is(sum(2, 2), 4);
});
```

Vous devez configurer AVA pour reconnaître les extensions `.mjs`. Si vous voulez que AVA applique ses presets Babel, utilisez :

```json
{
	"ava": {
		"babel": {
			"extensions": [
				"js",
				"mjs"
			]
		}
	}
}
```

Alternativement, vous pouvez utiliser :

```json
{
	"ava": {
		"babel": false,
		"extensions": [
			"js",
			"mjs"
		]
	}
}
```

Ou laissez Babel activé (ce qui signifie qu'il est appliqué aux fichiers `.js`), mais ne l'appliquez pas aux fichiers `.mjs` :

```json
{
	"ava": {
		"extensions": [
			"mjs"
		]
	}
}
```
