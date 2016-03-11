___
**Note du traducteur**

C'est la traduction du fichier [code-coverage.md](https://github.com/sindresorhus/ava/blob/master/docs/recipes/code-coverage.md). Voici un [lien](https://github.com/sindresorhus/ava/compare/1868204c1901f45b4f66a520ef6486fdd71fe1d2...master#diff-b3aa0c81a407f54f636a1cf5a619a4a6) vers les différences avec le master de AVA (Si en cliquant sur le lien, vous ne trouvez pas le fichier `code-coverage.md` parmi les fichiers modifiés, vous pouvez donc en déduire que la traduction est à jour).
___
# Couverture de code

Traductions : [English](https://github.com/sindresorhus/ava/blob/master/docs/recipes/code-coverage.md), [Español](https://github.com/sindresorhus/ava-docs/blob/master/es_ES/docs/recipes/code-coverage.md),  [日本語](https://github.com/sindresorhus/ava-docs/blob/master/ja_JP/docs/recipes/code-coverage.md), [Português](https://github.com/sindresorhus/ava-docs/blob/master/pt_BR/docs/recipes/code-coverage.md), [Русский](https://github.com/sindresorhus/ava-docs/blob/master/ru_RU/docs/recipes/code-coverage.md)

Comme AVA [génère les fichiers de test][process-isolation], vous ne pouvez pas utiliser [`istanbul`] pour la couverture de code. A la place, vous pouvez le faire avec [`nyc`] qui est essentiellement comme [`istanbul`] avec le support des sous-processus (sub-process).

## Configuration

Tout d'abord, installez NYC :

```
$ npm install nyc --save-dev
```

Ensuite ajoutez les répertoires `.nyc_output` et `coverage` à votre fichier `.gitignore`.

`.gitignore`:

```
node_modules
coverage
.nyc_output
```

## Couverture de ES5

L'utilisation de NYC pour assurer la couverture de code pour la production écrite en ES5 est simple. Ajoutez simplement `nyc` à votre script de test :

```json
{
	"scripts": {
		"test": "nyc ava"
	}
}
```

C'est tout !

Si vous voulez créer des rapports de couverture en HTML, ou télécharger les données de couverture depuis Coveralls, veuillez regarder cela quelques sections ci-dessous.

## Couverture de ES2015

L'utilisation de Babel pour transpiler votre code en production est un peu plus compliqué. Ici, nous l'avons décomposé en plusieurs étapes.

### Configurez Babel

Tout d'abord, nous aurons besoin d'une configuration de Babel. Ce qui suit est juste un exemple. Vous devrez le modifier pour l'adapter à vos besoins.

`package.json`:
```json
{
	"babel": {
		"presets": ["es2015"],
		"plugins": ["transform-runtime"],
		"ignore": "test.js",
		"env": {
			"development": {
				"sourceMaps": "inline"
			}
		}
	}
}
```

Il y a deux choses importantes à noter sur l'exemple ci-dessus.

1. Nous ignorons les fichiers de test car AVA gère déjà pour vous la transpilation des tests.

2. Nous spécifions `inline` pour sourceMaps pour l'environnement `development`. Ceci est important pour générer correctement la couverture. L'utilisation de la section `env` dans la configuration de Babel, nous permet de désactiver les sources maps lors du build de la production.


### Créez un script build

Comme il est peu probable de vouloir `inline` pour les source maps dans votre code de production. Vous devez spécifier une autre variable d'environnement dans votre script build :

`package.json`

```json
{
	"scripts": {
		"build": "BABEL_ENV=production babel --out-dir=dist index.js"
	}
}
```

> ATTENTION : `BABEL_ENV=production` ne fonctionne pas sous Windows, vous devez utilisez le mot-clef `set` (`set BABEL_ENV=production`).  Pour un build qui fonctionne sur toutes les OS, jetez un oeil à [`cross-env`].

Vous remarquez que le script build n'a pas vraiment grand-chose à voir avec AVA, c'est juste une démonstration de comment utiliser la configuration `env` de Babel pour manipuler votre config qui est ainsi compatible avec AVA.

### Utilisez le hook `require` de Babel

Pour utiliser le hook `require` de Babel, ajoutez `babel-core/register` à la section `require` de votre config AVA dans `package.json`.

```json
{
	"ava": {
		"require": ["babel-core/register"]
	}
}
```

*Remarque* : Vous pouvez aussi définir le hook require en ligne de commande : `ava --require=babel-core/register`. Toutefois, la configuration dans `package.json` vous évite de taper à plusieurs reprises l'option.

### Regroupez le tout

En combinant les étapes ci-dessus, votre `package.json` complet devrait ressembler à ceci :

```json
{
	"scripts": {
		"test": "nyc ava",
		"build": "BABEL_ENV=production babel --out-dir=dist index.js"
	},
	"babel": {
		"presets": ["es2015"],
		"plugins": ["transform-runtime"],
		"ignore": "test.js",
		"env": {
			"development": {
				"sourceMaps": "inline"
			}
		}
	},
	"ava": {
		"require": ["babel-core/register"]
	}
}
```


## Rapports HTML

NYC crée un fichier de couverture `json` pour chaque processus dans le répertoire `.nyc_ouput`.

Pour les regrouper dans un rapport HTML lisible par un être humain, procédez comme suit :

```
$ ./node_modules/.bin/nyc report --reporter=html
```

Ou utilisez un script npm qui demande moins de frappe :

```json
{
	"scripts": {
		"report": "nyc report --reporter=html"
	}
}
```

Cela générera un fichier HTML dans le répertoire `coverage`.


## Couverture hébergée

### Travis CI & Coveralls

Tout d'abord, vous devez vous connecter à [coveralls.io] et activer votre dépôt.

Une fois que cela est fait, ajoutez [`coveralls`] comme une dépendance de développement :

```
$ npm install coveralls --save-dev
```

Ensuite, ajoutez ce qui suit à votre `.travis.yml` :

```yaml
after_success:
	- './node_modules/.bin/nyc report --reporter=text-lcov | ./node_modules/.bin/coveralls'
```

Votre rapport de couverture apparaîtra alors sur coveralls peu de temps après que le service Travis soit terminé.

[`babel`]:      https://github.com/babel/babel
[coveralls.io]: https://coveralls.io
[`coveralls`]:  https://github.com/nickmerwin/node-coveralls
[`cross-env`]:  https://github.com/kentcdodds/cross-env
[process-isolation]: https://github.com/sindresorhus/ava-docs/blob/master/fr_FR/readme.md#isolement-du-processus
[`istanbul`]:   https://github.com/gotwarlost/istanbul
[`nyc`]:        https://github.com/bcoe/nyc
