___
**Note du traducteur**

C'est la traduction du fichier [when-to-use-plan.md](https://github.com/avajs/ava/blob/main/docs/recipes/when-to-use-plan.md). Voici un [lien](https://github.com/avajs/ava/compare/b208d143ad852dc95aa8b44eed94ac1f404a25f4...main#diff-75a61a239320caf424492802030515ad602bf2b7a926de429e8fb907de2b6794) vers les différences avec le master de AVA (Si en cliquant sur le lien, vous ne trouvez pas le fichier `when-to-use-plan.md` parmi les fichiers modifiés, vous pouvez donc en déduire que la traduction est à jour).
___
# Quand utiliser `t.plan()` ?

Traductions : [English](https://github.com/avajs/ava/raw/main/docs/recipes/when-to-use-plan.md), [Español](https://github.com/avajs/ava-docs/blob/main/es_ES/docs/recipes/when-to-use-plan.md), [Italiano](https://github.com/avajs/ava-docs/blob/main/it_IT/docs/recipes/when-to-use-plan.md),  [日本語](https://github.com/avajs/ava-docs/blob/main/ja_JP/docs/recipes/when-to-use-plan.md),  [Português](https://github.com/avajs/ava-docs/blob/main/pt_BR/docs/recipes/when-to-use-plan.md), [Русский](https://github.com/avajs/ava-docs/blob/main/ru_RU/docs/recipes/when-to-use-plan.md), [简体中文](https://github.com/avajs/ava-docs/blob/main/zh_CN/docs/recipes/when-to-use-plan.md)

Une des différences principales entre AVA et [`tap`](https://github.com/tapjs/node-tap)/[`tape`](https://github.com/substack/tape), c'est le comportement de `t.plan()`. Dans AVA, `t.plan()` est uniquement utilisé pour vérifier que le nombre assertions appelées correspond, il n'arrête pas automatiquement le test.

## Les mauvaises utilisations de `t.plan()`

Beaucoup d'utilisateurs venant de `tap`/`tape` sont habitués à utiliser `t.plan()` de manière prolifique dans chaque test. Cependant, dans AVA, nous ne considérons pas que cela soit une « bonne pratique ». Au lieu de cela, nous croyons que `t.plan()` devait être utilisé seulement dans les situations où il fournit une certaine valeur.

### Tests synchrone sans débranchements

`t.plan()` est inutile dans la plupart des tests synchrones.

```js
test('simple sums', t => {
	// MAUVAIS : Il n'y a pas de débranchement ici - t.plan() est inutile
	t.plan(2);

	t.is(1 + 1, 2);
	t.is(2 + 2, 4);
});
```

`t.plan()` ne fournit aucune valeur ici, et crée une tâche supplémentaire si jamais vous décidez d'ajouter ou supprimer des assertions.

### Les promesses qui sont censées se résoudre

```js
test('gives foo', t => {
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
test('gives foo', async t => {
	t.is(await somePromise(), 'foo');
});
```

### Les promesses avec un bloc `.catch()`

```js
test('rejects with foo', t => {
	t.plan(2);

	return shouldRejectWithFoo().catch(reason => {
		t.is(reason.message, 'Hello');
		t.is(reason.foo, 'bar');
	});
});
```

Ici, l'utilisation de `t.plan ()` vise à garantir que le code à l'intérieur du bloc `catch` soit exécuté.
Au lieu de cela, vous devriez profiter de `t.throwsAsync` et `async`/`await`, car cela conduit à rendre le code plus simple ce qui est plus facile à suivre :

```js
test('rejects with foo', async t => {
	const reason = await t.throwsAsync(shouldRejectWithFoo());
	t.is(reason.message, 'Hello');
	t.is(reason.foo, 'bar');
});
```

### S'assurer qu'une implémentation catch s'exécute

```js
test('throws', t => {
	t.plan(2);

	try {
		shouldThrow();
	} catch (err) {
		t.is(err.message, 'Hello');
		t.is(err.foo, 'bar');
	}
});
```

Comme indiqué dans l'exemple précédent, l'utilisation de l'assertion `t.throws()` avec `async`/`await` est un meilleur choix.

## Les bonnes utilisations de `t.plan()`

`t.plan()` fournit une valeur dans les cas suivants.

### Tests avec un débranchement

Dans la plupart des cas, c'est une mauvaise idée d'utiliser un débranchement complexe à l'intérieur de vos tests. Une exception notable, c'est pour les tests qui sont générés automatiquement (peut-être venant d'un document JSON). Ci-dessous `t.plan()` est utilisé pour s'assurer de l'exactitude du fichier JSON :

```js
import fs from 'fs';
import path from 'path';

const testData = JSON.parse(fs.readFileSync(new URL('./fixtures/test-definitions.json', import.meta.url)));

for (const testDefinition of testData) {
	test('foo ou bar', t => {
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
}
```

## Conclusion

`t.plan()` a beaucoup d'utilisations valables, mais il ne doit pas être utilisé de façon hasardeuse. Une bonne règle est de l'utiliser chaque fois que votre *test* est compliqué, c'est souvent le cas pour le flux de code. Les tests avec des assertions à l'intérieur des callbacks, d'implémentations `if`/`then`, des boucles `for`/`while` et (dans plusieurs cas) dans des blocs `try`/`catch`, sont tous des bons candidats pour `t.plan()`.
