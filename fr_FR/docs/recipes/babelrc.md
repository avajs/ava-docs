___
**Note du traducteur**

C'est la traduction du fichier [babelrc.md](https://github.com/avajs/ava/blob/master/docs/recipes/babelrc.md). Voici un [lien](https://github.com/avajs/ava/compare/0464b14527a7a5be805a79eff7e5c862693663c0...master#diff-3834ea415f09859260d100d1ec24207b) vers les différences avec le master de AVA (Si en cliquant sur le lien, vous ne trouvez pas le fichier `babelrc.md` parmi les fichiers modifiés, vous pouvez donc en déduire que la traduction est à jour).
___
# Configuration de Babel

Traductions : [English](https://github.com/avajs/ava/blob/master/docs/recipes/babelrc.md)

Il y a plusieurs options pour configurer la façon dont AVA transpile vos tests en utilisant Babel.

 - [Comportement par défaut du transpileur de AVA](#comportement-par-défaut-du-transpileur-de-ava)
 - [Personnaliser la manière dont AVA transpile vos tests](#personnaliser-la-manière-dont-ava-transpile-vos-tests)
 - [Transpilation des sources](#transpilation-des-sources)
 - [Transpilation des tests et des sources de la même manière](#transpilation-des-tests-et-des-sources-de-la-même-manière)
 - [Étendre la configuration de transpilation de vos sources](#Étendre-la-configuration-de-transpilation-de-vos-sources)
 - [Étendre un fichier de config alternatif (pas `.babelrc`)](#Étendre-un-fichier-de-config-alternatif)

## Comportement par défaut du transpileur de AVA

Par défaut, AVA transpile vos tests et vos helpers en utilisant le preset de Babel [`@ava/stage-4`](https://github.com/avajs/babel-preset-stage-4). C'est une bonne option pour des petits modules où vous ne souhaitez pas construire une étape de build pour transpiler vos sources avant le déploiement dans `npm`.

## Personnaliser la manière dont AVA transpile vos tests

Vous pouvez remplacer la configuration par défaut de Babel dans le `package.json` pour que AVA l'utilise pour la transpilation des tests. Par exemple, la configuration ci-dessous ajoute le plugin Babel `rewire` et ajoute le preset de Babel [`stage-3`](http://babeljs.io/docs/plugins/preset-stage-3/).

```json
{
  "ava": {
    "babel": {
      "plugins": ["rewire"],
      "presets": ["@ava/stage-4", "stage-3"]
    }
  }
}
```

## Transpilation des sources

Pour transpiler vos sources, vous aurez besoin de définir une [`config babel` ](http://babeljs.io/docs/usage/babelrc/) dans `package.json` ou dans un fichier `.babelrc`. De plus, vous aurez besoin de dire à AVA de charger [`babel-register`](http://babeljs.io/docs/usage/require/) dans tous les processus, en l'ajoutant à la section `require` de votre config AVA :

`package.json`

```json
{
  "ava": {
    "require": ["babel-register"]
  },
  "babel": {
    "presets": ["@ava/stage-4"]
  }
}
```

Notez que le chargement de `babel-register` dans chaque processus a un coût non négligeable sur la performance. Si vous avez beaucoup de fichiers de test, vous devriez peut-être envisager l'utilisation d'une étape de build pour transpiler vos sources *avant* l'exécution de vos tests. Ce n'est pas idéal, puisque cela complique l'utilisation du mode watch de AVA, nous vous recommandons d'utiliser `babel-register` tant que cela ne pénalise pas trop les performances. La mise en place d'une étape de précompilation dépasse le sujet de ce document, mais nous vous recommandons de consulter l'un des nombreux [systèmes de build que supporte Babel](http://babeljs.io/docs/setup/). Il y a une [issue ouverte](https://github.com/avajs/ava/issues/577) afin de discuter des possibilités pour améliorer cette expérience.

## Transpilation des tests et des sources de la même manière

Utilisez le raccourci `"inherit"` si vous voulez que vos tests soient transpilés de la même manière que vos sources (comme spécifié dans votre [`babelrc`](http://babeljs.io/docs/usage/babelrc/)). AVA ajoutera quelques [plugins internes](#notes) supplémentaires lors de la transpilation de vos tests, mais ils n'affecteront pas le comportement du code de votre test.

`package.json`:

```json
{
	"ava": {
    "require": "babel-register",
    "babel": "inherit"
  },
  "babel": {
    "presets": ["@ava/stage-4", "react"]
  }
}
```

Dans l'exemple ci-dessus, les tests et les sources seront transpilés en utilisant les presets [`@ava/stage-4`](https://github.com/avajs/babel-preset-stage-4) et [`react`](http://babeljs.io/docs/plugins/preset-react/).

AVA recherche un fichier `.babelrc` uniquement dans le même répertoire que le fichier `package.json`. S'il n'est pas trouvé, il suppose que votre configuration Babel se trouve dans le fichier `package.json`.

## Étendre la configuration de transpilation de vos sources

En spécifiant la config Babel pour vos tests, vous pouvez définir l'option `babelrc` à `true`. Cela fusionnera les plugins spécifiés avec ceux de votre [`babelrc`](http://babeljs.io/docs/usage/babelrc/).

`package.json`:

```json
{
	"ava": {
    "require": "babel-register",
		"babel": {
			"babelrc": true,
			"plugins": ["custom-plugin-name"],
			"presets": ["custom-preset"]
		}
  },
  "babel": {
    "presets": ["@ava/stage-4", "react"]
	}
}
```

Dans l'exemple ci-dessus, les *sources* sont compilées en utilisant [`@ava/stage-4`](https://github.com/avajs/babel-preset-stage-4) et [`react`](http://babeljs.io/docs/plugins/preset-react/), les *tests* utilisent les mêmes plugins avec en plus des plugins spécifiés `custom`.

AVA recherche un fichier `.babelrc` uniquement dans le même répertoire que le fichier `package.json`. S'il n'est pas trouvé, il suppose que votre configuration Babel se trouve dans le fichier `package.json`.

## Étendre un fichier de config alternatif


Si, pour une raison quelconque, votre config Babel n'est pas spécifiée dans l'un des emplacements par défaut ([`.babelrc` ou `package.json`](http://babeljs.io/docs/usage/babelrc/), vous pouvez définir l'option `extends` avec une config alternative que vous souhaitez utiliser pendant vos tests.

`package.json`:

```json
{
  "ava": {
    "require": "babel-register",
    "babel": {
      "extends": "./babel-test-config.json",
      "plugins": ["custom-plugin-name"],
      "presets": ["custom-preset"]
    }
  }
}
```

Ci-dessus, on utilise `babel-test-config.json` comme config de transpilation pour les *sources* et comme config de base pour les *tests*. Pour les *tests*, on étend la config de base avec des plugins personnalisés et des presets spécifiés.
