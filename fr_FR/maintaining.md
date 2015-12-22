___
**Note du traducteur**

C'est la traduction du fichier [maintaining.md](https://github.com/sindresorhus/ava/blob/master/maintaining.md). Voici un [lien](https://github.com/sindresorhus/ava/compare/0f0cbe9b0d5a71609a8f5a7afcb412f2bf41ad91...master#diff-af20adbc8ab4842b04d1f5c7df6f563a) vers les différences avec le master de AVA (Si en cliquant sur le lien, vous ne trouvez pas le fichier `maintaining.md` parmi les fichiers modifiés, vous pouvez donc en déduire que la traduction est à jour).
___
# Maintenance


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
