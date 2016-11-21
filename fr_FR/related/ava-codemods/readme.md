___
**Note du traducteur**

C'est la traduction du fichier [readme.md](https://github.com/avajs/ava-codemods/blob/master/readme.md). Voici un [lien](https://github.com/avajs/ava-codemods/compare/cbddb4b4256a40523b78e873d0ea1248fab17150...master#diff-0730bb7c2e8f9ea2438b52e419dd86c9) vers les différences avec le master de ava-codemods (Si en cliquant sur le lien, vous ne trouvez pas le fichier `readme.md` parmi les fichiers modifiés, vous pouvez donc en déduire que la traduction est à jour).
___
# ava-codemods [![Build Status](https://travis-ci.org/avajs/ava-codemods.svg?branch=master)](https://travis-ci.org/avajs/ava-codemods)

Traductions : [English](https://github.com/avajs/ava-codemods/blob/master/readme.md)

> Codemods pour [AVA](https://ava.li) qui simplifie la mise à jours des versions plus récentes et la migration vers AVA

<img src="https://github.com/avajs/ava-codemods/blob/master/screenshot.gif" width="440" align="right">

Les codemods sont des petits programmes qui vous aident à automatiser les changements de votre code. Considérez-les comme des chercheurs et des remplaçants sous stéroïdes.

Ce module contient un ensemble de codemods qui vous permettent de mettre à jour votre code entre les différents versions de AVA et de migrer d'exécuteurs de test existant vers AVA. Il est maintenu par l'équipe de AVA et sera mis à jour à chaque fois que nous introduisons une modification de l'API qui entraine une rupture.


## Installation

```
$ npm install --global ava-codemods
```

Ceci installe deux binaires `ava-codemods` et `tape-to-ava`.


## Migration vers AVA

Actuellement, nous supportons la migration de [tape](https://github.com/substack/tape) vers AVA.

```
$ tape-to-ava --help

	Usage
	  $ tape-to-ava <path> [options]

	path	Files or directory to transform. Can be a glob like src/**.test.js (Fichiers ou répertoires à transformer. Cela peut-être un glob comme src/**.test.js)

	Options
	  --force, -f	Bypass Git safety checks and forcibly run codemods (Contourner les contrôles de sécurité et forcer l'exécution de codemods)
	  --dry, -d		Dry run (no changes are made to files)
	  --parser		The parser to use for parsing your source files (babel | babylon | flow)  [babel]
```

Pour transformer tous les fichiers de test dans un répertoire, exécutez `tape-to-ava mySrcFolder` dans votre terminal. Seuls les fichiers qui requièrent ou importent tape seront transformés. Noter que pour les errurs dans la console, une intervention manuelle peut être nécessaire.

As we cannot statically determine if your sequential tape tests are able to run in parallel, all tests are transformed into `test.serial`. To speed up the AVA test execution you can remove `.serial` where applicable.


## Mettre à jour la version de AVA

```
$ ava-codemods --help

  Usage
    $ ava-codemods [<file|glob> ...]

  Options
    --force, -f    Bypass safety checks and forcibly run codemods (Contourner les contrôles de sécurité et forcer l'exécution de codemods)

  Available upgrades
    - 0.16.x → 0.17.x
    - 0.13.x → 0.14.x
```

Exécutez simplement `ava-codemods` dans votre terminal et répondez à quelques questions. Vous pouvez passer un nom de fichier directement au CLI. Si vous ne le faites pas, on vous en demandera un.

Assurez-vous de faire une sauvegarde de vos tests ou de committer vos derniers changement avant de l'exécuter.


### Codemods supportés

#### Montée de version vers 0.17.x

- Renomme `t.error()` en `t.ifError()`

#### Montée de version vers 0.14.x

- Renomme `t.ok()` en `t.truthy()`
- Renomme `t.notOk()` en `t.falsy()`
- Renomme `t.same()` en `t.deepEqual()`
- Renomme `t.notSame()` en `t.notDeepEqual()`


## Licence

MIT © [James Talmage](https://github.com/jamestalmage)
