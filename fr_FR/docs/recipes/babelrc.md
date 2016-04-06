___
**Note du traducteur**

C'est la traduction du fichier [babelrc.md](https://github.com/sindresorhus/ava/blob/master/docs/recipes/babelrc.md). Voici un [lien](https://github.com/sindresorhus/ava/compare/bcda7537dda7eeb38bef3405e1265226ba6d69ae...master#diff-3834ea415f09859260d100d1ec24207b) vers les différences avec le master de AVA (Si en cliquant sur le lien, vous ne trouvez pas le fichier `babelrc.md` parmi les fichiers modifiés, vous pouvez donc en déduire que la traduction est à jour).
___
# Configuration de Babel

Il y a plusieurs options pour configurer la façon dont AVA transpile vos tests en utilisant Babel.

 - [Spécifier une config complète dans `package.json`](#spécifier-une-config-complète-dans-packagejson)
 - [Étendre le `.babelrc` existant sans modification](#Étendre-le-babelrc-existant-sans-modification)
 - [Étendre le `.babelrc` existant avec des plugins supplémentaires ou prédéfinis](#Étendre-le-babelrc-existant-avec-des-plugins-supplémentaires-ou-prédéfinis)
 - [Étendre un fichier de config alternatif (pas `.babelrc`)](#Étendre-un-fichier-de-config-alternatif)
 - [Notes](#notes)

## Spécifier une config complète dans `package.json`

Par défaut, l'option `babelrc` est à `false`, ce qui signifie que les fichiers `.babelrc` ne sont pas pris en compte lors de la transpilation des tests. Cela signifie que vous devez spécifier votre config complète dans `package.json`.

```json
{
  "ava": {
    "babel": {
      "plugins": ["rewire"],
      "presets": ["es2015"]
    }
  }
}
```

## Étendre le `.babelrc` existant sans modification

Utilisez le raccourci `"inherit"` si vous voulez que vos tests soient transpilés de la même manière que vos sources. Il utilisera directement votre `.babelrc` (avec quelques [plugins internes](#notes) supplémentaires).

`package.json`:

```json
{
 "ava": {
   "babel": "inherit"
 }
}
```

## Étendre le `.babelrc` existant avec des plugins supplémentaires ou prédéfinis

Définissez `babelrc` à `true`. Il utilisera votre `.babelrc` et l'étendra avec les plugins supplémentaires spécifiés.

`package.json`:

```json
{
  "ava": {
    "babel": {
      "babelrc": true,
      "plugins": ["custom-plugin-name"],
      "presets": ["custom-preset"]
    }
  }
}
```

## Étendre un fichier de config alternatif


Si, pour une raison quelconque, vous ne souhaitez pas étendre `.babelrc`, définissez l'option `extends` avec une config alternative que vous souhaitez utiliser pendant vos tests.

`package.json`:

```json
{
  "ava": {
    "babel": {
      "extends": "./babel-test-config.json",
      "plugins": ["custom-plugin-name"],
      "presets": ["custom-preset"]
    }
  }
}
```

Ci-dessus, on utilise `babel-test-config.json` comme config de base et on l'étend avec des plugins personnalisés et des presets spécifiés.

## Notes

AVA ajoute *toujours* quelques plugins Babel personnalisés lors de la transpilation de vos plugins. Ils proposent une variété de fonctions :

 * Active le support de `power-assert`.
 * Réécrit le chemin interne des dépendances de AVA comme `babel-runtime` (important si vous utilisez encore `npm@2`).
 * Génére des metadata de test pour déterminer quels fichiers doivent être exécutés en premier (*à venir*).
 * Analyse statique de dépendance pour la précompilation (*à venir*).
