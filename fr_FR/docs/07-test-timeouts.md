___
**Note du traducteur**

C'est la traduction du fichier [07-test-timeouts.md](https://github.com/avajs/ava/blob/main/docs/07-test-timeouts.md). Voici un [lien](https://github.com/avajs/ava/compare/b208d143ad852dc95aa8b44eed94ac1f404a25f4...main#diff-359d7f196adb7d5fcf884234506213692106ed86f073ed3533d718e4c89b0375) vers les différences avec le master de AVA (Si en cliquant sur le lien, vous ne trouvez pas le fichier `07-test-timeouts.md` parmi les fichiers modifiés, vous pouvez donc en déduire que la traduction est à jour).
___
# Timeouts des tests

Traductions : [English](https://github.com/avajs/ava/blob/main/docs/07-test-timeouts.md)

[![Ouvrir dans StackBlitz](https://developer.stackblitz.com/img/open_in_stackblitz.svg)](https://stackblitz.com/github/avajs/ava/tree/main/examples/timeouts?file=test.js&terminal=test&view=editor)

Le délai de AVA se comporte différemment des autres frameworks de test. AVA réinitialise un minuteur après chaque test, cela oblige les tests à s'arrêter, si aucun nouveau résultat de test est reçu dans le délai imparti. Cela peut être utilisé pour gérer les tests bloqués.

Le délai d'expiration par défaut est de 10 secondes.

Vous pouvez configurer un délai (timeout) via l'[option en ligne de commande](./05-command-line.md) `--timeout` ou dans la [configuration](./06-configuration.md). Ils peuvent être définis de manière qu'ils soient lisibles :

```console
npx ava --timeout=10s # 10 secondes
npx ava --timeout=2m # 2 minutes
npx ava --timeout=100 # 100 millisecondes
```

### `t.timeout(ms, message?)`

Les délais peuvent également être définis individuellement pour chaque test. Ces délais sont réinitialisés chaque fois qu'une assertion est faite. Le test échoue s'il faut plus de `ms` pour qu'une assertion soit faite ou que le test soit terminé.

```js
test('foo', t => {
	t.timeout(100); // 100 millisecondes
	// Ecrivez ici vos assertions
});
```

Une chaîne de caractères d'un message facultative peut être fournie. Cela peut être utile si votre test dépend d'une autre configuration qui peut ne pas avoir été achevée :

```js
test('foo', t => {
	t.timeout(100, 'Assure que la base de données a démarré'); // 100 millisecondes
	// Ecrivez ici vos assertions
});
```
