___
**Note du traducteur**

C'est la traduction du fichier [debugging-with-webstorm.md](https://github.com/avajs/ava/blob/master/docs/recipes/debugging-with-webstorm.md). Voici un [lien](https://github.com/avajs/ava/compare/7d3b58ba8c433b99a2221dd38aeff736cbd97a24...master#diff-1fb9cdb432e04d416229256c338f1a06) vers les différences avec le master de AVA (Si en cliquant sur le lien, vous ne trouvez pas le fichier `debugging-with-webstorm.md` parmi les fichiers modifiés, vous pouvez donc en déduire que la traduction est à jour).
___
# Débogage des tests avec WebStorm

Traductions : [English](https://github.com/avajs/ava/blob/master/docs/recipes/debugging-with-webstorm.md)

**Pas encore disponible. Nécessite la version 0.17.0 de AVA qui n'est pas encore libérée**

Depuis la version 2016.2, [WebStorm](https://www.jetbrains.com/webstorm/) et les autres IDE de JetBrains (IntelliJ IDEA Ultimate, PHPStorm, PyCharm Professional et RubyMine avec le plugin Node.js installé) permettent de faire du débogage des tests AVA.


## Installation

Ajoutez une nouvelle *Node.js Run/Debug configuration* : sélectionnez `Edit Configurations...` depuis la liste déroulante en haut à droite, puis cliquez `+` et séléctionnez *Node.js*.

Dans le champ `JavaScript file`, renseignez le chemin vers AVA dans le répertoire `node_modules` du projet : `node_modules/.bin/ava` sur macOS et Linux ou `node_modules/.bin/ava.cmd` sur Windows.

Dans `Application parameters` passez les options du CLI que vous utilisez et les fichiers de test que vous souhaitez déboguer, par exemple `--verbose test.js`.

Sauvez la configuration.


## Débogage

Définissez des breakpoints dans le code.

Appuyez sur le bouton `Debug` à côté de la liste des configurations en haut à droite. Le *Debug tool window* apparaît. Une fois que breakpoint est atteint, vous pouvez évaluer les variables et parcourir le code. Lors du débogage de plusieurs fichiers de test, vous pouvez basculer entre les processus en utilisant la liste déroulante dans le volet Frames.
