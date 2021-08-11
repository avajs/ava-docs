___
**Note du traducteur**

C'est la traduction du fichier [06-configuration.md](https://github.com/avajs/ava/blob/main/docs/06-configuration.md). Voici un [lien](https://github.com/avajs/ava/compare/8fa28254dbebef32cbde05c0c9a49061d0ef82f8...main#diff-e314afbd72d4daaedf4d543da317ad58) vers les différences avec le master de AVA (Si en cliquant sur le lien, vous ne trouvez pas le fichier `06-configuration.md` parmi les fichiers modifiés, vous pouvez donc en déduire que la traduction est à jour).
___
# Configuration

Traductions : [English](https://github.com/avajs/ava/raw/main/docs/06-configuration.md)

**Ceci documente la prochaine version de AVA 3. Consultez plutôt la documentation [AVA 2](https://github.com/avajs/ava-docs/blob/2c3ceef77e84f23025753adb14ebf948946a7438/fr_FR/docs/06-configuration.md).**

Toutes les [options du CLI][CLI] peuvent être configurés dans la section `ava` de votre `package.json` ou dans un fichier `ava.config.*`. Cela vous permet de modifier le comportement par défaut de la commande `ava`, ainsi vous n'avez plus besoin à chaque fois de taper les mêmes options sur l'invite de commande.

Pour ignorer les fichiers, préfixer le pattern avec un `!` (point d'exclamation).

**`package.json`:**

```json
{
	"ava": {
		"files": [
			"test/**/*",
			"!test/exclude-files-in-this-directory",
			"!**/exclude-files-with-this-name.*"
		],
		"match": [
			"*oo",
			"!foo"
		],
		"concurrency": 5,
		"failFast": true,
		"failWithoutAssertions": false,
		"environmentVariables": {
			"MY_ENVIRONMENT_VARIABLE": "some value"
		},
		"verbose": true,
		"require": [
			"./my-helper-module.js"
		]
	}
}
```

Les arguments passés au CLI seront toujours prioritaires sur les options du CLI configurées dans `package.json`.

## Options

- `files` : un tableau de glob patterns pour sélectionner les fichiers de test. Les fichiers préfixés avec un underscore sont ignorés. Par défaut, seuls les fichiers avec l'extensions `cjs`, `mjs` & `js` sont sélectionnés, même si le pattern correspond à d'autres fichiers. Spécifiez `extensions` pour accepter d'autres extensions de fichier
- `ignoredByWatcher` : un tableau glob patterns pour faire correspondre les fichiers qui, même modifiés, sont ignorés par l'observateur. Voir la [recette du mode watch pour plus de détails](https://github.com/avajs/ava-docs/blob/main/fr_FR/docs/recipes/watch-mode.md)
- `match` : n'est généralement pas utile dans la configuration du `package.json`, mais est équivalent au [`--match` de la CLI](./05-command-line.md#exécution-de-tests-correspondants-à-des-titres)
- `cache` : met en cache les fichiers compilé sous `node_modules/.cache/ava`. Si la valeur est à `false`, les fichiers sont à la place mis en cache dans un répertoire temporaire
- `failFast` : arrête d'exécuter d'autres tests dès qu'un test échoue
- `failWithoutAssertions` : si `false`, ne pas faire échouer un test s'il n'exécute pas des [assertions](./03-assertions.md)
- `environmentVariables` : spécifie les variables d'environnement à mettre à la disposition des tests. Les variables d'environnement définies ici remplacent celles de `process.env`
- `tap` : si `true`, active le [reporter de TAP](./05-command-line.md#reporter-de-tap)
- `verbose` : si `true`, active la sortie verbeuse
- `snapshotDir` : indique l'endroit fixe pour le stockage des fichiers instantanés. Utilisez ceci si vos instantanés se positionnent à un mauvais endroit
- `extensions` : les extensions de fichiers de test. La définition de ce paramètre remplace la valeur par défaut `["cjs", "mjs", "js"]`, alors assurez-vous d'inclure ces extensions dans la liste
- `require` : modules supplémentaires à intégrer avant que les tests ne soient exécutés. Les modules qui sont requis dans le [processus de travail](./01-writing-tests.md#isolement-du-processus)
- `timeout` : Les timeouts dans AVA se comportent différemment de ceux des autres frameworks de test. AVA réinitialise une minuterie après chaque test, forçant l'arrêt des tests si aucun nouveau résultat de test n'a été reçu dans le délai imparti. Ceci peut être utilisé pour gérer des tests bloqués. Consultez notre [documentation sur le timeout](./07-test-timeouts.md) pour plus d'options.

Veuillez notez qu'en fournissant des fichiers à la CLI, cela écrase l'option `files`.

Fournissez l'option `babel` (et installez [`@ava/babel`](https://github.com/avajs/babel) comme dépendance supplémentaire) pour activer la compilation Babel.

## Utilisation des fichiers `ava.config.*`

Plutôt que de spécifier la configuration dans le fichier `package.json`, vous pouvez utiliser les fichiers `ava.config.js` ou `ava.config.cjs`.

Pour utiliser ces fichiers :

1. Ils doivent être dans le même répertoire que votre `package.json`
2. Votre `package.json` ne doit pas contenir la propriété `ava` (ou si c'est le cas, il doit s'agir d'un objet vide)
3. Vous ne devez pas avoir à la fois un fichier `ava.config.js` *et* un fichier `ava.config.cjs`

AVA reconnaît les fichiers `ava.config.mjs` mais refuse de les charger.

### `ava.config.js`

Pour les fichiers `ava.config.js`, vous devez utiliser `export default`. Vous ne pouvez pas utiliser la ["portée du module"](https://nodejs.org/docs/latest-v12.x/api/modules.html#modules_the_module_scope). Vous ne pouvez pas importer de dépendances.

L'export par défaut peut être soit un objet simple, soit une fonction factory qui renvoie un objet simple :

```js
export default {
	require: ['./_my-test-helper']
};
```

```js
export default function factory() {
	return {
		require: ['./_my-test-helper']
	};
};
```

La fonction factory est appelée avec un objet contenant une propriété `projectDir`, que vous pourrez l'utiliser pour changer la configuration retournée :

```js
export default ({projectDir}) => {
	if (projectDir === '/Users/username/projects/my-project') {
		return {
			// Config A
		};
	}

	return {
		// Config B
	};
};
```

Notez que la config finale ne doit pas être une promesse.

### `ava.config.cjs`

Pour les fichiers `ava.config.cjs`, vous devez affecter `module.exports`. La ["portée du module"](https://nodejs.org/docs/latest-v12.x/api/modules.html#modules_the_module_scope) est disponible. Vous pouvez faire un `require()` pour les dépendances.

Le module export peut être soit un objet simple, soit une fonction factory qui renvoie un objet simple :

```js
module.exports = {
	require: ['./_my-test-helper']
};
```

```js
module.exports = () => {
	return {
		require: ['./_my-test-helper']
	};
};
```

La fonction factory est appelée avec un objet contenant une propriété `projectDir`, que vous pouvez utiliser pour changer la configuration retournée :

```js
module.exports = ({projectDir}) => {
	if (projectDir === '/Users/username/projects/my-project') {
		return {
			// Config A
		};
	}

	return {
		// Config B
	};
};
```

Remarquez que la configuration finale ne doit pas être une promesse.

## Fichiers de configuration alternatifs

La [CLI] vous permet d'indiquer un fichier de configuration spécifique, en utilisant le flag `--config`. Ce fichier doit avoir une extension `.js` ou `.cjs` et est traité comme le serait un fichier `ava.config.js` ou `ava.config.cjs`.

Lorsque le flag `--config` est défini, le fichier fourni remplacera toute la configuration des fichiers `package.json` et `ava.config.js` ou `ava.config.cjs`. La configuration n'est pas fusionnée.

Le fichier de configuration *doit* être dans le même répertoire que le fichier `package.json`.

Vous pouvez l'utiliser pour personnaliser la configuration d'un test spécifique. Par exemple, vous pouvez exécuter des tests unitaires séparément des tests d'intégration :

`ava.config.cjs`:

```js
module.exports = {
	files: ['unit-tests/**/*']
};
```

`integration-tests.config.cjs`:

```js
const baseConfig = require('./ava.config.cjs');

module.exports = {
	...baseConfig,
	files: ['integration-tests/**/*']
};
```

Vous pouvez maintenant exécuter vos tests unitaires via `npx ava` et les tests d'intégration via `npx ava --config integration-tests.config.cjs`.

## Profondeur d'affichage d'un objet

Par défaut, AVA affiche les objets imbriqués avec une profondeur de `3`. Toutefois, lors du débogage de tests avec des objets imbriqués plus profondément, il peut être utile d’afficher plus de détails. Cela peut être fait en définissant [`util.inspect.defaultOptions.depth`](https://nodejs.org/api/util.html#util_util_inspect_defaultoptions) avec la profondeur souhaitée, avant l'exécution du test :

```js
const util = require('util');

const test = require('ava');

util.inspect.defaultOptions.depth = 5;  // Augmente la profondeur d'affichade de AVA

test('Mon test', t => {
	t.deepEqual(someDeeplyNestedObject, theExpectedValue);
});
```

AVA a une profondeur minimale de `3`.

## Expérimentations

De temps en temps, AVA implémentera des fonctionnalités expérimentales. Celles-ci peuvent changer ou être supprimées à tout moment, pas seulement lorsqu'une nouvelle version majeure est disponible. Vous pouvez choisir une telle fonctionnalité en l'activant dans la configuration `nonSemVerExperiments`.

`ava.config.js` :
```js
export default {
	nonSemVerExperiments: {
		feature: true
	}
};
```

Vous pouvez choisir la nouvelle assertion `t.try ()` en spécifiant `tryAssertion` :

`ava.config.js` :
```js
export default {
	nonSemVerExperiments: {
		tryAssertion: true
	}
};
```

[CLI]: ./05-command-line.md
