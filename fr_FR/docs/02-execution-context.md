___
**Note du traducteur**

C'est la traduction du fichier [02-execution-context.md](https://github.com/avajs/ava/blob/main/docs/02-execution-context.md). Voici un [lien](https://github.com/avajs/ava/compare/b208d143ad852dc95aa8b44eed94ac1f404a25f4...main#diff-bc6837223adbc4778415cdfd8ae668260afd34f3a562e5070cc25f536c5ded0a) vers les différences avec le master de AVA (Si en cliquant sur le lien, vous ne trouvez pas le fichier `02-execution-context.md` parmi les fichiers modifiés, vous pouvez donc en déduire que la traduction est à jour).
___
# Contexte d'exécution (argument `t`)

Traductions : [English](https://github.com/avajs/ava/raw/main/docs/02-execution-context.md)

Chaque test ou hook est appelé avec un contexte d'exécution. Par convention, il s'appelle `t`.

```js
import test from 'ava';

test('mon test passe', t => {
	t.pass();
});
```

Chaque test ou hook reçoit un objet différent. Il contient les [assertions](./03-assertions.md) ainsi que les méthodes et les propriétés énumérées ci-dessous.

## `t.title`

Le titre du test.

## `t.context`

Contient l'état partagé des hooks.

## `t.passed`

Lorsqu'il est utilisé dans les hooks `test.aftereach ()` ou `test.aftereach.always ()`, cela vous indique si le test est passé. Lorsqu'il est utilisé dans un test lui-même (y compris les fonctions de nettoyage), cela reste `true` jusqu'à ce qu'une assertion échoue, que le test s'est terminé par une erreur, ou qu'une fonction de nettoyage a provoqué une erreur. Cette valeur n'a aucune signification dans d'autres hooks.

## `t.log(...values)`

Enregistre les valeurs contextuellement à côté du résultat du test au lieu de les imprimer immédiatement dans `stdout`. Se comporte un peu comme `console.log`, mais sans le support des littéraux de gabarits.

## `t.plan(count)`

Planifie le nombre d'assertions dans le test. Le test échouera si le nombre d'assertions réel ne correspond pas au nombre d'assertions planifiées. Voir [planification des assertions](./03-assertions.md#planification-d-assertion).

## `t.teardown(fn)`

Enregistre la fonction `fn` qui sera exécutée après la fin du test. Vous pouvez enregistrer plusieurs fonctions. Elles seront exécutées dans l'ordre inverse, de sorte que la dernière fonction enregistrée sera exécutée en premier. Vous pouvez utiliser des fonctions asynchrones : une seule sera exécutée à la fois.

Vous ne pouvez pas effectuer d'assertions en utilisant l'objet `t` ou enregistrer des fonctions supplémentaires à l'intérieur de `fn`.

Vous ne pouvez pas non plus utiliser `t.teardown()` dans les hooks.

## `t.timeout(ms)`

Définit un délai d'expiration pour le test, en millisecondes. Le test échouera si ce délai est dépassé. Le délai d'attente est réinitialisé chaque fois qu'une assertion est faite.
