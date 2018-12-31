___
**Note du traducteur**

C'est la traduction du fichier [07-test-timeouts.md](https://github.com/avajs/ava/blob/master/docs/07-test-timeouts.md). Voici un [lien](https://github.com/avajs/ava/compare/1ba31d8f3e1b6aa645f10ee5fc5b8c84a366c40d...master#diff-ee554218ce48f7ac6fbeab40ba17919f) vers les différences avec le master de AVA (Si en cliquant sur le lien, vous ne trouvez pas le fichier `07-test-timeouts.md` parmi les fichiers modifiés, vous pouvez donc en déduire que la traduction est à jour).
___
# Timeouts des tests

Le délai de AVA se comporte différemment des autres frameworks de test. AVA réinitialise un minuteur après chaque test, cela oblige les tests à s'arrêter, si aucun nouveau résultat de test est reçu dans le délai imparti. Cela peut être utilisé pour gérer les tests bloqués.

Vous pouvez configurer un délai (timeout) via l'[option en ligne de commande](./05-command-line.md) `--timeout` ou dans la [configuration](./06-configuration.md).

Vous pouvez définir des délais qui soient lisibles :

```console
npx ava --timeout=10s # 10 secondes
npx ava --timeout=2m # 2 minutes
npx ava --timeout=100 # 100 millisecondes
```
