___
**Note du traducteur**

C'est la traduction du fichier [typescript.md](https://github.com/avajs/ava/blob/main/docs/recipes/typescript.md). Voici un [lien](https://github.com/avajs/ava/compare/b208d143ad852dc95aa8b44eed94ac1f404a25f4...main#diff-d59f2c2342aa1c060312888a2ce86dd1885ae88d28e3ceef6f6ab55a01954f22) vers les différences avec le master de AVA (Si en cliquant sur le lien, vous ne trouvez pas le fichier `typescript.md` parmi les fichiers modifiés, vous pouvez donc en déduire que la traduction est à jour).
___
# TypeScript

Traductions : [English](https://github.com/avajs/ava/raw/main/docs/recipes/typescript.md), [Italiano](https://github.com/avajs/ava-docs/blob/main/it_IT/docs/recipes/typescript.md), [Русский](https://github.com/avajs/ava-docs/blob/main/ru_RU/docs/recipes/typescript.md), [简体中文](https://github.com/avajs/ava-docs/blob/main/zh_CN/docs/recipes/typescript.md)

AVA est livré avec un fichier de définition TypeScript. Cela permet aux développeurs de profiter de TypeScript pour écrire des tests.

Ce guide suppose que vous avez déjà configuré TypeScript pour votre projet. Notez que la définition d'AVA attend au minimum la version 4.4.

## Activation de la prise en charge d'Ava pour les fichiers de tests TypeScript

### Avec une étape de précompilation

Par défaut, AVA ne charge pas les fichiers de test TypeScript. Vous pouvez utiliser notre paquetage [`@ava/typescript`], qui est conçu pour fonctionner pour les projets qui précompilent TypeScript en utilisant la commande `tsc`. Veuillez consulter [`@ava/typescript`] pour les instructions d'installation.

### Utilisation de `ts-node`

Vous pouvez utiliser [`ts-node`] pour faire des tests en direct sans transpiler. Cela peut être particulièrement utile lorsque vous utilisez un bundler. Assurez-vous d'installer les dépendances dev requises :

`npm install --save-dev typescript ts-node`

La configuration requise dépend du type de votre package :

1. [pour les packages avec le type "module"](#for-packages-with-type-module)
2. [pour les packages sans le type "module"](#for-packages-without-type-module)

#### Pour les packages avec le type `module`

Si votre `package.json` a `"type": "module"`, alors c'est la configuration suivante de AVA dont vous avez besoin :

`package.json`:

```json
{
	"ava": {
		"extensions": {
			"ts": "module"
		},
		"nodeArguments": [
			"--loader=ts-node/esm"
		]
	}
}
```

Vous devez également avoir ceci dans votre `tsconfig.json` :

```json
{
	"compilerOptions": {
		"module": "ES2020",
		"moduleResolution": "node"
	}
}
```

Rappelez-vous que, par défaut, les modules ES vous demandent de spécifier l'extension du fichier et que TypeScript produit des fichiers `.js`, donc vous devez écrire vos importations pour charger des fichiers `.js` et non `.ts`.

Si cela n'est pas à votre goût, il y a une option _expérimentale_ dans Node.js que vous pouvez utiliser. Vous pouvez l'ajouter au tableau `nodeArguments` dans la configuration de AVA afin qu'elle s'applique à vos tests : [`--experimental-specifier-resolution=node`](https://nodejs.org/api/esm.html#customizing-esm-specifier-resolution-algorithm).

#### Pour les packages sans le type "module"

Si votre `package.json` n'a pas `"type": "module"`, alors c'est la configuration suivante de AVA dont vous avez besoin :

`package.json`:

```json
{
	"ava": {
		"extensions": [
			"ts"
		],
		"require": [
			"ts-node/register"
		]
	}
}
```

Il est intéressant de noter qu'avec cette configuration, les tests échoueront s'il y a des erreurs de compilation de TypeScript. Si vous voulez tester en ignorant ces erreurs, vous pouvez utiliser `ts-node/register/transpile-only` au lieu de `ts-node/register`.

## Ecriture des tests

[![Ouvrir dans StackBlitz](https://developer.stackblitz.com/img/open_in_stackblitz.svg)](https://stackblitz.com/github/avajs/ava/tree/main/examples/typescript-basic?file=source%2Ftest.ts&terminal=test&view=editor)

Créez un fichier `test.ts`. La syntaxe ESM est la meilleure, même si vous utilisez CommonJS.

```ts
import test from 'ava';

const fn = () => 'foo';

test('fn() returns foo', t => {
	t.is(fn(), 'foo');
});
```

Vous pouvez également utiliser la syntaxe CommonJS :

```ts
const test = require('ava');
```

Cela fonctionne selon si `esModuleInterop` est activé ou non.

La syntaxe `import ... = require()` est moins évocatrice. C'est mieux comme ça :

```ts
import ava = require('ava')

const test = ava.default;
```

## Utilisation des [macros](../01-writing-tests.md#réutilisation-de-test-logique-à-travers-des-macros)

Les macros peuvent recevoir des arguments supplémentaires. AVA peut déduire ceux-ci pour garantir que vous utilisez la macro correctement :

```ts
import test, {ExecutionContext} from 'ava';

const hasLength = (t: ExecutionContext, input: string, expected: number) => {
	t.is(input.length, expected);
};

test('bar fait 3 de long', hasLength, 'bar', 3);
```

Cependant, si vous utilisez le helper `test.macro()`, vous obtiendrez une bien meilleure inférence de type :

```ts
import test from 'ava';

const macro = test.macro((t, input: string, expected: number) => {
	t.is(eval(input), expected);
});

test('title', macro, '3 * 3', 9);
```

Ou avec une fonction title :

```ts
import test from 'ava';

const macro = test.macro({
	exec(t, input: string, expected: number) {
		t.is(eval(input), expected);
	},
	title(providedTitle = '', input, expected) {
		return `${providedTitle} ${input} = ${expected}`.trim();
	}
});

test(macro, '2 + 2', 4);
test(macro, '2 * 3', 6);
test('providedTitle', macro, '3 * 3', 9);
```

## Typage de [`t.context`](../01-writing-tests.md#tester-le-contexte)

[![Ouvrir dans StackBlitz](https://developer.stackblitz.com/img/open_in_stackblitz.svg)](https://stackblitz.com/github/avajs/ava/tree/main/examples/typescript-context?file=source%2Ftest.ts&terminal=test&view=editor)

Par défaut, le type de `t.context` sera un objet vide (`{}`). AVA expose une interface `TestFn<Context>` qui vous permet de l'utiliser pour appliquer votre propre type à `t.context`. Cela peut vous aider à détecter les erreurs lors de la compilation :

```ts
import anyTest, {TestFn} from 'ava';

const test = anyTest as TestFn<{foo: string}>;

test.beforeEach(t => {
	t.context = {foo: 'bar'};
});

test.beforeEach(t => {
	t.context.foo = 123; // erreur : le type '123' n'est pas assignable au type 'string'
});

test.serial.failing('very long chains are properly typed', t => {
	t.context.fooo = 'a value'; // erreur : la propriété 'fooo' n'existe pas sur le type ''
});

test('un test normal', t => {
	t.deepEqual(t.context.foo.map(c => c), ['b', 'a', 'r']); // erreur : La propriété 'map' n'existe pas sur le type 'string'
});
```

Notez que malgré le type de casting ci-dessus, lors de l'exécution, `t.context` est un objet vide à moins qu'il ne soit assigné.

## Typage des assertions `throws`

Les assertions `t.throws()` et `t.throwsAsync()` sont typées pour toujours retourner une Error. Vous pouvez personnaliser la classe d'erreur à l'aide de génériques :

```ts
import test from 'ava';

class CustomError extends Error {
	parent: Error

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

### Utilisation du mappage du chemin du module

`ts-node` [ne prend pas en charge le mappage du chemin du module](https://github.com/TypeStrong/ts-node/issues/138), cependant vous pouvez utiliser [`tsconfig-paths`](https://github.com/dividab/tsconfig-paths#readme).

Une fois installé, ajoutez l'entrée `tsconfig-paths/register` à la section `require` de la configuration de AVA :

`package.json`:

```json
{
	"ava": {
		"extensions": [
			"ts"
		],
		"require": [
			"ts-node/register",
			"tsconfig-paths/register"
		]
	}
}
```

Ensuite, vous pouvez commencer à utiliser des alias de modules :

`tsconfig.json`:
```json
{
	"baseUrl": ".",
	"paths": {
		"@helpers/*": ["helpers/*"]
	}
}
```

Test :

```ts
import myHelper from '@helpers/myHelper';

// Reste du fichier
```

[`@ava/typescript`]: https://github.com/avajs/typescript
[`ts-node`]: https://www.npmjs.com/package/ts-node
