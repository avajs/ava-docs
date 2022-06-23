___
**Note du traducteur**

C'est la traduction du fichier [readme.md](https://github.com/avajs/eslint-plugin-ava/blob/main/readme.md). Voici un [lien](https://github.com/avajs/eslint-plugin-ava/compare/fb251f3fa84007aec9a3864f9f565e8692b09045...main#diff-5a831ea67cf5cf8703b0de46901ab25bd191f56b320053be9332d9a3b0d01d15) vers les différences avec le master de eslint-plugin-ava (Si en cliquant sur le lien, vous ne trouvez pas le fichier `readme.md` parmi les fichiers modifiés, vous pouvez donc en déduire que la traduction est à jour).
___
# eslint-plugin-ava [![Coverage Status](https://coveralls.io/repos/github/avajs/eslint-plugin-ava/badge.svg?branch=main)](https://coveralls.io/github/avajs/eslint-plugin-ava?branch=main)

> Règles ESLint pour [AVA](https://avajs.dev)

Traductions : [English](https://github.com/avajs/eslint-plugin-ava/blob/main/readme.md)

Ce plugin est intégré dans [XO](https://github.com/xojs/xo). Pas besoin de faire quoi que ce soit si vous utilisez XO.

[**Proposez ou contribuez à une nouvelle règle ➡**](https://github.com/avajs/eslint-plugin-ava/blob/main/.github/contributing.md)

## L'installation

```
$ npm install --save-dev eslint eslint-plugin-ava
```

## L'utilisation

Configurez le dans le `package.json`.

```json
{
	"name": "my-awesome-project",
	"eslintConfig": {
		"env": {
			"es6": true
		},
		"parserOptions": {
			"ecmaVersion": "latest",
			"sourceType": "module"
		},
		"plugins": [
			"ava"
		],
		"rules": {
			"ava/assertion-arguments": "error",
			"ava/hooks-order": "error",
			"ava/max-asserts": [
				"off",
				5
			],
			"ava/no-async-fn-without-await": "error",
			"ava/no-duplicate-modifiers": "error",
			"ava/no-identical-title": "error",
			"ava/no-ignored-test-files": "error",
			"ava/no-import-test-files": "error",
			"ava/no-incorrect-deep-equal": "error",
			"ava/no-inline-assertions": "error",
			"ava/no-nested-tests": "error",
			"ava/no-only-test": "error",
			"ava/no-skip-assert": "error",
			"ava/no-skip-test": "error",
			"ava/no-todo-implementation": "error",
			"ava/no-todo-test": "warn",
			"ava/no-unknown-modifiers": "error",
			"ava/prefer-async-await": "error",
			"ava/prefer-power-assert": "off",
			"ava/prefer-t-regex": "error",
			"ava/test-title": "error",
			"ava/test-title-format": "off",
			"ava/use-t": "error",
			"ava/use-t-throws-async-well": "error",
			"ava/use-t-well": "error",
			"ava/use-test": "error",
			"ava/use-true-false": "error"
		}
	}
}
```

## Les règles

Les règles s'activeront uniquement dans les fichiers de test.

- [assertion-arguments](docs/rules/assertion-arguments.md) - Imposer le passage d'arguments corrects aux assertions.
- [hooks-order](docs/rules/hooks-order.md) - Imposer l'ordre des hooks de test. *(réparable)*
- [max-asserts](docs/rules/max-asserts.md) - Limiter le nombre d'assertions dans un test.
- [no-async-fn-without-await](docs/rules/no-async-fn-without-await.md) - S'assurer que les tests async utilisent `await`.
- [no-duplicate-modifiers](docs/rules/no-duplicate-modifiers.md) - S'assurer qu'aucun test n'a de modificateur en doublon.
- [no-identical-title](docs/rules/no-identical-title.md) - S'assurer que chaque test a un titre différent.
- [no-ignored-test-files](docs/rules/no-ignored-test-files.md) - S'assurer qu'aucun test ne soit écrit dans des fichiers ignorés.
- [no-import-test-files](docs/rules/no-import-test-files.md) - S'assurer qu'aucun fichier de test n'est importé de n'importe où.
- [no-incorrect-deep-equal](docs/rules/no-incorrect-deep-equal.md) - Éviter d'utiliser `deepEqual` avec des primitives. *(réparable)*
- [no-inline-assertions](docs/rules/no-inline-assertions.md) - S'assurer que les assertions ne sont pas appelées depuis des fonctions fléchées sur une ligne. *(réparable)*
- [no-nested-tests](docs/rules/no-nested-tests.md) - S'assurer qu'aucun test soit imbriqué.
- [no-only-test](docs/rules/no-only-test.md) - S'assurer qu'aucun `test.only()` soit présent.
- [no-skip-assert](docs/rules/no-skip-assert.md) - S'assurer qu'aucune assertion soit passée (skip).
- [no-skip-test](docs/rules/no-skip-test.md) - S'assurer qu'aucun test soit passé (skip).
- [no-todo-implementation](docs/rules/no-todo-implementation.md) - S'assurer que `test.todo()` n'a pas une fonction d'implémentation.
- [no-todo-test](docs/rules/no-todo-test.md) - S'assurer qu'aucun `test.todo()` soit utilisé.
- [no-unknown-modifiers](docs/rules/no-unknown-modifiers.md) - Empêcher l'utilisation de modificateurs de test inconnus.
- [prefer-async-await](docs/rules/prefer-async-await.md) - Préférer l'utilisation de async/await au lieu de retourner une Promesse.
- [prefer-power-assert](docs/rules/prefer-power-assert.md) - Autoriser uniquement l'utilisation des assertions qui n'ont pas d'alternatives dans [power-assert](https://github.com/power-assert-js/power-assert).
- [prefer-t-regex](docs/rules/prefer-t-regex.md) - Préférer l'utilisation de `t.regex()` au lieu de tests d'expressions régulières. *(réparable)*
- [test-title](docs/rules/test-title.md) - S'assurer que les tests ont un titre.
- [test-title-format](docs/rules/test-title-format.md) - S'assurer que les titres des tests ont un certain format.
- [use-t](docs/rules/use-t.md) - S'assurer que les fonctions de test utilisent `t` comme paramètre.
- [use-t-throws-async-well](docs/rules/use-t-throws-async-well.md) - S'assurer que `t.throwsAsync()` et `t.notThrowsAsync()` sont attendus. *(correction partielle)*
- [use-t-well](docs/rules/use-t-well.md) - Empêcher une mauvaise utilisation de `t`. *(correction partielle)*
- [use-test](docs/rules/use-test.md) - S'assurer que AVA est importé avec la variable nommée `test`.
- [use-true-false](docs/rules/use-true-false.md) - S'assurer que `t.true()`/`t.false()` sont utilisés à la place de `t.truthy()`/`t.falsy()`.

## Config recommandée

Ce plugin exporte une [config recommandée](https://github.com/avajs/eslint-plugin-ava/blob/main/index.js) (`recommended`) qui applique les bonnes pratiques.

Activez la dans votre `package.json` avec l'option `extends` :

```json
{
	"name": "my-awesome-project",
	"eslintConfig": {
		"extends": "plugin:ava/recommended"
	}
}
```

Consulter les [docs ESLint](https://eslint.org/docs/user-guide/configuring#extending-configuration-files) pour plus d'informations sur l'extension des fichier de config.

**Remarque** : Cette config activera aussi correctement les [options du parser](https://eslint.org/docs/user-guide/configuring#specifying-parser-options) et de l'[environnement](https://eslint.org/docs/user-guide/configuring#specifying-environments).
