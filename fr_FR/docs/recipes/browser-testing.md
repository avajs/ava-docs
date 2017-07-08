___
**Note du traducteur**

C'est la traduction du fichier [browser-testing.md](https://github.com/avajs/ava/blob/master/docs/recipes/browser-testing.md). Voici un [lien](https://github.com/avajs/ava/compare/589489db04128f9287de44e600175b4af5a2f52d...master#diff-9d3d394077fa7f97cbbb0fefc098ac60) vers les différences avec le master de AVA (Si en cliquant sur le lien, vous ne trouvez pas le fichier `browser-testing.md` parmi les fichiers modifiés, vous pouvez donc en déduire que la traduction est à jour).
___
# Configuration de AVA pour des tests de navigateur

 Traductions : [English](https://github.com/avajs/ava/blob/master/docs/recipes/browser-testing.md), [Español](https://github.com/avajs/ava-docs/blob/master/es_ES/docs/recipes/browser-testing.md), [Italiano](https://github.com/avajs/ava-docs/blob/master/it_IT/docs/recipes/browser-testing.md), [Русский](https://github.com/avajs/ava-docs/blob/master/ru_RU/docs/recipes/browser-testing.md), [简体中文](https://github.com/avajs/ava-docs/blob/master/zh_CN/docs/recipes/browser-testing.md)

AVA ne supporte pas [encore](https://github.com/avajs/ava/issues/24) l'exécution de tests dans les navigateurs. Certaines bibliothèques exigent des globales, spécifiques aux navigateurs (`window`, `document`, `navigator`, etc).
Par exemple, c'est le cas de React, si vous voulez utiliser ReactDOM.render et simuler les événements avec ReactTestUtils.

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

Créez un fichier helper et placez le dans le répertoire `test/helpers`. Cela permet de garantir que AVA ne le traitera pas comme un fichier de test.

`test/helpers/setup-browser-env.js`:

```js
import browserEnv from 'browser-env';
browserEnv();
```

Par défaut, `browser-env` ajoutera les variables globales du navigateur à la portée globale de Node.js : la création d'un environnement complet d'un navigateur. Cela permet d'avoir une bonne compatibilité avec la plupart des bibliothèques front-end, cependant, ce n'est généralement pas une bonne idée de créer beaucoup de variables globales si vous ne devez pas les utiliser. Si vous savez exactement quelles globales vous avez besoin, vous pouvez leur passer un tableau.

```js
import browserEnv from 'browser-env';
browserEnv(['window', 'document', 'navigator']);
```

## Configurez les tests pour qu'ils utilisent browser-env

Configurez le `require` de AVA avec le helper pour l'exiger avant chaque fichier de test.

`package.json`:

```json
{
	"ava": {
		"require": [
			"./test/helpers/setup-browser-env.js"
		]
	}
}
```

## Amusez-vous !

Écrivez vos tests et profitez d'un environnement de navigateur maquetté.

`test/my.dom.test.js`:

```js
import test from 'ava';

test('Insérer au DOM', t => {
	const div = document.createElement('div');
	document.body.appendChild(div);

	t.is(document.querySelector('div'), div);
});
```

`test/my.react.test.js`:

```js
import test from 'ava';
import React from 'react';
import {render} from 'react-dom';
import {Simulate} from 'react-addons-test-utils';
import sinon from 'sinon';
import CustomInput from './components/custom-input.jsx';

test('Input appelle onBlur', t => {
	const onUserBlur = sinon.spy();
	const input = render(
		React.createElement(CustomInput, onUserBlur),
		div
	);

	Simulate.blur(input);

	t.true(onUserBlur.calledOnce);
});
```
