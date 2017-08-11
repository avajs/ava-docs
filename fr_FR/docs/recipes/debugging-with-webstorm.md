___
**Note du traducteur**

C'est la traduction du fichier [debugging-with-webstorm.md](https://github.com/avajs/ava/blob/master/docs/recipes/debugging-with-webstorm.md). Voici un [lien](https://github.com/avajs/ava/compare/d8c21a6b8102bcc11daacba2ea6af6f9b9713959...master#diff-1fb9cdb432e04d416229256c338f1a06) vers les différences avec le master de AVA (Si en cliquant sur le lien, vous ne trouvez pas le fichier `debugging-with-webstorm.md` parmi les fichiers modifiés, vous pouvez donc en déduire que la traduction est à jour).
___
# Débogage des tests avec WebStorm

Traductions : [English](https://github.com/avajs/ava/blob/master/docs/recipes/debugging-with-webstorm.md)

Depuis la version 2016.2, [WebStorm](https://www.jetbrains.com/webstorm/) et les autres IDE de JetBrains (IntelliJ IDEA Ultimate, PHPStorm, PyCharm Professional et RubyMine avec le plugin Node.js installé) permettent de faire du débogage des tests AVA.


## Installation en utilisant Node.js

Ajoutez une nouvelle *Node.js Run/Debug configuration* : sélectionnez `Edit Configurations...` depuis la liste déroulante en haut à droite, puis cliquez `+` et sélectionnez *Node.js*.

Dans le champ `JavaScript file`, renseignez le chemin vers AVA dans le répertoire `node_modules` du projet : `node_modules/.bin/ava` sur macOS et Linux ou `node_modules/.bin/ava.cmd` sur Windows.

Dans `Application parameters` passez les options du CLI que vous utilisez et les fichiers de test que vous souhaitez déboguer, par exemple `--verbose test.js`.

Dans `Node parameters`, pour Node.js 7+, passez l'option `--inspect-brk` pour activer l'inspecteur de Node. Pour les versions antérieures utilisez `--debug-brk`.

Sauvez la configuration.

## Installation en utilisant npm

Exécutez `ava --init` dans le répertoire de votre projet pour ajouter AVA à votre `package.json`.

Votre `package.json` ressemblera à quelque chose comme cela :

```json
{
	"name": "awesome-package",
	"scripts": {
		"test": "ava"
	},
	"devDependencies": {
		"ava": "^0.20.0"
	}
}
```

Ajoutez une nouvelle *npm Run/Debug configuration* : sélectionnez `Edit Configurations...` depuis la liste déroulante en haut à droite, puis cliquez `+` et sélectionnez *npm*.

Utilisez les paramètres de configurations suivants :

- `package.json`: Le chemin du fichier `package.json` de votre projet
- `Command`: `test`

Votre IDE exécutera alors `npm run test` et donc appellera `node_modules/.bin/ava` et la configuration AVA que vous avez spécifiée dans votre package.json.

Dans `Node parameters`, pour Node.js 7+ passez `--inspect-brk` ou `--debug-brk` pour les versions antérieures.

N'oubliez pas de sélectionner un interpréteur Node.js.

Sauvez la configuration.

## Débogage

Définissez des breakpoints dans le code.

Appuyez sur le bouton `Debug` à côté de la liste des configurations en haut à droite. Le *Debug tool window* apparaît. Une fois que breakpoint est atteint, vous pouvez évaluer les variables et parcourir le code. Lors du débogage de plusieurs fichiers de test, vous pouvez basculer entre les processus en utilisant la liste déroulante dans le volet Frames.
