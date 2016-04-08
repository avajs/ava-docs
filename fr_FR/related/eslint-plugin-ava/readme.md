___
**Note du traducteur**

C'est la traduction du fichier [readme.md](https://github.com/sindresorhus/eslint-plugin-ava/blob/master/readme.md). Voici un [lien](https://github.com/sindresorhus/eslint-plugin-ava/compare/625080ddb948f849fa7b6c8acd623155e111e2b2...master#diff-0730bb7c2e8f9ea2438b52e419dd86c9) vers les différences avec le master de AVA (Si en cliquant sur le lien, vous ne trouvez pas le fichier `readme.md` parmi les fichiers modifiés, vous pouvez donc en déduire que la traduction est à jour).
___
# eslint-plugin-ava [![Build Status](https://travis-ci.org/sindresorhus/eslint-plugin-ava.svg?branch=master)](https://travis-ci.org/sindresorhus/eslint-plugin-ava)

> Règles ESLint pour [AVA](https://ava.li)


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
			"ecmaVersion": 6,
			"sourceType": "module"
		},
		"plugins": [
			"ava"
		],
		"rules": {
			"ava/max-asserts": ["off", 5],
			"ava/no-cb-test": "off",
			"ava/no-identical-title": "error",
			"ava/no-ignored-test-files": "error",
			"ava/no-invalid-end": "error",
			"ava/no-only-test": "error",
			"ava/no-skip-assert": "error",
			"ava/no-skip-test": "error",
			"ava/no-statement-after-end": "error",
			"ava/no-todo-test": "warn",
			"ava/no-unknown-modifiers": "error",
			"ava/prefer-power-assert": "off",
			"ava/test-ended": "error",
			"ava/test-title": ["error", "if-multiple"],
			"ava/use-t": "error",
			"ava/use-test": "error"
		}
	}
}
```


## Les règles

Les règles s'activeront uniquement dans les fichiers de test.

- [max-asserts](docs/rules/max-asserts.md) - Limiter le nombre d'assertions dans un test.
- [no-cb-test](docs/rules/no-cb-test.md) - S'assurer qu'aucun `test.cb()` n'est utilisé.
- [no-identical-title](docs/rules/no-identical-title.md) - S'assurer que chaque test a un titre différent.
- [no-ignored-test-files](docs/rules/no-ignored-test-files.md) - S'assurer qu'aucun test ne soit écrit dans des fichiers ignorés.
- [no-invalid-end](docs/rules/no-invalid-end.md) - S'assurer que `t.end()` est seulement appelé dans `test.cb()`.
- [no-only-test](docs/rules/no-only-test.md) - S'assurer qu'aucun `test.only()` soit présent.
- [no-skip-assert](docs/rules/no-skip-assert.md) - S'assurer qu'aucune assertion soit passée (skip).
- [no-skip-test](docs/rules/no-skip-test.md) - S'assurer qu'aucun test soit passé (skip).
- [no-statement-after-end](docs/rules/no-statement-after-end.md) - S'assurer que `t.end()` soit la dernière instruction exécutée.
- [no-todo-test](docs/rules/no-todo-test.md) - S'assurer qu'aucun `test.todo()` soit utilisé.
- [no-unknown-modifiers](docs/rules/no-unknown-modifiers.md) - Empêcher l'utilisation de modificateurs de test inconnus.
- [prefer-power-assert](docs/rules/prefer-power-assert.md) - Autoriser uniquement l'utilisation des assertions qui n'ont pas d'alternatives dans [power-assert](https://github.com/power-assert-js/power-assert).
- [test-ended](docs/rules/test-ended.md) - S'assurer que les tests de callback soient explicitement terminés.
- [test-title](docs/rules/test-title.md) - S'assurer que les tests ont un titre.
- [use-t](docs/rules/use-t.md) - S'assurer que les fonctions de test utilisent `t` comme paramètre.
- [use-test](docs/rules/use-test.md) - S'assurer que AVA est importé avec la variable nommée `test`.


## Configuration recommandée

Ce plugin exporte une [configuration recommandée](index.js#L9) (`recommended`) qui applique les bonnes pratiques.

Pour activer cette configuration, utilisez la propriété `extends` dans votre `package.json`.

```json
{
	"name": "my-awesome-project",
	"eslintConfig": {
		"extends": "plugin:ava/recommended",
		"plugins": [
			"ava"
		]
	}
}
```

Consulter la [documentation ESLint](http://eslint.org/docs/user-guide/configuring#extending-configuration-files) pour plus d'informations sur l'extension des fichier de configuration.

**Remarque** : Cette configuration activera aussi correctement les [options du parser](http://eslint.org/docs/user-guide/configuring#specifying-parser-options) et de l'[environment](http://eslint.org/docs/user-guide/configuring#specifying-environments).


## Auteurs

- [Jeroen Engels](https://github.com/jfmengels)
- [Takuto Wada](https://github.com/twada)
- [Et nos supers contributeurs](https://github.com/sindresorhus/eslint-plugin-ava/graphs/contributors)


## Licence

MIT © [Sindre Sorhus](http://sindresorhus.com)
