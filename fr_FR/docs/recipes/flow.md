___
**Note du traducteur**

C'est la traduction du fichier [flow.md](https://github.com/avajs/ava/blob/master/docs/recipes/flow.md). Voici un [lien](https://github.com/avajs/ava/compare/6fb0964941b66ce1cc613c62d0b74c3301398d95...master#diff-d7e81d64f6d02f405e93d77a24b6a4af) vers les différences avec le master de AVA (Si en cliquant sur le lien, vous ne trouvez pas le fichier `flow.md` parmi les fichiers modifiés, vous pouvez donc en déduire que la traduction est à jour).
___
# Flow

Traductions : [English](https://github.com/avajs/ava/blob/master/docs/recipes/flow.md)

AVA est livré avec un fichier de définition de Flow. Cela permet aux développeurs de tirer parti de Flow pour l'écriture de test.

Ce guide suppose que vous avez déjà configuré Flow pour votre projet. Notez que la définition de AVA a été testée avec la version 0.78.0.

Nous vous recommandons d'utiliser le pipeline Babel intégré de AVA pour supprimer les annotations et les déclarations de type Flow. AVA applique automatiquement la configuration Babel de votre projet, donc tout peut fonctionner sans changements. Sinon, installez [`@babel/plugin-transform-flow-strip-types`](https://www.npmjs.com/package/@babel/plugin-transform-flow-strip-types) et personnalisez la configuration de AVA dans le fichier `package.json` (ou le fichier `ava.config.js`) comme suit :

```json
{
	"ava": {
		"babel": {
			"testOptions": {
				"plugins": ["@babel/plugin-transform-flow-strip-types"]
			}
		}
	}
}
```

Consultez notre [documentation Babel](babel.md) pour plus de détails.

## Ecritures des tests

Créez un fichier `test.js`.

```js
// @flow
import test from 'ava';

const fn = async () => Promise.resolve('foo');

test(async (t) => {
	t.is(await fn(), 'foo');
});
```

## Typage de [`t.context`](https://github.com/avajs/ava#test-context)

Par défaut, le type de `t.context` sera un objet vide (`{}`). AVA expose une interface `TestInterface<Context>` qui vous permet de l'utiliser pour appliquer votre propre type `t.context`. Cela peut vous aider à détecter les erreurs lors de la compilation :

```js
// @flow
import anyTest from 'ava';
import type {TestInterface} from 'ava';

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
