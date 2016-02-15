___
**Note du traducteur**

C'est la traduction du fichier [when-to-use-plan.md](https://github.com/sindresorhus/ava/blob/master/docs/recipes/when-to-use-plan.md). Voici un [lien](https://github.com/sindresorhus/ava/compare/559dda50034dadffecde624e505ed0282ddf2fd2...master#diff-0c25d982e94d600cb6b8e438a0e67169) vers les différences avec le master de AVA (Si en cliquant sur le lien, vous ne trouvez pas le fichier `when-to-use-plan.md` parmi les fichiers modifiés, vous pouvez donc en déduire que la traduction est à jour).
___
# Quand utiliser `t.plan()` ?

Traductions : [Español](https://github.com/sindresorhus/ava-docs/blob/master/es_ES/docs/recipes/when-to-use-plan.md), [Français](https://github.com/sindresorhus/ava-docs/blob/master/fr_FR/docs/recipes/when-to-use-plan.md), [日本語](https://github.com/sindresorhus/ava-docs/blob/master/ja_JP/docs/recipes/when-to-use-plan.md),  [Português](https://github.com/sindresorhus/ava-docs/blob/master/pt_BR/docs/recipes/when-to-use-plan.md)

Une des différences principales entre AVA et [`tap`](https://github.com/tapjs/node-tap)/[`tape`](https://github.com/substack/tape), c'est le comportement de `t.plan()`. Dans AVA, `t.plan()` est uniquement utilisé pour vérifier que le nombre assertions appelées correspond, il n'arrête pas automatiquement le test.

## Les mauvaises utilisations de `t.plan()`

Beaucoup d'utilisateurs venant de `tap`/`tape` sont habitués à utiliser `t.plan()` de manière prolifique dans chaque test. Cependant, dans AVA, nous ne considérons pas que cela soit une « bonne pratique ». Au lieu de cela, nous croyons que `t.plan()` devait être utilisé seulement dans les situations où il fournit une certaine valeur.

### Tests synchrone sans débranchements

`t.plan()` est inutile dans la plupart des tests synchrones.

```js
test(t => {
	// MAUVAIS : Il n'y a pas de débranchement ici - t.plan() est inutile
	t.plan(2);

	t.is(1 + 1, 2);
	t.is(2 + 2, 4);
});
```

`t.plan()` ne fournit aucune valeur ici, et crée une tâche supplémentaire si jamais vous décidez d'ajouter ou supprimer des assertions.

### Les promesses qui sont censées se résoudre

```js
test(t => {
	t.plan(1);

	return somePromise().then(result => {
		t.is(result, 'foo');
	});
});
```

A première vue, ce test semblent utiliser correctement `t.plan()` car un gestionnaire de promesse asynchrone est implémenté. Cependant, il y a plusieurs problèmes avec ce test :

1. `t.plan()` est sans doute utilisé ici pour se protéger contre la possibilité que `somePromise()` soit rejetée.  Mais le retour d'une promesse rejetée fera échouer de toute façon le test.

2. Il serait préférable de profiter de `async`/`await`:

```js
test(async t => {
	t.is(await somePromise(), 'foo');
});
```

## Les bonnes utilisations de `t.plan()`

`t.plan()` a de nombreuses utilisations acceptables.

### Les promesses avec un bloc `.catch()`

```js
test(t => {
	t.plan(2);

	return shouldRejectWithFoo().catch(reason => {
		t.is(reason.message, 'Hello') // Préférez t.throws(), si la seule chose qui vous importe c'est message
		t.is(reason.foo, 'bar');
	});
});
```

Ici, `t.plan()` est utilisé pour s'assurer que le code à l'intérieur du bloc `catch` soit exécuté. Dans la plupart des cas, vous devez préférer l'assertion `t.throws()`. Mais c'est une utilisation acceptable puisque `t.throws()` vous permet seulement de contrôler la propriété `message` de l'erreur.

### S'assurer qu'une implémentation catch s'exécute

```js
test(t => {
	t.plan(2);

	try {
		shouldThrow();
	} catch (err) {
		t.is(err.message, 'Hello') // Préférez t.throws(), si la seule chose qui vous importe c'est message
		t.is(err.foo, 'bar');
	}
});
```

Comme indiqué dans l'exemple `try`/`catch` ci-dessus, l'utilisation de l'assertion `t.throws()` est généralement un meilleur choix, mais il vous permet seulement de contrôler la propriété `message` de l'erreur.

### S'assurer que plusieurs callbacks sont réellement appelés

```js
test.cb(t => {
	t.plan(2);

	const callbackA = () => {
		t.pass();
		t.end();
	};

	const callbackB = () => t.pass();

	bThenA(callbackA, callbackB);
});
```

Ci-dessus, cela permet de s'assurer que `callbackB` est appelé en premier (et une seule fois), suivi de `callbackA`. Toute autre combinaison ne satisferaient pas `plan`.

### Tests avec un débranchement

Dans la plupart des cas, c'est une mauvaise idée d'utiliser un débranchement complexe à l'intérieur de vos tests. Une exception notable, c'est pour les tests qui sont générés automatiquement (peut-être venant d'un document JSON). Ci-dessous `t.plan()` est utilisé pour s'assurer de l'exactitude du fichier JSON :

```js
const testData = require('./fixtures/test-definitions.json');

testData.forEach(testDefinition => {
	test(t => {
		const result = functionUnderTest(testDefinition.input);

		// testDefinition doit attendre `foo` ou `bar` mais pas les deux
		t.plan(1);

		if (testDefinition.foo) {
			t.is(result.foo, testDefinition.foo);
		}

		if (testDefinition.bar) {
			t.is(result.bar, testDefinition.foo);
		}
	});
});
```

## Conclusion

`t.plan()` a beaucoup d'utilisations valables, mais il ne doit pas être utilisé de façon hasardeuse. Une bonne règle est de l'utiliser chaque fois que votre *test* est compliqué, c'est souvent le cas pour le flux de code. Les tests avec des assertions à l'intérieur des callbacks, d'implémentations `if`/`then`, des boucles `for`/`while` et (dans plusieurs cas) dans des blocs `try`/`catch`, sont tous des bons candidats pour `t.plan()`.
