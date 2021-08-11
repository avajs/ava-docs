___
**Note du traducteur**

C'est la traduction du fichier [debugging-with-vscode.md](https://github.com/avajs/ava/blob/main/docs/recipes/debugging-with-vscode.md). Voici un [lien](https://github.com/avajs/ava/compare/4953457277a355e231beaa11431eb58a209ae7fc...main#diff-a3927068f3a0ffbbdf1b02fbd401b146) vers les différences avec le master de AVA (Si en cliquant sur le lien, vous ne trouvez pas le fichier `debugging-with-vscode.md` parmi les fichiers modifiés, vous pouvez donc en déduire que la traduction est à jour).
___
# Débogage des tests avec Visual Studio Code

Traductions : [English](https://github.com/avajs/ava/raw/main/docs/recipes/debugging-with-vscode.md)

**Cette recette décrit la nouvelle commande `inspect` dans la prochaine version de AVA 3. Consultez à la place la documentation de [AVA 2](https://github.com/avajs/ava-docs/blob/abdd141b225a15b504c31741c61e081026991e35/fr_FR/docs/recipes/debugging-with-vscode.md).**

Vous pouvez déboguer vos tests en utilisant [Visual Studio Code](https://code.visualstudio.com/).

## Création d'une configuration de lancement

1. Ouvrez un espace de travail pour votre projet.
1. Dans la barre latérale, cliquez sur le bouton *Debug*.
1. Créez un fichier `launch.json`.
1. Sélectionnez l'environnement Node.js.
1. Ajoutez ce qui suit à l'objet `configurations` :

  ```json
  {
    "type": "node",
    "request": "launch",
    "name": "Déboguer le fichier de test AVA",
    "runtimeExecutable": "${workspaceFolder}/node_modules/.bin/ava",
    "runtimeArgs": [
      "debug",
      "--break",
      "${file}"
    ],
    "port": 9229,
    "outputCapture": "std",
    "skipFiles": [
      "<node_internals>/**/*.js"
    ]
  }
  ```
1. Enregistrez vos modifications dans le fichier `launch.json`.

## Utilisation du débogueur

Ouvrez le ou les fichiers que vous souhaitez déboguer. Vous pouvez définir des points d'arrêt ou utiliser le mot clé `debugger`.

Maintenant, *avec un fichier de test ouvert*, à partir du menu *Debug* exécutez la configuration *Déboguer le fichier de test AVA*.

## Débogage en série

Par défaut, AVA exécute les tests simultanément. Cela peut compliquer le débogage. Ajoutez une configuration avec l'argument `--serial` afin qu'AVA n'exécute qu'un seul test à la fois :

```json
{
  "type": "node",
  "request": "launch",
  "name": "Déboguer le fichier de test AVA",
  "runtimeExecutable": "${workspaceFolder}/node_modules/.bin/ava",
  "runtimeArgs": [
    "debug",
    "--break",
    "--serial",
    "${file}"
  ],
  "port": 9229,
  "outputCapture": "std",
  "skipFiles": [
    "<node_internals>/**/*.js"
  ]
}
```

*Notez que si vos tests ne sont pas correctement isolés, certains échecs sur un test peuvent ne pas apparaître lors de l'exécution des tests en série.*
