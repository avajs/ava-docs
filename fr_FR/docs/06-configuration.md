___
**Note du traducteur**

C'est la traduction du fichier [06-configuration.md](https://github.com/avajs/ava/blob/main/docs/06-configuration.md). Voici un [lien](https://github.com/avajs/ava/compare/b208d143ad852dc95aa8b44eed94ac1f404a25f4...main#diff-ca74b9b04caf1bea61ec4f62d5d8ddcd93b0cef710e781bd01629d71fc6aedc2) vers les différences avec le master de AVA (Si en cliquant sur le lien, vous ne trouvez pas le fichier `06-configuration.md` parmi les fichiers modifiés, vous pouvez donc en déduire que la traduction est à jour).
___
# Configuration

Traductions : [English](https://github.com/avajs/ava/blob/main/docs/06-configuration.md)

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
		],
		"nodeArguments": [
			"--trace-deprecation",
			"--napi-modules"
		]
	}
}
```

Les arguments passés au CLI seront toujours prioritaires sur les options du CLI configurées dans `package.json`.

## Options

- `files` : un tableau de glob patterns pour sélectionner les fichiers de test. Les fichiers préfixés avec un underscore sont ignorés. Par défaut, seuls les fichiers avec l'extensions `cjs`, `mjs` & `js` sont sélectionnés, même si le pattern correspond à d'autres fichiers. Spécifiez `extensions` pour accepter d'autres extensions de fichier
- `ignoredByWatcher` : un tableau glob patterns pour faire correspondre les fichiers qui, même modifiés, sont ignorés par l'observateur. Voir la [recette du mode watch pour plus de détails](https://github.com/avajs/ava-docs/blob/main/fr_FR/docs/recipes/watch-mode.md)
- `match` : n'est généralement pas utile dans la configuration du `package.json`, mais est équivalent au [`--match` de la CLI](./05-command-line.md#exécution-de-tests-correspondants-à-des-titres)
- `cache` : par défaut à `true` pour mettre en cache les fichiers compilés sous `node_modules/.cache/ava`. Si à `false`, les fichiers sont mis en cache à la place dans un répertoire temporaire
- `concurrency` : nombre maximum de fichiers de test en cours d'exécution en même temps (par défaut : le nombre de cœurs du processeur)
- `workerThreads` : utilise des processus de travail pour exécuter des tests (activé par défaut). Si à `false`, les tests s'exécuteront dans les processus enfants
- `failFast` : arrête d'exécuter d'autres tests dès qu'un test échoue
- `failWithoutAssertions` : si `false`, ne pas faire échouer un test s'il n'exécute pas des [assertions](./03-assertions.md)
- `environmentVariables` : spécifie les variables d'environnement à mettre à la disposition des tests. Les variables d'environnement définies ici remplacent celles de `process.env`
- `tap` : si `true`, active le [reporter de TAP](./05-command-line.md#reporter-de-tap)
- `verbose` : si `true`, active la sortie verbeuse (bien qu'actuellement la sortie non-verbeuse ne soit pas prise en charge)
- `snapshotDir` : indique l'endroit fixe pour le stockage des fichiers instantanés. Utilisez ceci si vos instantanés se positionnent à un mauvais endroit
- `extensions` : les extensions de fichiers de test. La définition de ce paramètre remplace la valeur par défaut `["cjs", "mjs", "js"]`, alors assurez-vous d'inclure ces extensions dans la liste. [Expérimentalement vous pouvez configurer comment les fichiers sont chargés](#configuration-des-formats-de-modules)
- `require` : modules supplémentaires à intégrer avant que les tests ne soient exécutés. Les modules qui sont requis dans le [processus de travail](./01-writing-tests.md#isolement-du-processus)
- `timeout` : les timeouts dans AVA se comportent différemment de ceux des autres frameworks de test. AVA réinitialise une minuterie après chaque test, forçant l'arrêt des tests si aucun nouveau résultat de test n'a été reçu dans le délai imparti. Ceci peut être utilisé pour gérer des tests bloqués. Consultez notre [documentation sur le timeout](./07-test-timeouts.md) pour plus d'options.
- `nodeArguments` : configure les arguments Node.js utilisés pour lancer des processus de travail.
- `sortTestFiles` : une fonction de comparaison pour trier les fichiers de test. Disponible uniquement lorsque vous utilisez un fichier `ava.config.*`. Consultez un exemple de cas d'utilisation [ici](recipes/splitting-tests-ci.md).
- `utilizeParallelBuilds` : Si à `false`, désactive [construction en parallèle](/docs/recipes/splitting-tests-ci.md) (par défaut : true)

Veuillez notez qu'en fournissant des fichiers à la CLI, cela écrase l'option `files`.

Fournissez l'option `typescript` (et installez [`@ava/typescript`](https://github.com/avajs/typescript) comme dépendance supplémentaire) pour AVA afin d'exécuter les tests écrits en TypeScript.

## Utilisation des fichiers `ava.config.*`

Plutôt que de spécifier la configuration dans le fichier `package.json`, vous pouvez utiliser les fichiers `ava.config.js`, `ava.config.cjs` ou `ava.config.mjs`.

Pour utiliser ces fichiers :

1. Votre `package.json` ne doit pas contenir la propriété `ava` (ou si c'est le cas, il doit s'agir d'un objet vide)
2. Vous ne devez avoir qu'un seul fichier `ava.config.*` dans un répertoire, donc ne mélangez pas les fichiers `ava.config.js` *et* `ava.config.cjs`.

AVA recherche dans votre système de fichiers les fichiers `ava.config.*`. D'abord, quand vous lancez AVA, il trouve le `package.json` le plus proche. En partant de ce répertoire, il vérifie récursivement les répertoires parents jusqu'à ce qu'il atteigne la racine du système de fichiers ou qu'il rencontre un fichier ou un répertoire `.git`. Le premier fichier `ava.config.*` trouvé est sélectionné. Cela vous permet d'utiliser un seul fichier de configuration dans une installation monorepo.

### `ava.config.js`

AVA suit le comportement de Node.js, donc si vous avez défini `"type": "module"` vous devez utiliser ESM, et sinon vous devez utiliser CommonJS.

L'export par défaut peut être soit un objet simple, soit une fonction factory qui renvoie un objet simple. Vous pouvez exporter ou renvoyer une promesse pour un objet simple :

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

### `ava.config.cjs`

Pour les fichiers `ava.config.cjs`, vous devez affecter `module.exports`. La ["portée du module"](https://nodejs.org/docs/latest-v12.x/api/modules.html#modules_the_module_scope) est disponible. Vous pouvez faire un `require()` pour les dépendances.

Le module export peut être soit un objet simple, soit une fonction factory qui renvoie un objet simple. Vous pouvez exporter ou renvoyer une promesse pour un objet simple :

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

### `ava.config.mjs`

L'export default peut être soit un objet simple, soit une fonction factory qui renvoie un objet simple. Vous pouvez exporter ou renvoyer une promesse pour un objet simple :

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

La fonction factory est appelée avec un objet contenant une propriété `projectDir`, que vous pouvez utiliser pour changer la configuration retournée :

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

## Fichiers de configuration alternatifs

La [CLI] vous permet d'indiquer un fichier de configuration spécifique, en utilisant le flag `--config`. Ce fichier doit avoir une extension `.js`, `.cjs` ou `.mjs` et est traité comme le serait un fichier `ava.config.js`, `ava.config.cjs` ou `ava.config.mjs`.

Lorsque le flag `--config` est défini, le fichier fourni remplacera toute la configuration des fichiers `package.json` et `ava.config.js`, `ava.config.cjs` ou `ava.config.mjs`. La configuration n'est pas fusionnée.

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
import util from 'util';

import test from 'ava';

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

## Configuration des formats de modules

Node.js peut uniquement charger les extensions non standard en tant que modules ES lors de l'utilisation de [chargeurs expérimentaux.](https://nodejs.org/docs/latest/api/esm.html#esm_experimental_loaders). Pour l'utiliser, vous devrez également configurer AVA pour qu'il `import()` votre fichier de test.

Comme pour la forme tableau, vous devez explicitement lister les extensions `js`, `cjs`, et `mjs`. Celles-ci **doivent** être définies en utilisant la valeur `true`, les autres extensions sont configurables en utilisant soit `'commonjs'` soit `'module'` :

`ava.config.js` :
```js
export default {
	extensions: {
		js: true,
		ts: 'module'
	}
};
```

## Arguments Node

La configuration `nodeArguments` peut être utilisée pour spécifier des arguments supplémentaires pour le lancement des processus de travail. Ceux-ci sont combinés avec les `--node-arguments` passés sur l'interface CLI et tous les arguments passés au binaire `node` lors du démarrage d'AVA.

[CLI]: ./05-command-line.md
