___
**Note du traducteur**

C'est la traduction du fichier [jspm-systemjs.md](https://github.com/avajs/ava/blob/master/docs/recipes/jspm-systemjs.md). Voici un [lien](https://github.com/avajs/ava/compare/b33cb1d533293c62f1244fb426e59d98de7890bc...master#diff-e1128fe3361ff27e14e2ebec6c6ff362) vers les différences avec le master de AVA (Si en cliquant sur le lien, vous ne trouvez pas le fichier `jspm-systemjs.md` parmi les fichiers modifiés, vous pouvez donc en déduire que la traduction est à jour).
___

# JSPM et SystemJS pour ES2015

Traductions : [English](https://github.com/avajs/ava/blob/master/docs/recipes/jspm-systemjs.md)

Cela nécessite un helper de chargement spécial pour résoudre correctement l'`import` des packages de JSPM lors de l'utilisation AVA. Le but du chargeur est de vous permettre d'exécuter vos tests sans avoir à pré-construire votre projet JSPM.

## Installation

Cette recette a été testée uniquement avec JSPM v0.17.0-beta.22, mais ceci devrait fonctionner avec toutes les versions de JSPM v0.17 et devrait fonctionner avec la version v0.16.

### Babel

Configurez votre .babelrc pour travailler avec AVA, si ce n'est pas déjà fait. REMARQUE : Vous pouvez garder une configuration supplémentaire dans vos fichiers de configuration de JSPM pour remplacer ces paramètres pendant le regroupement et la construction. À partir de babel 7, @babel/ preset-env transformera le code comme babel-preset-esXXXX le faisait précédemment.

```
$ npm install --save-dev @babel/preset-env
```

```json
{
	"presets": ["@babel/preset-env"]
}
```

Vous pouvez trouver plus d'informations sur la mise en place de Babel avec AVA dans la [recette babelrc](babelrc.md).

### Helper de chargement de JSPM

Vous aurez besoin d'installer le [chargeur JSPM de AVA](https://github.com/skorlir/ava-jspm-loader) comme une dépendance de dev.

```
$ npm install --save-dev ava-jspm-loader
```

Vous devrez également mettre à jour votre config de AVA dans le package.json pour utiliser le chargeur JSPM. A partir de Babel 7, vous devez installer[@babel-register](https://www.npmjs.com/package/@babel/register).

```
$ npm install @babel/register --save-dev
```
```json
{
	"ava": {
		"require": [
			"@babel/register",
			"ava-jspm-loader"
		]
	}
}
```

REMARQUE : Si vous utilisez async/await dans votre code source (pas dans votre code de test), vous aurez besoin d'installer [@babel/polyfill](https://www.npmjs.com/package/@babel/polyfill) et l'ajoutez à votre tableau `require`.

### Exemple de fichier de test

Remarquez que vous aurez besoin d'utiliser les chemins de `System.import` pour tous vos fichiers du projet. Donc, si vous avez nommé votre projet `app` et que vous voulez importer votre `main.js` dans un fichier de test, vous aurez besoin de faire `import main from 'app/main'`.

```js
import test from 'ava';
import main from 'app/main';  // Indique à votre JSPM config "app/main.js"
import BigNumber from 'bignumber.js';  // Dans jspm_packages

function fn() {
	return Promise.resolve(new BigNumber('1234567890.123456789'));
}

test('example test', async t => {
	t.is((await fn()).toString(), '1234567890.123456789');
});
```
