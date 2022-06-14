___
**Note du traducteur**

C'est la traduction du fichier [debugging-with-vscode.md](https://github.com/avajs/ava/blob/main/docs/recipes/debugging-with-vscode.md). Voici un [lien](https://github.com/avajs/ava/compare/91f5254d7bfb6c658d0e89f48a852b92b70e31da...main#diff-1e7f3c0d41918f918d18ac976d8ac2abeb66daea695023d07ed56df2e7c72da4) vers les différences avec le master de AVA (Si en cliquant sur le lien, vous ne trouvez pas le fichier `debugging-with-vscode.md` parmi les fichiers modifiés, vous pouvez donc en déduire que la traduction est à jour).
___
# Débogage des tests avec Visual Studio Code

Traductions : [English](https://github.com/avajs/ava/raw/main/docs/recipes/debugging-with-vscode.md)

Vous pouvez déboguer vos tests en utilisant [Visual Studio Code](https://code.visualstudio.com/).

## Débogage avec le terminal de débogage

Vous pouvez utiliser le « Terminal de débogage JavaScript » de VS Code pour déboguer automatiquement l'exécution d'AVA en ligne de commande.

1. Depuis la palette de commande (<kbd>F1</kbd> ou <kbd>command + shift + p</kbd> / <kbd>control + shift + p</kbd>), exécutez `Debug: JavaScript Debug Terminal`
2. Exécutez `npx ava` dans le terminal

## Création d'une configuration de lancement

Vous pouvez également créer une configuration de lancement, ce qui facilite le débogage des fichiers de test individuels.

1. Ouvrez un espace de travail pour votre projet.
1. Dans la barre latérale, cliquez sur le bouton *Debug*.
1. Créez un fichier `launch.json`.
1. Sélectionnez l'environnement Node.js.
1. Ajoutez ce qui suit au tableau `configurations` et sauvegardez-le :

  ```json
  {
    "type": "node",
    "request": "launch",
    "name": "Déboguer le fichier de test AVA",
    "program": "${workspaceFolder}/node_modules/ava/entrypoints/cli.mjs",
    "args": [
      "${file}"
    ],
    "outputCapture": "std",
		"console": "integratedTerminal", // facultatif
    "skipFiles": [
      "<node_internals>/**/*.js"
    ]
  }
  ```

### Utilisation du débogueur

Ouvrez le ou les fichiers que vous souhaitez déboguer. Vous pouvez définir des points d'arrêt ou utiliser le mot clé `debugger`.

Maintenant, *avec un fichier de test ouvert*, à partir du menu *Debug* exécutez la configuration *Déboguer le fichier de test AVA*.

### Débogage des tests précompilés

Si vous compilez vos fichiers de test dans un autre répertoire et exécutez les tests *depuis* ce répertoire, la configuration ci-dessus ne fonctionnera pas.

En supposant que les noms de vos fichiers de test sont uniques, vous pouvez essayer à la place la configuration suivante. Cela suppose que la sortie de la compilation est écrite dans le répertoire `build`. Ajustez comme il convient :


```json
{
  "type": "node",
  "request": "launch",
  "name": "Debug AVA test file",
  "program": "${workspaceFolder}/node_modules/ava/entrypoints/cli.mjs",
  "args": [
    "build/**/${fileBasenameNoExtension}.*"
  ],
  "outputCapture": "std",
	"console": "integratedTerminal", // facultatif
  "skipFiles": [
    "<node_internals>/**/*.js"
  ]
}
```

## Débogage en série

Par défaut, AVA exécute les tests simultanément. Cela peut compliquer le débogage. A la place, faites en sorte que AVA n'exécute qu'un seul test à la fois.

*Notez que, si vos tests ne sont pas correctement isolés, certains échecs de test peuvent ne pas apparaître lorsque vous exécutez les tests en série.*

Si vous utilisez le terminal de débogage, assurez-vous d'invoquer AVA avec `npx ava --serial`.

Ou, si vous utilisez une configuration de lancement, ajoutez l'argument `--serial` :

```json
{
  "type": "node",
  "request": "launch",
  "name": "Déboguer le fichier de test AVA",
  "program": "${workspaceFolder}/node_modules/ava/entrypoints/cli.mjs",
  "args": [
    "--serial",
    "${file}"
  ],
  "outputCapture": "std",
	"console": "integratedTerminal", // facultatif
  "skipFiles": [
    "<node_internals>/**/*.js"
  ]
}
```
