___
**Note du traducteur**

C'est la traduction du fichier [es-modules.md](https://github.com/avajs/ava/blob/master/docs/recipes/es-modules.md). Voici un [lien](https://github.com/avajs/ava/compare/8fa28254dbebef32cbde05c0c9a49061d0ef82f8...master#diff-1da5d6bfe1c97b416498e18caafe90fc) vers les différences avec le master de AVA (Si en cliquant sur le lien, vous ne trouvez pas le fichier `es-modules.md` parmi les fichiers modifiés, vous pouvez donc en déduire que la traduction est à jour).
___
# Utilisation des ES modules dans AVA

Traductions : [English](https://github.com/avajs/ava/blob/master/docs/recipes/es-modules.md)

À partir de Node.js 13, les [ECMAScript modules](https://nodejs.org/docs/latest/api/esm.html#esm_introduction) sont nativement pris en charge dans Node.js. AVA ne les prend pas encore *en charge*, mais nous en sommes proches.

Pour le moment, AVA *sélectionne* les fichiers de test avec l'extension `.mjs`, mais refuse de les charger. De même, le champ `"type": "module"` du `package.json` est reconnu, mais s'il est défini, AVA refusera de charger les fichiers de test avec l'extension `.js`.

Pour l'instant, votre meilleure option est d'utiliser le package [`esm`](https://github.com/standard-things/esm). Assurez-vous d'utiliser l'extension `.js` et *ne* définissez *pas* `"type": "module"` dans `package.json`.

Voici comment vous devez travailler avec AVA.

D'abord, installez `esm` :

```
$ npm install esm
```

Configurez-le dans votre fichier `package.json` ou `ava.config.*` et ajoutez-le à l'option `"require"` de AVA. Assurez-vous de l'ajouter en tant que premier élément.

**`package.json` :**

```json
{
	"ava": {
		"require": [
			"esm"
		]
	}
}
```

Vous pouvez maintenant utiliser les modules ES natifs avec AVA :

```js
// sum.js
export default function sum(a, b) {
	return a + b;
};
```

```js
// test.js
const test = require('ava');
const sum = require('./sum');

test('2 + 2 = 4', t => {
	t.is(sum(2, 2), 4);
});
```
