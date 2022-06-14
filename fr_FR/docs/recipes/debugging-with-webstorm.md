___
**Note du traducteur**

C'est la traduction du fichier [debugging-with-webstorm.md](https://github.com/avajs/ava/blob/main/docs/recipes/debugging-with-webstorm.md). Voici un [lien](https://github.com/avajs/ava/compare/b208d143ad852dc95aa8b44eed94ac1f404a25f4...main#diff-282fbdcc6c13d7c58c36f8d0f802b7a13e32fbc8b305951ff9bde13376c97e7e) vers les différences avec le master de AVA (Si en cliquant sur le lien, vous ne trouvez pas le fichier `debugging-with-webstorm.md` parmi les fichiers modifiés, vous pouvez donc en déduire que la traduction est à jour).
___
# Débogage des tests avec WebStorm

Traductions : [English](https://github.com/avajs/ava/raw/main/docs/recipes/debugging-with-webstorm.md)

**Cette recette est obsolète.**

---

Depuis la version 2016.2, [WebStorm](https://www.jetbrains.com/webstorm/) et les autres IDE de JetBrains (IntelliJ IDEA Ultimate, PHPStorm, PyCharm Professional et RubyMine avec le plugin Node.js installé) permettent de faire du débogage des tests AVA.


## Installation en utilisant Node.js

Ajoutez une nouvelle *Node.js Run/Debug configuration* : sélectionnez `Edit Configurations...` depuis la liste déroulante en haut à droite, puis cliquez `+` et sélectionnez *Node.js*.

Dans le champ `JavaScript file`, renseignez le chemin vers AVA dans le répertoire `node_modules` du projet : `node_modules/.bin/ava` sur macOS et Linux ou `node_modules/.bin/ava.cmd` sur Windows.

Dans `Application parameters` passez les options du CLI que vous utilisez et les fichiers de test que vous souhaitez déboguer, par exemple `--verbose test.js`.

Dans `Node parameters`, passez l'option `--inspect-brk` pour activer l'inspecteur de Node.

Sauvez la configuration.

## Installation en utilisant npm

Exécutez `npx @ava/init` dans le répertoire de votre projet pour ajouter AVA à votre `package.json`.

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

Dans `Node parameters`, passez `--inspect-brk`.

N'oubliez pas de sélectionner un interpréteur Node.js.

Sauvez la configuration.

## Débogage

Définissez des breakpoints dans le code.

Appuyez sur le bouton `Debug` à côté de la liste des configurations en haut à droite. Le *Debug tool window* apparaît. Une fois que breakpoint est atteint, vous pouvez évaluer les variables et parcourir le code. Lors du débogage de plusieurs fichiers de test, vous pouvez basculer entre les processus en utilisant la liste déroulante dans le volet Frames.
