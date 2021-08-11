___
**Note du traducteur**

C'est la traduction du fichier [flow.md](https://github.com/avajs/ava/blob/main/docs/recipes/flow.md). Voici un [lien](https://github.com/avajs/ava/compare/8fa28254dbebef32cbde05c0c9a49061d0ef82f8...master#diff-d7e81d64f6d02f405e93d77a24b6a4af) vers les différences avec le master de AVA (Si en cliquant sur le lien, vous ne trouvez pas le fichier `flow.md` parmi les fichiers modifiés, vous pouvez donc en déduire que la traduction est à jour).
___
# Flow

Traductions : [English](https://github.com/avajs/ava/raw/main/docs/recipes/flow.md)

Jusqu'à la version [1.4.1](https://github.com/avajs/ava/releases/tag/v1.4.1) AVA était livré avec un fichier de définition de Flow. Cela permettait aux développeurs d'exploiter Flow pour écrire des tests.

**Nous avons besoin d’aide pour la publication des définitions de types en dehors de AVA. Si vous souhaitez aider, veuillez nous rejoindre dans https://github.com/avajs/flow-typed/issues/1 .**

Ce guide suppose que vous avez déjà configuré Flow pour votre projet. Remarque : la définition de AVA a été testée avec la version 0.95.1.

Nous vous recommandons d'utiliser le [support de Babel](https://github.com/avajs/babel) de AVA pour supprimer les annotations et les déclarations de type Flow. AVA applique automatiquement la configuration Babel de votre projet, donc tout peut fonctionner sans changements. Sinon, installez [`@babel/plugin-transform-flow-strip-types`](https://www.npmjs.com/package/@babel/plugin-transform-flow-strip-types) et personnalisez la configuration de AVA dans le fichier `package.json` (ou le fichier `ava.config.*`) comme suit.

**`package.json` :**

```json
{
	"ava": {
		"babel": {
			"testOptions": {
				"plugins": [
					"@babel/plugin-transform-flow-strip-types"
				]
			}
		}
	}
}
```

Consultez notre [`@ava/babel`](https://github.com/avajs/babel) pour plus de détails.

## Ecritures des tests

Créez un fichier `test.js`.

```js
// @flow
const test = require('ava');

const getFoo = () => 'foo';

test('check getFoo', t => {
	t.is(getFoo(), 'foo');
});
```

## Typage de [`t.context`](../01-writing-tests.md#tester-le-contexte)

Par défaut, le type de `t.context` sera un objet vide (`{}`). AVA expose une interface `TestInterface<Context>` qui vous permet de l'utiliser pour appliquer votre propre type `t.context`. Cela peut vous aider à détecter les erreurs lors de la compilation :

```js
// @flow
const anyTest = require('ava');
const type {TestInterface} = require('ava');

const test: TestInterface<{foo: string}> = (anyTest: any);

test.beforeEach(t => {
	t.context = {foo: 'bar'};
});

test.beforeEach(t => {
	t.context.foo = 123; // erreur: le type '123' n'est pas assignable au type 'string'
});

test.serial.cb.failing('very long chains are properly typed', t => {
	t.context.fooo = 'a value'; // erreur: la propriété 'fooo' n'existe pas sur le type ''
});

test('an actual test', t => {
	t.deepEqual(t.context.foo.map(c => c), ['b', 'a', 'r']); // erreur : la propriété 'map' n'existe pas sur le type 'string'
});
```

Notez que, malgré le type de casting ci-dessus, lors de l'exécution, `t.context` est un objet vide à moins qu'il ne soit assigné.

## Typage des assertions `throws`

Les assertions `t.throws()` et `t.throwsAsync()` sont typées pour toujours retourner une Error. Vous pouvez personnaliser la classe d'erreur à l'aide de génériques :

```js
// @flow
const test = require('ava');

class CustomError extends Error {
	parent: Error;

	constructor(parent) {
		super(parent.message);
		this.parent = parent;
	}
}

function myFunc() {
	throw new CustomError(new TypeError('🙈'));
};

test('throws', t => {
	const err = t.throws<CustomError>(myFunc);
	t.is(err.parent.name, 'TypeError');
});

test('throwsAsync', async t => {
	const err = await t.throwsAsync<CustomError>(async () => myFunc());
	t.is(err.parent.name, 'TypeError');
});
```

Remarquez que, malgré le typage, l’assertion retourne `undefined` si elle échoue. Le typage des assertions retournant `Error | undefined` ne semblait pas être le choix pragmatique.
