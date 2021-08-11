___
**Note du traducteur**

C'est la traduction du fichier [browser-testing.md](https://github.com/avajs/ava/blob/main/docs/recipes/browser-testing.md). Voici un [lien](https://github.com/avajs/ava/compare/79b2ea30c125f44e4d47bdafdeec351cddb5911a...main#diff-9d3d394077fa7f97cbbb0fefc098ac60) vers les différences avec le master de AVA (Si en cliquant sur le lien, vous ne trouvez pas le fichier `browser-testing.md` parmi les fichiers modifiés, vous pouvez donc en déduire que la traduction est à jour).
___
# Configuration de AVA pour des tests de navigateur

 Traductions : [English](https://github.com/avajs/ava/raw/main/docs/recipes/browser-testing.md), [Español](https://github.com/avajs/ava-docs/blob/main/es_ES/docs/recipes/browser-testing.md), [Italiano](https://github.com/avajs/ava-docs/blob/main/it_IT/docs/recipes/browser-testing.md), [Русский](https://github.com/avajs/ava-docs/blob/main/ru_RU/docs/recipes/browser-testing.md), [简体中文](https://github.com/avajs/ava-docs/blob/main/zh_CN/docs/recipes/browser-testing.md)

AVA ne supporte pas [encore](https://github.com/avajs/ava/issues/24) l'exécution de tests dans les navigateurs. Cependant, les bibliothèques JavaScript nécessitant des globales, spécifiques aux navigateurs (`window`, `document`, `navigator`, etc) peuvent encore être testé avec AVA en maquettant ces globales.

Cette recette fonctionne pour toutes les bibliothèques qui ont besoin d'un environnement de navigateur maquetté.

## Installez browser-env

> **❗️ Note importante**
>
>`browser-env` ajoute des propriétés depuis l'espace nommé window de `jsdom` à l'espace nommé global de Node.js. Ceci est explicitement [déconseillé](https://github.com/tmpvar/jsdom/wiki/Don't-stuff-jsdom-globals-onto-the-Node-global) par `jsdom`. Veuillez lire la page du wiki (lien "déconseillé") et vous assurer de comprendre les mises en garde. Si vous n'avez pas beaucoup de dépendances qui nécessitent également un environnement du navigateur, alors [`window`](https://github.com/lukechilds/window#universal-testing-pattern) peut être une meilleure solution.

Installez [browser-env](https://github.com/lukechilds/browser-env).

> Simule un environnement global d'un navigateur en utilisant jsdom.

```
$ npm install --save-dev browser-env
```

## Configuration de browser-env

Créez un fichier helper, préfixez le avec un underscore. Cela permet de garantir que AVA ne le traitera pas comme un fichier de test.

`test/_setup-browser-env.js`:

```js
const browserEnv = require('browser-env');
browserEnv();
```

Par défaut, `browser-env` ajoutera les variables globales du navigateur à la portée globale de Node.js : la création d'un environnement complet d'un navigateur. Cela permet d'avoir une bonne compatibilité avec la plupart des bibliothèques front-end, cependant, ce n'est généralement pas une bonne idée de créer beaucoup de variables globales si vous ne devez pas les utiliser. Si vous savez exactement quelles globales vous avez besoin, vous pouvez leur passer un tableau.

```js
const browserEnv = require('browser-env');
browserEnv(['window', 'document', 'navigator']);
```

Vous pouvez exposer plus de variables globales en les affectant à l'objet `global`. Par exemple, jQuery est généralement disponible via la variable `$` :

```js
const browserEnv = require('browser-env');
const jQuery = require('jquery');

browserEnv();
global.$ = jQuery(window);
```

## Configurez les tests pour qu'ils utilisent browser-env

Configurez le `require` de AVA avec le helper pour l'exiger avant chaque fichier de test.

**`package.json` :**

```json
{
	"ava": {
		"require": [
			"./test/_setup-browser-env.js"
		]
	}
}
```

## Amusez-vous !

Écrivez vos tests et profitez d'un environnement de navigateur maquetté.

`test.js`:

```js
const test = require('ava');

test('Insérer au DOM', t => {
	const div = document.createElement('div');
	document.body.appendChild(div);

	t.is(document.querySelector('div'), div);
});
```
