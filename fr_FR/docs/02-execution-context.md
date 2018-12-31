___
**Note du traducteur**

C'est la traduction du fichier [02-execution-context.md](https://github.com/avajs/ava/blob/master/docs/02-execution-context.md). Voici un [lien](https://github.com/avajs/ava/compare/1ba31d8f3e1b6aa645f10ee5fc5b8c84a366c40d...master#diff-fdc3f6412c8049ee065ab49994dc0879) vers les différences avec le master de AVA (Si en cliquant sur le lien, vous ne trouvez pas le fichier `02-execution-context.md` parmi les fichiers modifiés, vous pouvez donc en déduire que la traduction est à jour).
___
# Contexte d'exécution (argument `t`)

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

## `t.plan(count)`

Planifie le nombre d'assertions dans le test. Le test échouera si le nombre d'assertions réel ne correspond pas au nombre d'assertions planifiées. Voir [planification des assertions](./03-assertions.md#planification-d-assertion).

## `t.end()`

Fin du test. Fonctionne uniquement avec `test.cb()`.

## `t.log(...values)`

Journalise les valeurs de manière contextuelle à côté du résultat du test au lieu de les imprimer immédiatement dans `stdout`. Se comporte un peu comme `console.log`, mais sans prise en charge pour les jetons de l’espace réservé.
