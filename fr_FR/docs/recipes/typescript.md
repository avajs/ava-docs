___
**Note du traducteur**

C'est la traduction du fichier [typescript.md](https://github.com/avajs/ava/blob/master/docs/recipes/typescript.md). Voici un [lien](https://github.com/avajs/ava/compare/6fb0964941b66ce1cc613c62d0b74c3301398d95...master#diff-60cce07a584082115d230f2e3d571ad6) vers les différences avec le master de AVA (Si en cliquant sur le lien, vous ne trouvez pas le fichier `typescript.md` parmi les fichiers modifiés, vous pouvez donc en déduire que la traduction est à jour).
___
# TypeScript

Traductions : [English](https://github.com/avajs/ava/blob/master/docs/recipes/typescript.md), [Italiano](https://github.com/avajs/ava-docs/blob/master/it_IT/docs/recipes/typescript.md), [Русский](https://github.com/avajs/ava-docs/blob/master/ru_RU/docs/recipes/typescript.md), [简体中文](https://github.com/avajs/ava-docs/blob/master/zh_CN/docs/recipes/typescript.md)

AVA est livré avec un fichier de définition TypeScript. Cela permet aux développeurs de profiter de TypeScript pour écrire des tests.

Ce guide suppose que vous avez déjà configuré TypeScript pour votre projet. Notez que la définition de AVA a été testée avec la version 3.0.1.

## Configuration de AVA pour compiler des fichiers TypeScript à la volée

Vous pouvez configurer AVA pour reconnaître les fichiers TypeScript. Ensuite, avec `ts-node` installé, vous pouvez les compiler à la volée :

```json
{
	"ava": {
		"compileEnhancements": false,
		"extensions": [
			"ts"
		],
		"require": [
			"ts-node/register"
		]
	}
}
```

Il est intéressant de noter qu'avec cette configuration, les tests échoueront s'il y a des erreurs de construction du TypeScript. Si vous voulez tester en ignorant ces erreurs, vous pouvez utiliser `ts-node/register/transpile-only` à la place de `ts-node/register`.

## Compilation des fichiers TypeScript avant d'exécuter AVA

Ajoutez un script `test` dans le fichier `package.json`. Cela compilera d'abord le projet puis exécutera AVA.

```json
{
	"scripts": {
		"test": "tsc && ava"
	}
}
```

Make sure that AVA runs your built TypeScript files.

## Ecriture des tests

Créez un fichier `test.ts`.

```ts
import test from 'ava';

const fn = async () => Promise.resolve('foo');

test(async (t) => {
	t.is(await fn(), 'foo');
});
```

## Utilisation des [macros](https://github.com/avajs/ava-docs/tree/master/fr_FR#macros-de-test)

Pour pouvoir attribuer la propriété `title` à une macro vous devez typer la fonction :

```ts
import test, {Macro} from 'ava';

const macro: Macro = (t, input: string, expected: number) => {
	t.is(eval(input), expected);
};
macro.title = (providedTitle: string, input: string, expected: number) => `${providedTitle} ${input} = ${expected}`.trim();

test(macro, '2 + 2', 4);
test(macro, '2 * 3', 6);
test('providedTitle', macro, '3 * 3', 9);
```

Vous aurez besoin d'un type différent, si vous voulez que votre macro soit utilisée avec un test de callback :

```ts
import test, {CbMacro} from 'ava';

const macro: CbMacro = t => {
	t.pass();
	setTimeout(t.end, 100);
};

test.cb(macro);
```

## Typing [`t.context`](https://github.com/avajs/ava#test-context)

Par défaut, le type de `t.context` sera un objet vide (`{}`). AVA expose une interface `TestInterface<Context>` qui vous permet de l'utiliser pour appliquer votre propre type `t.context`. Cela peut vous aider à détecter les erreurs lors de la compilation :

```ts
import anyTest, {TestInterface} from 'ava';

const test = anyTest as TestInterface<{foo: string}>;

test.beforeEach(t => {
	t.context = {foo: 'bar'};
});

test.beforeEach(t => {
	t.context.foo = 123; // erreur : le type '123' n'est pas assignable au type 'string'
});

test.serial.cb.failing('Les chaînes très longues sont correctement typées', t => {
	t.context.fooo = 'a value'; // erreur : la propriété 'fooo' n'existe pas sur le type ''
});

test('un test normal', t => {
	t.deepEqual(t.context.foo.map(c => c), ['b', 'a', 'r']); // erreur : La propriété 'map' n'existe pas sur le type 'string'
});
```

Vous pouvez également typer le contexte lors de la création de macros :

```ts
import anyTest, {Macro, TestInterface} from 'ava';
 
interface Context {
	foo: string
}

const test = anyTest as TestInterface<Context>;

const macro: Macro<Context> = (t, expected: string) => {
	t.is(t.context.foo, expected);
};

test.beforeEach(t => {
	t.context = {foo: 'bar'};
});

test('foo est bar', macro, 'bar');
```

Notez que malgré le type de casting ci-dessus, lors de l'exécution, `t.context` est un objet vide à moins qu'il ne soit assigné.
