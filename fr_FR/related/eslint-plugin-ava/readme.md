___
**Note du traducteur**

C'est la traduction du fichier [readme.md](https://github.com/avajs/eslint-plugin-ava/blob/master/readme.md). Voici un [lien](https://github.com/avajs/eslint-plugin-ava/compare/e196c7a2bd7ed2a52366ede4c1bc57505949cdd1...master#diff-0730bb7c2e8f9ea2438b52e419dd86c9) vers les différences avec le master de eslint-plugin-ava (Si en cliquant sur le lien, vous ne trouvez pas le fichier `readme.md` parmi les fichiers modifiés, vous pouvez donc en déduire que la traduction est à jour).
___
# eslint-plugin-ava [![Build Status](https://travis-ci.org/avajs/eslint-plugin-ava.svg?branch=master)](https://travis-ci.org/avajs/eslint-plugin-ava) [![Coverage Status](https://coveralls.io/repos/github/avajs/eslint-plugin-ava/badge.svg?branch=master)](https://coveralls.io/github/avajs/eslint-plugin-ava?branch=master)

> Règles ESLint pour [AVA](https://ava.li)

Traductions : [English](https://github.com/avajs/eslint-plugin-ava/blob/master/readme.md)

Ce plugin est intégré dans [XO](https://github.com/xojs/xo). Pas besoin de faire quoi que ce soit si vous utilisez XO.


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
			"ecmaVersion": 2017,
			"sourceType": "module"
		},
		"plugins": [
			"ava"
		],
		"rules": {
			"ava/assertion-arguments": "error",
			"ava/max-asserts": ["off", 5],
			"ava/no-async-fn-without-await": "error",
			"ava/no-cb-test": "off",
			"ava/no-duplicate-modifiers": "error",
			"ava/no-identical-title": "error",
			"ava/no-ignored-test-files": "error",
			"ava/no-invalid-end": "error",
			"ava/no-nested-tests": "error",
			"ava/no-only-test": "error",
			"ava/no-skip-assert": "error",
			"ava/no-skip-test": "error",
			"ava/no-statement-after-end": "error",
			"ava/no-todo-implementation": "error",
			"ava/no-todo-test": "warn",
			"ava/no-unknown-modifiers": "error",
			"ava/prefer-async-await": "error",
			"ava/prefer-power-assert": "off",
			"ava/test-ended": "error",
			"ava/test-title": ["error", "if-multiple"],
			"ava/use-t-well": "error",
			"ava/use-t": "error",
			"ava/use-test": "error",
			"ava/use-true-false": "error"
		}
	}
}
```


## Les règles

Les règles s'activeront uniquement dans les fichiers de test.

- [assertion-arguments](docs/rules/assertion-arguments.md) - Imposer le passage d'arguments corrects aux assertions.
- [max-asserts](docs/rules/max-asserts.md) - Limiter le nombre d'assertions dans un test.
- [no-async-fn-without-await](docs/rules/no-async-fn-without-await.md) - S'assurer que les tests async utilisent `await`.
- [no-cb-test](docs/rules/no-cb-test.md) - S'assurer qu'aucun `test.cb()` n'est utilisé.
- [no-duplicate-modifiers](docs/rules/no-duplicate-modifiers.md) - S'assurer qu'aucun test n'a de modificateur en doublon.
- [no-identical-title](docs/rules/no-identical-title.md) - S'assurer que chaque test a un titre différent.
- [no-ignored-test-files](docs/rules/no-ignored-test-files.md) - S'assurer qu'aucun test ne soit écrit dans des fichiers ignorés.
- [no-invalid-end](docs/rules/no-invalid-end.md) - S'assurer que `t.end()` est seulement appelé dans `test.cb()`.
- [no-nested-tests](docs/rules/no-nested-tests.md) - S'assurer qu'aucun test soit imbriqué.
- [no-only-test](docs/rules/no-only-test.md) - S'assurer qu'aucun `test.only()` soit présent.
- [no-skip-assert](docs/rules/no-skip-assert.md) - S'assurer qu'aucune assertion soit passée (skip).
- [no-skip-test](docs/rules/no-skip-test.md) - S'assurer qu'aucun test soit passé (skip).
- [no-statement-after-end](docs/rules/no-statement-after-end.md) - S'assurer que `t.end()` soit la dernière instruction exécutée.
- [no-todo-implementation](docs/rules/no-todo-implementation.md) - S'assurer que `test.todo()` n'a pas une fonction d'implémentation.
- [no-todo-test](docs/rules/no-todo-test.md) - S'assurer qu'aucun `test.todo()` soit utilisé.
- [no-unknown-modifiers](docs/rules/no-unknown-modifiers.md) - Empêcher l'utilisation de modificateurs de test inconnus.
- [prefer-async-await](docs/rules/prefer-async-await.md) - Préférer l'utilisation de async/await au lieu de retourner une Promesse.
- [prefer-power-assert](docs/rules/prefer-power-assert.md) - Autoriser uniquement l'utilisation des assertions qui n'ont pas d'alternatives dans [power-assert](https://github.com/power-assert-js/power-assert).
- [test-ended](docs/rules/test-ended.md) - S'assurer que les tests de callback soient explicitement terminés.
- [test-title](docs/rules/test-title.md) - S'assurer que les tests ont un titre.
- [use-t-well](docs/rules/use-t-well.md) - Empêcher une mauvaise utilisation de `t`.
- [use-t](docs/rules/use-t.md) - S'assurer que les fonctions de test utilisent `t` comme paramètre.
- [use-test](docs/rules/use-test.md) - S'assurer que AVA est importé avec la variable nommée `test`.
- [use-true-false](docs/rules/use-true-false.md) - S'assurer que `t.true()`/`t.false()` sont utilisés à la place de `t.truthy()`/`t.falsy()`.


## Config recommandée

Ce plugin exporte une [config recommandée](https://github.com/avajs/eslint-plugin-ava/blob/master/index.js) (`recommended`) qui applique les bonnes pratiques.

Activez la dans votre `package.json` avec l'option `extends` :

```json
{
	"name": "my-awesome-project",
	"eslintConfig": {
		"plugins": [
			"ava"
		],
		"extends": "plugin:ava/recommended"
	}
}
```

Consulter les [docs ESLint](http://eslint.org/docs/user-guide/configuring#extending-configuration-files) pour plus d'informations sur l'extension des fichier de config.

**Remarque** : Cette config activera aussi correctement les [options du parser](http://eslint.org/docs/user-guide/configuring#specifying-parser-options) et de l'[environnement](http://eslint.org/docs/user-guide/configuring#specifying-environments).


## Licence

MIT
