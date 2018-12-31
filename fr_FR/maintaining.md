___
**Note du traducteur**

C'est la traduction du fichier [maintaining.md](https://github.com/avajs/ava/blob/master/maintaining.md). Voici un [lien](https://github.com/avajs/ava/compare/afe028a0a05c8b8bb0b33e12b557ff455e47355d...master#diff-af20adbc8ab4842b04d1f5c7df6f563a) vers les différences avec le master de AVA (Si en cliquant sur le lien, vous ne trouvez pas le fichier `maintaining.md` parmi les fichiers modifiés, vous pouvez donc en déduire que la traduction est à jour).
___
# Maintenance [![Dependency Status](https://david-dm.org/avajs/ava.svg)](https://david-dm.org/avajs/ava) [![devDependency Status](https://david-dm.org/avajs/ava/dev-status.svg)](https://david-dm.org/avajs/ava#info=devDependencies)


## Conduite

**Soyez gentil avec tout le monde.**
Lisez et adhérez au [Code de Conduite](code-of-conduct.md).


## Test

 - `npm test`: Vérifie le code (Lint) et exécute tous les tests avec une couverture de code
 - `npm run test-win`: Exécute les tests sous Windows.
 - `tap test/fork.js --bail`: Exécute un fichier de test spécifique et s'arrête au premier échec (utile quand on cherche des bugs).


## Procédure de version

- Mettez à niveau les dépendances.
- Assurez vous que [Travis CI](https://travis-ci.org/avajs/ava) soit au vert.
- Si [nécessaire](docs/support-statement.md), mettez à jour le champs `engines` dans `package.json`
- Publiez une nouvelle version en utilisant [`np`](https://github.com/sindresorhus/np) avec un numéro de version qui respecte [semver](http://semver.org).
- Écrivez une [note de version](https://github.com/avajs/ava/releases/new) en utilisant le même style que les notes précédentes.


## Pull requests

- Les nouvelles fonctionnalités doivent être livrées avec des tests et de la documentation.
- Assurez vous que le [guide de contribution](contributing.md) est respecté.
- Au moins un membre de l'équipe doit apposer un `LGTM` sur le pull request avant de le merger.
- Faites un squash des commits avant de merger. *[Exemple](https://github.com/avajs/ava/commit/0675d3444da6958b54c7e5eada91034e516bc97c)*


## Étiquettes d'une issue

Ajoutez des étiquettes lors du triage des issues :

* `babel`: Utilisez-la lorsque l'issue est liée à notre infrastructure Babel
* `blocked`: Utilisez-la quand l'issue est bloquée. Veuillez laisser un commentaire ou modifiez la description de l'issue pour expliquer ce qui bloque
* `bug`: Utilisez-la pour les bugs de AVA
* `DO NOT MERGE`: Utilisez-la pour les pull requests d'exploration que nous ne devons pas merger
* `docs`: Utilisez-la pour suivre l'amélioration de la documentation
* `enhancement`: Utilisez-la pour les demandes de fonctionnalités
* `good for beginner`: Utilisez-la pour les issues qui sont bonnes pour les débutants
* `help wanted`: Utilisez-la pour les issues où nous aimerions vraiment l'aide de personnes extérieures à l'équipe de base
* `performance`: Utilisez-la pour issues liées à la performance
* `question`: Utilisez-la pour les issues qui sont en phase de discussion

Veuillez notez la priorité avec les étiquettes suivantes :

* `priority`: Issues à traiter le plus tôt possible
* `low priority`: Issues que nous aimerions voir avancées
* `future`: Issues où nous prévoyons leur traitement plus tard. Celles-ci sont des suggestions à long terme pour lesquelles nous n'accepterons probablement pas de PR

Utilisez l'étiquette `assigned` lorsque quelqu'un est en train de travailler sur l'issue, ainsi nous évitons du travail en double.

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

Dès que les tests sont terminés, arrêtez l'enregistrement et inspectez les résultats d'analyse. Le flamegraph peut être affiché en choisissant `Chart` (graphique) dans la liste déroulante sur l'onglet `Profiles` (il y a d'autres vues `Tree (top down)` et `Heavy (bottom up)`).

Vous pouvez également consulter la page Settings (Paramètres) dans Dev Tools et activer une ou plusieurs options dans la section Profiling (Analyse).

##### Ressources utiles

 - [Une introduction pour déboguer Node.js avec `devtool`](http://mattdesl.svbtle.com/debugging-nodejs-in-chrome-devtools).
 - [Une vidéo d'introduction sur l'analyse d'exécution du CPU et de la mémoire avec Chrome DevTools](https://www.youtube.com/watch?v=KKwmdTByxLk).


## Benchmarking

D'abord, collectez des données de benchmark pour une branche/un commit :

```
$ node bench/run
```

Une fois que vous avez recueilli des données à partir de deux/trois branches/commits :

```
$ node bench/compare
```

*Vous pouvez par exemple recueillir des données de benchmark de votre travail et du dernier commit.*

![](https://cloud.githubusercontent.com/assets/4082216/12700805/bf18f730-c7bf-11e5-8a4f-fec0993c053f.png)

Vous pouvez maintenant lancer un sous-ensemble de test :

```
$ node bench/run.js concurrent/sync.js serial/sync.js -- concurrent/sync.js -- serial/sync.js
```

Veuillez remarquer le séparateur `--`. Cela correspond au même benchmark que les trois commandes suivantes.

```
$ ava concurrent/sync.js serial/sync.js
$ ava concurrent/sync.js
$ ava serial/sync.js
```

Aussi, si vous faites une suite de benchmark qui doit échouer, vous devez ajouter l'option `--should-fail` dans ce groupe :

```
$ node bench/run.js concurrent/sync.js -- --should-fail other/failures.js
```

Le benchmark ci-dessus a deux commandes, mais il s'attend à ce que la seconde échoue.


## Intégration des nouveaux membres de base

- Ajoutez l'utilisateur dans `readme.md` et `package.json`.
- Ajoutez l'utilisateur comme collaborateur à tous dépôts liés à AVA et les packages npm.
- Partagez le compte Twitter et nous vous encourageons à tweeter/retweeter des choses pertinentes.
