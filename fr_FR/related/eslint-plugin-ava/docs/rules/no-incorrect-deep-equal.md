___
**Note du traducteur**

C'est la traduction du fichier [no-incorrect-deep-equal.md](https://github.com/avajs/eslint-plugin-ava/blob/main/docs/rules/no-incorrect-deep-equal.md). Voici un [lien](https://github.com/avajs/eslint-plugin-ava/compare/dee1802d39e22aec0915d5067062356f5abfbd84...main#diff-07f9cd788860446801712fd4887dc16fa82d43bdb9885d67d15f7cd5710f4dd4) vers les différences avec le master de eslint-plugin-ava (Si en cliquant sur le lien, vous ne trouvez pas le fichier `no-incorrect-deep-equal` parmi les fichiers modifiés, vous pouvez donc en déduire que la traduction est à jour).
___
# Éviter d'utiliser `deepEqual` avec des primitives

Traductions : [English](https://github.com/avajs/eslint-plugin-ava/blob/main/docs/rules/no-incorrect-deep-equal.md)

Les assertions `deepEqual` et `notDeepEqual` sont inutiles lors de la comparaison de primitives. Utilisez à la place `is` ou `not`.

Cette règle est réparable.

## Échoue

```js
t.deepEqual(expression, 'foo');
t.deepEqual(expression, 1);
t.deepEqual(expression, `foo${bar}`);
t.deepEqual(expression, null);
t.deepEqual(expression, undefined);
t.notDeepEqual(expression, undefined);
```

## Passe

```js
t.is(expression, 'foo');

t.deepEqual(expression, otherExpression);
t.deepEqual(expression, {});
t.deepEqual(expression, []);
t.notDeepEqual(expression, []);
```
