___
**Note du traducteur**

C'est la traduction du fichier [06-configuration.md](https://github.com/avajs/ava/blob/master/docs/06-configuration.md). Voici un [lien](https://github.com/avajs/ava/compare/1e3b072ab7aee9ce78d8faa774287902792fab90...master#diff-e314afbd72d4daaedf4d543da317ad58) vers les différences avec le master de AVA (Si en cliquant sur le lien, vous ne trouvez pas le fichier `06-configuration.md` parmi les fichiers modifiés, vous pouvez donc en déduire que la traduction est à jour).
___
# Configuration

Traductions : [English](https://github.com/avajs/ava/blob/master/docs/06-configuration.md)

Toutes les [options du CLI](./05-command-line.md) peuvent être configurés dans la section `ava` de votre `package.json` ou dans un fichier `ava.config.js`. Cela vous permet de modifier le comportement par défaut de la commande `ava`, ainsi vous n'avez plus besoin à chaque fois de taper les mêmes options sur l'invite de commande.

Pour ignorer un fichier ou un répertoire, préfixer le chemin avec un `!` (point d'exclamation).

**`package.json`:**

```json
{
	"ava": {
		"files": [
			"my-test-directory/**/*.js",
			"!my-test-directory/exclude-this-directory/**/*.js",
			"!**/exclude-this-file.js"
		],
		"sources": [
			"**/*.{js,jsx}",
			"!dist/**/*"
		],
		"match": [
			"*oo",
			"!foo"
		],
		"cache": true,
		"concurrency": 5,
		"failFast": true,
		"failWithoutAssertions": false,
		"tap": true,
		"verbose": true,
		"compileEnhancements": false,
		"require": [
			"@babel/register"
		],
		"babel": {
			"extensions": ["jsx"],
			"testOptions": {
				"babelrc": false
			}
		}
	}
}
```

Les arguments passés au CLI seront toujours prioritaires sur les options du CLI configurées dans `package.json`.

## Options

- `files` : les chemins des fichiers et des répertoires, ainsi que les modèles de glob (glob patterns) qui sélectionnent les fichiers AVA qui feront des tests. Les fichiers préfixés avec un underscore sont ignorés. Tous les fichiers correspondants dans les répertoires sélectionnés sont exécutés. Par défaut, cela sélectionne uniquement les fichiers avec les extensions `js`, même si le modèle de glob correspond à d'autres fichiers. Spécifier `extensions` et `babel.extensions` pour autoriser d'autres extensions de fichiers
- `sources` : les fichiers, lorsqu'ils sont modifiés, provoquent la ré-exécution des tests lors du mode watch. Voir la [recette du mode watch pour plus de détails](https://github.com/avajs/ava/blob/master/docs/recipes/watch-mode.md#les-fichiers-sources-et-les-fichiers-de-test)
- `match` : n'est généralement pas utile dans la configuration du `package.json`, mais est équivalent au [`--match` de la CLI](./05-command-line.md#exécution-de-tests-correspondants-à-des-titres)
- `cache` : met en cache les fichiers de test compilé et les helpers sous `node_modules/.cache/ava`. Si la valeur est à `false`, les fichiers sont à la place mis en cache dans un répertoire temporaire
- `failFast` : arrête d'exécuter d'autres tests dès qu'un test échoue
- `failWithoutAssertions` : si `false`, ne pas faire échouer un test s'il n'exécute pas des [assertions](./03-assertions.md)
- `tap` : si `true`, active le [reporter de TAP](./05-command-line.md#reporter-de-tap)
- `verbose`: si `true`, active la sortie verbeuse
- `snapshotDir` : indique l'endroit fixe pour le stockage des fichiers instantanés. Utilisez ceci si vos instantanés se positionnent à un mauvais endroit
- `compileEnhancements` : si `false`, désactive [power-assert](https://github.com/power-assert-js/power-assert) qui aide tout de même à fournir des messages d'erreur plus descriptifs — et la détection d'une mauvaise utilisation de l'assertion `t.throws()`
- `extensions` : les extensions de fichiers de test qui seront précompilées à l'aide des presets Babel de AVA. Notez que les fichiers sont toujours compilés pour activer power-assert et d'autres fonctionnalités, donc vous devrez peut-être aussi définir `compileEnhancements` à `false` si vos fichiers ne sont pas valides JavaScript. La définition de ce paramètre remplace la valeur par défaut `"js"`, alors assurez-vous d'inclure cette extension dans la liste, si celle-ci n'est pas incluse dans `babel.extensions`
- `require` : modules supplémentaires à intégrer avant que les tests ne soient exécutés. Les modules qui sont requis dans le [processus de travail](./01-writing-tests.md#isolement-du-processus)
- `babel` : options spécifiques de Babel pour les fichiers de test. Consultez notre [recette Babel](./recipes/babel.md#configuration-de-babel) pour plus de détails
- `babel.extensions` : les extensions de fichiers de test qui seront précompilées à l'aide des presets Babel de AVA. Ce réglage remplace la valeur par défaut `"js"`, alors assurez-vous d'inclure cette extension dans la liste
- `timeout` : Les timeouts dans AVA se comportent différemment de ceux des autres frameworks de test. AVA réinitialise une minuterie après chaque test, forçant l'arrêt des tests si aucun nouveau résultat de test n'a été reçu dans le délai imparti. Ceci peut être utilisé pour gérer des tests bloqués. Consultez notre [documentation sur le timeout](./07-test-timeouts.md) pour plus d'options.

Veuillez notez qu'en fournissant des fichiers à la CLI, cela écrase l'option `files`. Si vous avez configuré un glob pattern, par exemple `test/**/*.test.js`, vous devez peut-être le répéter lors de l'utilisation de la CLI : `ava 'test/integration/*.test.js'`.

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
