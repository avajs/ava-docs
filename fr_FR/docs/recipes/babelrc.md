___
**Note du traducteur**

C'est la traduction du fichier [babelrc.md](https://github.com/sindresorhus/ava/blob/master/docs/recipes/babelrc.md). Voici un [lien](https://github.com/sindresorhus/ava/compare/3201b1b4ff80ff75f0e1c288ca7da22f92c9b814...master#diff-3834ea415f09859260d100d1ec24207b) vers les différences avec le master de AVA (Si en cliquant sur le lien, vous ne trouvez pas le fichier `babelrc.md` parmi les fichiers modifiés, vous pouvez donc en déduire que la traduction est à jour).
___
# Configuration de Babel

Traductions : [English](https://github.com/sindresorhus/ava/blob/master/docs/recipes/babelrc.md)

Il y a plusieurs options pour configurer la façon dont AVA transpile vos tests en utilisant Babel.

 - [Comportement par défaut du transpileur de AVA](#comportement-par-défaut-du-transpileur-de-ava)
 - [Personnaliser la manière dont AVA transpile vos tests](#personnaliser-la-manière-dont-ava-transpile-vos-tests)
 - [Transpilation des sources](#transpilation-des-sources)
 - [Transpilation des tests et des sources de la même manière](#transpilation-des-tests-et-des-sources-de-la-même-manière)
 - [Étendre la configuration de transpilation de vos sources](#Étendre-la-configuration-de-transpilation-de-vos-sources)
 - [Étendre un fichier de config alternatif (pas `.babelrc`)](#Étendre-un-fichier-de-config-alternatif)
 - [Notes](#notes)

## Comportement par défaut du transpileur de AVA

Par défaut, AVA transpile vos tests (et uniquement vos tests) en utilisant les presets de Babel [`es2015`](http://babeljs.io/docs/plugins/preset-es2015/) et [`stage-2`](http://babeljs.io/docs/plugins/preset-stage-2/). C'est une bonne option pour des petits modules où vous ne souhaitez pas contruire une étape de build pour transpiler vos sources avant le déploiement dans `npm`.

## Personnaliser la manière dont AVA transpile vos tests

Vous pouvez remplacer la configuration par défaut de Babel dans le `package.json` pour que AVA l'utilise pour la transpilation des tests. Par exemple, la configuration ci-dessous ajoute le plugin Babel `rewire` et les options pour utiliser uniquement le preset [`stage-3`](http://babeljs.io/docs/plugins/preset-stage-3/) (qui est un sous-ensemble de [`stage-2`](http://babeljs.io/docs/plugins/preset-stage-2/)).

```json
{
  "ava": {
    "babel": {
      "plugins": ["rewire"],
      "presets": ["es2015", "stage-3"]
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
    "presets": ["es2015"]
  }
}
```

Notez que le chargement de `babel-register` dans chaque processus a un coût non négligeable sur la performance. Si vous avez beaucoup de fichiers de test, vous devriez peut-être envisager l'utilisation d'une étape de build pour transpiler vos sources *avant* l'exécution de vos tests. Ce n'est pas idéal, puisque cela complique l'utilisation du mode watch de AVA, nous vous recommandons d'utiliser `babel-register` tant que cela ne pénalise pas trop les performances. La mise en place d'une étape de précompilation dépasse le sujet de ce document, mais nous vous recommandons de consulter l'un des nombreux [systèmes de build que supporte Babel](http://babeljs.io/docs/setup/). Il y a une [issue ouverte](https://github.com/sindresorhus/ava/issues/577) afin de discuter des possibilités pour améliorer cette expérience.

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
    "presets": ["es2015", "react"]
  }
}
```

Dans l'exemple ci-dessus, les tests et les sources seront transpilés en utilisant les presets [`es2015`](http://babeljs.io/docs/plugins/preset-es2015/) et [`react`](http://babeljs.io/docs/plugins/preset-react/).

## Étendre la configuration de transpilation de vos sources

En spécifiant la config Babel pour vos tests, vous pouvez définir l'option `babelrc` à `true`. Cela fusionnera les plugins spécifié avec ceux de votre [`babelrc`](http://babeljs.io/docs/usage/babelrc/).

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
    "presets": ["es2015", "react"]
	}
}
```

Dans l'exemple ci-dessus, les *sources* sont compilées en utilisant [`es2015`](http://babeljs.io/docs/plugins/preset-es2015/) et [`react`](http://babeljs.io/docs/plugins/preset-react/), les *tests* utilisent les mêmes plugins avec en plus des plugins spécifiés `custom`.

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

## Notes

AVA ajoute *toujours* quelques plugins Babel personnalisés lors de la transpilation de vos plugins. Ils proposent une variété de fonctions :

 * Active le support de `power-assert`.
 * Réécrit le chemin interne des dépendances de AVA comme `babel-runtime` (important si vous utilisez encore `npm@2`).
 * [`ava-throws-helper`](https://github.com/jamestalmage/babel-plugin-ava-throws-helper) aide AVA à [détecter et à signaler](https://github.com/sindresorhus/ava/pull/742) une mauvaise utilisation de l'assertion `t.throws`.
 * Génére des metadata de test pour déterminer quels fichiers doivent être exécutés en premier (*à venir*).
 * Analyse statique de dépendance pour la précompilation (*à venir*).
