___
**Note du traducteur**

C'est la traduction du fichier [typescript.md](https://github.com/avajs/ava/blob/master/docs/recipes/typescript.md). Voici un [lien](https://github.com/avajs/ava/compare/2a6c5a9fc042fe7f96a76ff4a012d77c67c43188...master#diff-60cce07a584082115d230f2e3d571ad6) vers les différences avec le master de AVA (Si en cliquant sur le lien, vous ne trouvez pas le fichier `typescript.md` parmi les fichiers modifiés, vous pouvez donc en déduire que la traduction est à jour).
___
# TypeScript

Traductions : [English](https://github.com/avajs/ava/blob/master/docs/recipes/typescript.md), [Italiano](https://github.com/avajs/ava-docs/blob/master/it_IT/docs/recipes/typescript.md), [Русский](https://github.com/avajs/ava-docs/blob/master/ru_RU/docs/recipes/typescript.md), [简体中文](https://github.com/avajs/ava-docs/blob/master/zh_CN/docs/recipes/typescript.md)

AVA est livré avec un fichier de définition TypeScript. Cela permet aux développeurs de profiter de TypeScript pour écrire des tests.

## Configuration

Installez d'abord [TypeScript](https://github.com/Microsoft/TypeScript).

```
$ npm install --save-dev typescript
```

Créez un fichier [`tsconfig.json`](https://github.com/Microsoft/TypeScript/wiki/tsconfig.json). Ce fichier spécifie les options du compilateur nécessaires pour compiler le projet ou les fichiers de test.

```json
{
	"compilerOptions": {
		"module": "commonjs",
		"target": "es2015"
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

async function fn() {
    return Promise.resolve('foo');
}

test(async (t) => {
    t.is(await fn(), 'foo');
});
```


## Exécutez les tests

```
$ npm test
```
