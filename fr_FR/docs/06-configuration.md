___
**Note du traducteur**

C'est la traduction du fichier [06-configuration.md](https://github.com/avajs/ava/blob/master/docs/06-configuration.md). Voici un [lien](https://github.com/avajs/ava/compare/b4ea43529a6d058a96055735cfa6e7056c009112...master#diff-e314afbd72d4daaedf4d543da317ad58) vers les différences avec le master de AVA (Si en cliquant sur le lien, vous ne trouvez pas le fichier `06-configuration.md` parmi les fichiers modifiés, vous pouvez donc en déduire que la traduction est à jour).
___
# Configuration

Traductions : [English](https://github.com/avajs/ava/blob/master/docs/06-configuration.md)

Toutes les [options du CLI][CLI] peuvent être configurés dans la section `ava` de votre `package.json` ou dans un fichier `ava.config.js`. Cela vous permet de modifier le comportement par défaut de la commande `ava`, ainsi vous n'avez plus besoin à chaque fois de taper les mêmes options sur l'invite de commande.

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
		"helpers": [
			"**/helpers/**/*"
		],
		"sources": [
			"src/**/*"
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

- `files`: un tableau de glob patterns pour sélectionner les fichiers de test. Les fichiers préfixés avec un underscore sont ignorés. Par défaut, seuls les fichiers avec l'extensions `js` sont sélectionnés, même si le pattern correspond à d'autres fichiers. Spécifiez `extensions` pour accepter d'autres extensions de fichier
- `helpers`: un tableau de glob patterns pour sélectionner les fichiers de helper. Les fichiers sélectionnés ne sseront jamais considérés comme des tests. Par défaut, seuls les fichiers avec l'extensions `js` sont sélectionnés, même si le pattern correspond à d'autres fichiers. Spécifiez `extensions` pour accepter d'autres extensions de fichier
- `sources`: un tableau de glob patterns pour sélectionner les fichiers, lorsqu'ils sont modifiés, provoquent la ré-exécution des tests lors du mode watch. Voir la [recette du mode watch pour plus de détails](https://github.com/avajs/ava/blob/master/docs/recipes/watch-mode.md#les-fichiers-sources-et-les-fichiers-de-test)
- `match` : n'est généralement pas utile dans la configuration du `package.json`, mais est équivalent au [`--match` de la CLI](./05-command-line.md#exécution-de-tests-correspondants-à-des-titres)
- `cache` : met en cache les fichiers de test compilé et les helpers sous `node_modules/.cache/ava`. Si la valeur est à `false`, les fichiers sont à la place mis en cache dans un répertoire temporaire
- `failFast` : arrête d'exécuter d'autres tests dès qu'un test échoue
- `failWithoutAssertions` : si `false`, ne pas faire échouer un test s'il n'exécute pas des [assertions](./03-assertions.md)
- `environmentVariables`: spécifie les variables d'environnement à mettre à la disposition des tests. Les variables d'environnement définies ici remplacent celles de `process.env`
- `tap` : si `true`, active le [reporter de TAP](./05-command-line.md#reporter-de-tap)
- `verbose`: si `true`, active la sortie verbeuse
- `snapshotDir` : indique l'endroit fixe pour le stockage des fichiers instantanés. Utilisez ceci si vos instantanés se positionnent à un mauvais endroit
- `extensions` : les extensions de fichiers de test. La définition de ce paramètre remplace la valeur par défaut `"js"`, alors assurez-vous d'inclure cette extension dans la liste
- `require` : modules supplémentaires à intégrer avant que les tests ne soient exécutés. Les modules qui sont requis dans le [processus de travail](./01-writing-tests.md#isolement-du-processus)
- `timeout` : Les timeouts dans AVA se comportent différemment de ceux des autres frameworks de test. AVA réinitialise une minuterie après chaque test, forçant l'arrêt des tests si aucun nouveau résultat de test n'a été reçu dans le délai imparti. Ceci peut être utilisé pour gérer des tests bloqués. Consultez notre [documentation sur le timeout](./07-test-timeouts.md) pour plus d'options.

Veuillez notez qu'en fournissant des fichiers à la CLI, cela écrase l'option `files`.

Fournissez l'option `babel` (et installez [`@ava/babel`](https://github.com/avajs/babel) comme dépendance supplémentaire) pour activer la compilation Babel.

## Utilisation de `ava.config.js`

Pour utiliser un fichier `ava.config.js` :

1. Il doit être dans le même répertoire que votre `package.json`
2. Votre `package.json` ne doit pas contenir la propriété `ava` (ou si c'est le cas, il doit s'agir d'un objet vide)
3. Vous devez utiliser `export default`, bien que [`require()`](https://nodejs.org/api/modules.html#modules_require_id) soit disponible pour charger les modules non-ES

Le fichier config doit avoir un export default, en utilisant des modules ES. Il peut s'agir d'un objet simple ou d'une fonction factory qui renvoie un objet brut :

```js
export default {
	require: ['esm']
};
```

```js
export default function factory() {
	return {
		require: ['esm']
	};
};
```

La fonction factory est appelée avec un objet contenant une propriété `projectDir`, vous pourrez l'utiliser pour changer la configuration retournée :

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

## Fichiers de configuration alternatifs

La [CLI] vous permet d'indiquer un fichier de configuration spécifique, en utilisant le flag `--config`. Ce fichier est traité comme un fichier `ava.config.js`. Lorsque le flag `--config` est défini, le fichier fourni remplacera toute la configuration des fichiers `package.json` et `ava.config.js`. La configuration n'est pas fusionnée.

Le fichier de configuration *doit* être dans le même répertoire que le fichier `package.json`.

Vous pouvez l'utiliser pour personnaliser la configuration d'un test spécifique. Par exemple, vous pouvez exécuter des tests unitaires séparément des tests d'intégration :

`ava.config.js`:

```js
export default {
	files: ['unit-tests/**/*']
};
```

`integration-tests.config.js`:

```js
import baseConfig from './ava.config.js';

export default {
	...baseConfig,
	files: ['integration-tests/**/*']
};
```

Vous pouvez maintenant exécuter vos tests unitaires via `npx ava` et les tests d'intégration via `npx ava --config integration-tests.config.js`.

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
