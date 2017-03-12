___
**Note du traducteur**

C'est la traduction du fichier [precompiling-with-webpack.md](https://github.com/avajs/ava/blob/master/docs/recipes/jspm-systemjs.md). Voici un [lien](https://github.com/avajs/ava/compare/1ad347c53c4618bca7c9a01e637441aeb5db5168...master#diff-b4c25a4a1412e9204ac923133f67814f) vers les différences avec le master de AVA (Si en cliquant sur le lien, vous ne trouvez pas le fichier `precompiling-with-webpack.md` parmi les fichiers modifiés, vous pouvez donc en déduire que la traduction est à jour).
___
## Précompilation des fichiers sources avec webpack

Traductions : [English](https://github.com/avajs/ava/blob/master/docs/recipes/precompiling-with-webpack.md)

Le [readme](https://github.com/avajs/ava#transpiling-imported-modules) de AVA mentionne la précompilation de vos modules importés comme une alternative à la compilation lors de l'exécution, mais il ne l'explique pas. Cette recette montre comment faire en utilisant webpack. (Cet exemple utilise le webpack 2.0)

###### webpack.config.js

```js
const nodeExternals = require('webpack-node-externals');

module.exports = {
	entry: ['src/tests.js'],
	target: 'node',
	output: {
		path: '_build',
		filename: 'tests.js'
	},
	externals: nodeExternals,
	module: {
		rules: [{
				test: /\.(js|jsx)$/,
				use: 'babel-loader'
		}]
	}
};
```

Les points importants sont `target: 'node'`, qui ne tient pas compte des `require`s spécifiques de Node.js (par exemple `fs`, `path`, etc...) et `externals: nodeModules` qui empêche webpack d'empaqueter les modules externes de Node.js qui pourraient être gênants.

Vous pouvez maintenant exécuter `$ ava _build/tests.js` pour lancer les tests contenus dans ce fichier généré.
