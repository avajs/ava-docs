___
**Note du traducteur**

C'est la traduction du fichier [maintaining.md](https://github.com/sindresorhus/ava/blob/master/maintaining.md). Voici un [lien](https://github.com/sindresorhus/ava/compare/0316047c8df66202e308ce095eb2f335f4aac1c1...master#diff-af20adbc8ab4842b04d1f5c7df6f563a) vers les différences avec le master de AVA (Si en cliquant sur le lien, vous ne trouvez pas le fichier `maintaining.md` parmi les fichiers modifiés, vous pouvez donc en déduire que la traduction est à jour).
___
# Maintenance [![Dependency Status](https://david-dm.org/sindresorhus/ava.svg)](https://david-dm.org/sindresorhus/ava) [![devDependency Status](https://david-dm.org/sindresorhus/ava/dev-status.svg)](https://david-dm.org/sindresorhus/ava#info=devDependencies)


## Test

 - `npm test`: Vérifie le code (Lint) et éxécute tous les tests avec une couverture de code
 - `npm run test-win`: Exécute les tests sous Windows.
 - `npm run coverage`: Génère un rapport de couverture de code pour les derniers tests exécutés (ouvre une fenêtre de navigateur).
 - `tap test/fork.js --bail`: Exécute un fichier de test spécifique et s'arrête au premier échec (utile quand on cherche des bugs).


## Procédure de version

- Mettez à niveau les dépendances.
- Assurez vous que [Travis CI](https://travis-ci.org/sindresorhus/ava) et [AppVeyor](https://ci.appveyor.com/project/sindresorhus/ava/branch/master) sont au vert.
- Publiez une nouvelle version en utilisant [`np`](https://github.com/sindresorhus/np) avec un numéro de version qui respecte [semver](http://semver.org).
- Ecrivez une [note de version](https://github.com/sindresorhus/ava/releases/new) en utilisant le même style que les notes précédentes.


## Pull requests

- Les nouvelles fonctionnalités doivent être livrées avec des tests et de la documentation.
- Assurez vous que le [guide de contribution](contributing.md) est respecté.
- Au moins deux membres de l'équipe doivent apposer un `LGTM` sur le pull request avant de le merger.
- Faites un squash des commits avant de merger.


## Analyse de l'exécution

Vous devez d'abord installer globalement [`iron-node`](https://github.com/s-a/iron-node) et / ou [`devtool`](https://github.com/Jam3/devtool) :

```
$ npm install --global iron-node devtool
```

A la racine du projet qui utilise AVA, exécutez :

```
$ iron-node node_modules/ava/profile.js <test-file>
```

Ou :

```
$ devtool node_modules/ava/profile.js <test-file>
```

Une fois que la fenêtre Dev Tools est chargée, activez l'analyse de la mémoire ou du CPU, puis appuyez sur <kbd>Cmd</kbd> <kbd>R</kbd> pour relancer les tests.

Dès que les tests sont terminés, arrêtez l'enregistrement et inspectez les résultats d'analyse'. Le flamegraph peut être affiché en choisissant `Chart` (graphique) dans la liste déroulante sur l'onglet `Profiles` (il y a d'autres vues `Tree (top down)` et `Heavy (bottom up)`).

Vous pouvez également consulter la page Settings (Paramètres) dans Dev Tools et activer une ou plusieurs options dans la section Profiling (Analyse).

##### Ressources utiles

 - [Une introduction pour déboguer Node.js avec `devtool`](http://mattdesl.svbtle.com/debugging-nodejs-in-chrome-devtools).
 - [Une vidéo d'introduction sur l'analyse d'exécution du CPU et de la mémoire avec Chrome DevTools](https://www.youtube.com/watch?v=KKwmdTByxLk).
