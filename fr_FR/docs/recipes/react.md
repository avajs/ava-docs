___
**Note du traducteur**

C'est la traduction du fichier [react.md](https://github.com/avajs/ava/blob/main/docs/recipes/react.md). Voici un [lien](https://github.com/avajs/ava/compare/b208d143ad852dc95aa8b44eed94ac1f404a25f4...main#diff-60cb6077059951c16eae00b64c2505290aebf57a66e29c37a23f03cc51b68581) vers les différences avec le master de AVA (Si en cliquant sur le lien, vous ne trouvez pas le fichier `react.md` parmi les fichiers modifiés, vous pouvez donc en déduire que la traduction est à jour).
___
# Tester les composants React avec AVA 3

Traductions : [English](https://github.com/avajs/ava/raw/main/docs/recipes/react.md), [Español](https://github.com/avajs/ava-docs/blob/main/es_ES/docs/recipes/react.md)

**AVA 4 n'a plus de support Babel intégré, et `t.snapshot()` et `t.deepEqual()` ne reconnaissent plus les éléments React non plus. Par conséquent, cette recette s'adresse principalement aux utilisateurs d'AVA 3**.

## Installer Babel

Lorsque vous [activez Babel](https://github.com/avajs/babel), AVA 3 étendra automatiquement votre configuration Babel actuelle (au niveau du projet). Vous devriez être en mesure d'utiliser React dans vos fichiers de test sans aucune configuration supplémentaire.

Cependant, si vous voulez le configurer explicitement, ajoutez le preset aux options de test dans le pipeline Babel d'AVA en modifiant votre `package.json` ou votre fichier `ava.config.*`.

**`package.json` :**

```json
{
	"ava": {
		"babel": {
			"testOptions": {
				"presets": ["@babel/preset-react"]
			}
		}
	}
}
```

Vous pouvez trouver plus d'informations dans [`@ava/babel`](https://github.com/avajs/babel).

## Utiliser [Enzyme](https://github.com/airbnb/enzyme)

Commençons par voir comment utiliser AVA avec l'une des bibliothèques de test de React parmi les plus populaires : [Enzyme](https://github.com/enzymejs/enzyme).

Si vous comptez utiliser seulement [le rendu de composants shallow](https://facebook.github.io/react/docs/test-utils.html#shallow-rendering), vous n'avez pas besoin de configuration supplémentaire.

### Installez Enzyme

Premièrement, installez [Enzyme et son adaptateur requis](https://github.com/enzymejs/enzyme#installation) :

```console
$ npm install --save-dev enzyme enzyme-adapter-react-16
```

### Mettez en place Enzyme

Créez un fichier helper, préfixé par un underscore. Cela permet à AVA de ne pas le traiter comme un test.

`test/_setup-enzyme-adapter.js`:

```js
import Enzyme from 'enzyme';
import Adapter from 'enzyme-adapter-react-16';

Enzyme.configure({
	adapter: new Adapter()
});
```

### Configurez des tests pour utiliser Enzyme

Configurez AVA pour faire un `require` du helper avant chaque fichier de test.

**`package.json` :**

```json
{
	"ava": {
		"require": [
			"./test/_setup-enzyme-adapter.js"
		]
	}
}
```

### C'est prêt !

Ensuite vous pouvez utiliser Enzyme directement :

`test.js`:

```js
import test from 'ava';
import React from 'react';
import PropTypes from 'prop-types';
import {shallow} from 'enzyme';

const Foo = ({children}) =>
	<div className="Foo">
		<span className="bar">bar</span>
		{children}
		<span className="bar">bar</span>
	</div>;

Foo.propTypes = {
	children: PropTypes.any
};

test('a une classe .Foo', t => {
	const wrapper = shallow(<Foo/>);
	t.true(wrapper.hasClass('Foo'));
});

test('rend un `.Bar`', t => {
	const wrapper = shallow(<Foo/>);
	t.is(wrapper.find('.bar').length, 2);
});

test('rend un enfant quand il est passé', t => {
	const wrapper = shallow(
		<Foo>
			<div className="unique"/>
		</Foo>
	);
	t.true(wrapper.contains(<div className="unique"/>));
});
```

Enzyme a aussi des helpers `mount` et `render` pour tester dans un vrai environnement de navigateur. Si vous voulez utiliser ces fonctions, il faudra configurer un environnement de navigateur. Pour cela, allez voir [la recette sur les tests de navigateur](/fr_FR/docs/recipes/browser-testing.md).

Pour voir un exemple d'AVA fonctionnant avec Enzyme configuré pour les tests de navigateur, vous pouvez regarder [cet exemple de projet](https://github.com/adriantoine/ava-enzyme-demo).

Ceci n'est qu'un exemple basique sur l'intégration d'Enzyme avec AVA. Pour plus d'informations sur l'utilisation d'Enzyme pour tester les composants React, allez voir [la documentation d'Enzyme](https://enzymejs.github.io/enzyme/).

## Utiliser les helpers JSX

Une autre approche aux tests de composants React est d'utiliser le package [`react-element-to-jsx-string`](https://github.com/algolia/react-element-to-jsx-string) pour comparer l'arborescence du DOM en tant que chaînes de caractères. [`jsx-test-helpers`](https://github.com/MoOx/jsx-test-helpers) est une bonne bibliothèque pour [le rendu des composants shallow](https://facebook.github.io/react/docs/test-utils.html#shallow-rendering) et convertir du JSX en chaîne de caractère pour tester les composants React en utilisant les assertions d'AVA.

```console
$ npm install --save-dev jsx-test-helpers
```

Exemple d'utilisation :

```js
import test from 'ava';
import React from 'react';
import PropTypes from 'prop-types';
import {renderJSX, JSX} from 'jsx-test-helpers';

const Foo = ({children}) =>
	<div className="Foo">
		<span className="bar">bar</span>
		{children}
		<span className="bar">bar</span>
	</div>;

Foo.propTypes = {
	children: PropTypes.any
};

test('rend un balisage correct', t => {
	const actual = renderJSX(<Foo/>);
	const expected = JSX(
		<div className="Foo">
			<span className="bar">bar</span>
			<span className="bar">bar</span>
		</div>
	);
	t.is(actual, expected);
});

test('rend un enfant quand il est passé', t => {
	const actual = renderJSX(
		<Foo>
			<div className="unique"/>
		</Foo>
	);
	const expected = JSX(
		<div className="Foo">
			<span className="bar">bar</span>
			<div className="unique"/>
			<span className="bar">bar</span>
		</div>
	);
	t.is(actual, expected);
});
```

Notez que vous devez utiliser des variables, comme `actual` et `expected` parce que [`power-assert` ne gère pas le JSX correctement](https://github.com/power-assert-js/power-assert/issues/34).

Ceci est un exemple basique de l'utilisation de `jsx-test-helpers` avec AVA. Pour voir une utilisation plus avancée de cette bibliothèque, allez voir [ce fichier de tests annoté](https://github.com/MoOx/jsx-test-helpers/blob/master/src/__tests__/index.js).

[Cet exemple de project](https://github.com/MoOx/jsx-test-helpers) montre une configuration basique et minimale d'AVA avec `jsx-test-helpers`.

## Utiliser d'autres bibliothèques d'assertion

Avec AVA, vous pouvez utiliser n'importe quelle bibliothèque d'assertion, et il y en a déjà quelques unes qui existent pour tester des composants React. Voici une liste de bibliothèque d'assertion qui marche bien avec AVA :

- [`expect-jsx`](https://github.com/algolia/expect-jsx) ([Exemple](https://github.com/avajs/ava/issues/186#issuecomment-161317068))
- [`unexpected-react`](https://github.com/bruderstein/unexpected-react) ([Exemple de projet](https://github.com/adriantoine/ava-unexpected-react-demo))
