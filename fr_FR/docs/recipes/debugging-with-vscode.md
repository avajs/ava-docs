___
**Note du traducteur**

C'est la traduction du fichier [debugging-with-vscode.md](https://github.com/avajs/ava/blob/master/docs/recipes/debugging-with-vscode.md). Voici un [lien](https://github.com/avajs/ava/compare/0a5fe429ca37c025a15c5af919827436cc413abc...master#diff-a3927068f3a0ffbbdf1b02fbd401b146) vers les différences avec le master de AVA (Si en cliquant sur le lien, vous ne trouvez pas le fichier `debugging-with-vscode.md` parmi les fichiers modifiés, vous pouvez donc en déduire que la traduction est à jour).
___
# Débogage des tests avec Visual Studio Code

Traductions : [English](https://github.com/avajs/ava/blob/master/docs/recipes/debugging-with-vscode.md)

## Installation

Dans la barre latérale, cliquez sur `Debug`.

Ajoutez une nouvelle configuration dans le menu déroulant à côté du bouton vert `Debug` : `Add configuration`. Cela ouvrira `launch.json` avec toutes les configurations de débogage.

Ajoutez à l'objet `configurations` ce qui suit :

```json
{
	"type": "node",
	"request": "launch",
	"name": "Exécute les tests AVA",
	"program": "${workspaceFolder}/node_modules/ava/profile.js",
	"args": [
	  "${file}"
	],
	"skipFiles": [
		"<node_internals>/**/*.js"
	]
}
```

Sauvez cette configuration après son ajout.

## Débogage

> **Remarque :** Le fichier que vous voulez déboguer doit être ouvert et actif

> **Remarque :** Les points d'arrêt dans VSCode sont parfois un peu bogués (surtout avec du code async). `debugger;` fonctionne toujours bien.

Définissez des points d'arrêt dans le code **ou** écrivez `debugger;` à l'endroit où il devrait s'arrêter.

Appuyez sur le bouton `Debug` à côté de la liste des configurations en haut à gauche dans la vue `Debug`. Une fois que le point d'arrêt est atteint, vous pouvez évaluer les variables et suivre le code pas à pas.

## Débogage en série

Par défaut, AVA exécute les tests simultanément. Cela peut compliquer le débogage. Ajoutez une configuration avec l'argument `--serial` afin qu'AVA n'exécute qu'un seul test à la fois :

```json
{
	"type": "node",
	"request": "launch",
	"name": "Exécute les tests AVA en série",
	"program": "${workspaceFolder}/node_modules/ava/profile.js",
	"args": [
	  "${file}",
	  "--serial"
	],
	"skipFiles": [
		"<node_internals>/**/*.js"
	]
}
```

*Notez que si vos tests ne sont pas correctement isolés, certains échecs sur un test peuvent ne pas apparaître lors de l'exécution des tests en série.*
