___
**Note du traducteur**

C'est la traduction du fichier [07-test-timeouts.md](https://github.com/avajs/ava/blob/master/docs/07-test-timeouts.md). Voici un [lien](https://github.com/avajs/ava/compare/f2172526ae03490a548f6ef30a81c8212c280c58...master#diff-ee554218ce48f7ac6fbeab40ba17919f) vers les différences avec le master de AVA (Si en cliquant sur le lien, vous ne trouvez pas le fichier `07-test-timeouts.md` parmi les fichiers modifiés, vous pouvez donc en déduire que la traduction est à jour).
___
# Timeouts des tests

Traductions : [English](https://github.com/avajs/ava/blob/master/docs/07-test-timeouts.md)

Le délai de AVA se comporte différemment des autres frameworks de test. AVA réinitialise un minuteur après chaque test, cela oblige les tests à s'arrêter, si aucun nouveau résultat de test est reçu dans le délai imparti. Cela peut être utilisé pour gérer les tests bloqués.

Le délai d'expiration par défaut est de 10 secondes.

Vous pouvez configurer un délai (timeout) via l'[option en ligne de commande](./05-command-line.md) `--timeout` ou dans la [configuration](./06-configuration.md). Ils peuvent être définis de manière qu'ils soient lisibles :

```console
npx ava --timeout=10s # 10 secondes
npx ava --timeout=2m # 2 minutes
npx ava --timeout=100 # 100 millisecondes
```

Les timeouts peuvent également être définis individuellement pour chaque test. Ces délais sont réinitialisés chaque fois qu'une assertion est faite.

```js
test('foo', t => {
	t.timeout(100); // 100 milliseconds
	// Ecrivez ici vos assertions
});
```
