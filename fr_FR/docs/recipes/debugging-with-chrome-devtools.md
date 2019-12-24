___
**Note du traducteur**

C'est la traduction du fichier [debugging-with-chrome-devtools.md](https://github.com/avajs/ava/blob/master/docs/recipes/debugging-with-chrome-devtools.md). Voici un [lien](https://github.com/avajs/ava/compare/4953457277a355e231beaa11431eb58a209ae7fc...master#diff-7621c4f96f622506986f9cf7a125d6a1) vers les différences avec le master de AVA (Si en cliquant sur le lien, vous ne trouvez pas le fichier `debugging-with-chrome-devtools.md` parmi les fichiers modifiés, vous pouvez donc en déduire que la traduction est à jour).
___
# Débogage des tests avec Chrome DevTools

Traductions : [English](https://github.com/avajs/ava/blob/master/docs/recipes/debugging-with-chrome-devtools.md)

**Cette recette décrit la nouvelle commande `inspect` dans la prochaine version de AVA 3. Consultez à la place la documentation de [AVA 2](https://github.com/avajs/ava-docs/blob/75f6a363f28f1ed4b7c84d50838c9ba036ef6b9e/fr_FR/docs/recipes/debugging-with-chrome-devtools.md).**

Vous pouvez déboguer vos tests en utilisant [Chrome DevTools](https://developers.google.com/web/tools/chrome-devtools).

Ouvrez Chrome, puis accédez à <chrome://inspect/>. Cliquez sur le lien *Open dedicated DevTools for Node* dans la section *Devices*.

Dans la fenêtre *DevTools for Node*, naviguez vers *Sources* et dans la colonne de gauche, sélectionnez *Filesystem*. Ajoutez votre répertoire de projet à l'espace de travail. Assurez-vous d'accorder l'autorisation.

Exécutez maintenant un fichier de test spécifique :

```console
$ npx ava debug test.js
```

Les DevTools devraient se connecter automatiquement et vos tests s'exécuteront. Utilisez DevTools pour définir des points d'arrêt ou utilisez le mot clé `debugger`.

Exécutez avec l'option `--break` pour vous assurer que les DevTools atteignent un point d'arrêt juste avant le chargement du fichier de test :

```console
$ inspect node_modules/ava/profile.js some/test/file.js
```

Vous pouvez également personnaliser le port. La valeur par défaut est `9229` :

```console
npx ava debug --port 9230 test.js
```

Vous devrez ajouter une connexion pour ce port dans l'onglet *Connection*. AVA se lie uniquement à `localhost`.