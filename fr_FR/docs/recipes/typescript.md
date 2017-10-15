___
**Note du traducteur**

C'est la traduction du fichier [typescript.md](https://github.com/avajs/ava/blob/master/docs/recipes/typescript.md). Voici un [lien](https://github.com/avajs/ava/compare/68ce4b813afcf81f31e48212ab04cb6fcf224457...master#diff-60cce07a584082115d230f2e3d571ad6) vers les différences avec le master de AVA (Si en cliquant sur le lien, vous ne trouvez pas le fichier `typescript.md` parmi les fichiers modifiés, vous pouvez donc en déduire que la traduction est à jour).
___
# TypeScript

Traductions : [English](https://github.com/avajs/ava/blob/master/docs/recipes/typescript.md), [Italiano](https://github.com/avajs/ava-docs/blob/master/it_IT/docs/recipes/typescript.md), [Русский](https://github.com/avajs/ava-docs/blob/master/ru_RU/docs/recipes/typescript.md), [简体中文](https://github.com/avajs/ava-docs/blob/master/zh_CN/docs/recipes/typescript.md)

AVA est livré avec un fichier de définition TypeScript. Cela permet aux développeurs de profiter de TypeScript pour écrire des tests.


## Configuration

Installez d'abord [TypeScript](https://github.com/Microsoft/TypeScript). (Si vous l'avez déjà installé, assurez-vous d'utiliser la version 2.1 ou supérieure).

```
$ npm install --save-dev typescript
```

Créez un fichier [`tsconfig.json`](http://www.typescriptlang.org/docs/handbook/tsconfig-json.html). Ce fichier spécifie les options du compilateur nécessaires pour compiler le projet ou les fichiers de test.

```json
{
	"compilerOptions": {
		"module": "commonjs",
		"target": "es2015",
		"sourceMap": true
	}
}
```

Ajoutez un script `test` dans le fichier `package.json`. Cela compilera d'abord le projet puis exécutera AVA.

```json
{
	"scripts": {
		"test": "tsc && ava"
	}
}
```


## Ajoutez les tests

Créez un fichier `test.ts`.

```ts
import test from 'ava';

const fn = async () => Promise.resolve('foo');

test(async (t) => {
	t.is(await fn(), 'foo');
});
```


## Travailler avec [`context`](https://github.com/avajs/ava-docs/tree/master/fr_FR#tester-le-contexte)

Par défaut, le type de `t.context` sera [`any`](https://www.typescriptlang.org/docs/handbook/basic-types.html#any). AVA expose une interface `RegisterContextual<T>` que vous pouvez utiliser pour appliquer votre propre type à `t.context`. Cela peut vous aider à détecter des erreurs lors de la compilation :

```ts
import * as ava from 'ava';

function contextualize<T>(getContext: () => T): ava.RegisterContextual<T> {
	ava.test.beforeEach(t => {
		Object.assign(t.context, getContext());
	});

	return ava.test;
}

const test = contextualize(() => ({ foo: 'bar' }));

test.beforeEach(t => {
	t.context.foo = 123; // erreur :  Type '123' n'est pas assignable au type 'string'
});

test.after.always.failing.cb.serial('Les chaînes très longues sont correctement typées', t => {
	t.context.fooo = 'a value'; // erreur : La propriété 'fooo' n'existe pas sur le type '{ foo: string }'
});

test('un test normal', t => {
	t.deepEqual(t.context.foo.map(c => c), ['b', 'a', 'r']); // erreur : La propriété 'map' n'existe pas sur le type 'string'
});
```


## Exécutez les tests

```
$ npm test
```
