___
**Note du traducteur**

C'est la traduction du fichier [debugging-with-chrome-devtools.md](https://github.com/avajs/ava/blob/main/docs/recipes/debugging-with-chrome-devtools.md). Voici un [lien](https://github.com/avajs/ava/compare/b208d143ad852dc95aa8b44eed94ac1f404a25f4...main#diff-f5e363bb05df566efcd9000a18ff662f283a921b4f0e1bcf15e4a46560b663ff) vers les différences avec le master de AVA (Si en cliquant sur le lien, vous ne trouvez pas le fichier `debugging-with-chrome-devtools.md` parmi les fichiers modifiés, vous pouvez donc en déduire que la traduction est à jour).
___
# Débogage des tests avec Chrome DevTools

Traductions : [English](https://github.com/avajs/ava/blob/main/docs/recipes/debugging-with-chrome-devtools.md)

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

Par défaut, l'inspecteur écoute sur `127.0.0.1:9229`. Vous pouvez personnaliser l'hôte et le port :

```console
npx ava debug --host 0.0.0.0 --port 9230 test.js
```

Vous devrez ajouter une connexion pour ce port dans l'onglet *Connection*.
