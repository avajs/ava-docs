___
**Note du traducteur**

C'est la traduction du fichier [assertion-message.md](https://github.com/sindresorhus/eslint-plugin-ava/blob/master/docs/rules/assertion-message.md). Voici un [lien](https://github.com/sindresorhus/eslint-plugin-ava/compare/216cd688cded0f2b79f3f652dc2eb43353f08fc4...master#diff-0b23fa0b3ae6c1f427b3c0f3331ea500) vers les différences avec le master de eslint-plugin-ava (Si en cliquant sur le lien, vous ne trouvez pas le fichier `assertion-message` parmi les fichiers modifiés, vous pouvez donc en déduire que la traduction est à jour).
___
# Imposer ou interdir les messages d'assertion

Traductions : [English](https://github.com/sindresorhus/eslint-plugin-ava/blob/master/docs/rules/assertion-message.md)

Les messages d'assertion sont des arguments facultatifs, qui peuvent être fournis lors de l'appel d'une assertion, pour améliorer le message d'erreur si l'assertion échoue. Cette règle impose ou interdit l'utilisation de ces messages.


## Échoue

```js
import test from 'ava';

/* eslint ava/assertion-message: ["error", "always"] */
test(t => {
	t.true(array.indexOf(value) !== -1);
});

/* eslint ava/assertion-message: ["error", "never"] */
test(t => {
	t.true(array.indexOf(value) !== -1, 'la valeur n\'est pas dans le tableau');
});
```


## Passe

```js
import test from 'ava';

/* eslint ava/assertion-message: ["error", "always"] */
test(t => {
	t.true(array.indexOf(value) !== -1, 'la valeur n\'est pas dans le tableau');
});

/* eslint ava/assertion-message: ["error", "never"] */
test(t => {
	t.true(array.indexOf(value) !== -1);
});
```

## Options

La règle prend une option, un string, qui peut être soit `"always"` (toujours) ou `"never"` (jamais). La valeur par défaut est `"always"`.

Vous pouvez définir l'option dans la configuration comme ceci :

```js
"ava/assertion-message": ["error", "always"]
```
