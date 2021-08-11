___
**Note du traducteur**

C'est la traduction du fichier [03-assertions.md](https://github.com/avajs/ava/blob/main/docs/03-assertions.md). Voici un [lien](https://github.com/avajs/ava/compare/7f99aef61f3aed2389ca9407115ad4c9aecada92...master#diff-35a3a6b97b8ddb82e64de6c59a97bf4d) vers les différences avec le master de AVA (Si en cliquant sur le lien, vous ne trouvez pas le fichier `03-assertions.md` parmi les fichiers modifiés, vous pouvez donc en déduire que la traduction est à jour).
___
# Assertions

Traductions : [English](https://github.com/avajs/ava/raw/main/docs/03-assertions.md)

Les assertions sont incluses dans l'[objet d'exécution](./02-execution-context.md) fourni à chaque implémentation de test :

```js
test('les licornes sont truthy (vraies)', t => {
	t.truthy('licornes'); // Assertion
});
```

Les assertions sont liées à leur test de sorte que vous puissiez les affecter à une variable ou les transmettre :

```js
test('les licornes sont truthy (vraies)', t => {
	const truthy = t.truthy;
	truthy('licornes');
});
```

Si plusieurs échecs d’assertion sont rencontrés au cours d’un même test, AVA n’affiche que le *premier*.

## Planification d'assertion

Une assertion **plan**-ifiée assure que les tests passent uniquement quand un certain nombre d'assertions ont été exécutées. Elle vous aidera à traiter les cas où les tests sortent trop tôt. Elle mettra aussi les tests en échec si trop d'assertions sont exécutées, ce qui peut être utile si vous avez des assertions à l'intérieur des callbacks ou des boucles.

Si vous ne spécifiez pas une assertion planifiée, votre test échouera toujours si aucune assertion n'est exécutée. Définissez l'option `failWithoutAssertions` à `false` dans la [configuration de AVA dans le `package.json`](./06-configuration.md) pour désactiver ce comportement.

Il faut savoir que, contrairement à [`tap`](https://www.npmjs.com/package/tap) et [`tape`](https://www.npmjs.com/package/tape), AVA *n'arrête pas* automatiquement un test lorsque le nombre d'assertion prévu est atteint.

Ces exemples se traduiront par un test réussi :

```js
test('se résout avec 3', t => {
	t.plan(1);

	return Promise.resolve(3).then(n => {
		t.is(n, 3);
	});
});

test.cb('appelle le callback', t => {
	t.plan(1);

	someAsyncFunction(() => {
		t.pass();
		t.end();
	});
});
```

Ce n'est pas le cas de ces exemples :

```js
test('boucle deux fois', t => {
	t.plan(2);

	for (let i = 0; i < 3; i++) {
		t.true(i < 3);
	}
}); // Echec, 3 assertions de trop sont exécutées

test('appelle le callback de manière synchrone', t => {
	t.plan(1);

	someAsyncFunction(() => {
		t.pass();
	});
}); // Echec, le test se termine de façon synchrone avant que l'assertion soit exécutée
```

## Assertions ignorées

Toute assertion peut être ignorée en utilisant le modificateur `skip`. Les assertions ignorées sont encore comptées, donc il n'y a pas besoin de changer le nombre d'assertion dans `plan`.

```js
test('passe une assertion', t => {
	t.plan(2);
	t.is.skip(foo(), 5); // Pas besoin de changer le nombre d'assertion dans `plan`.
	t.is(1, 1);
});
```

## Messages d'assertions améliorés

L'activation de [Babel](./recipes/babel.md) activera aussi [`power-assert`](https://github.com/power-assert-js/power-assert), cela vous donne des messages d'assertion plus clairs.

Prenons cet exemple, en utilisant la [bibliothèque `assert`](https://nodejs.org/api/assert.html) standard  de Node.

```js
const a = /foo/;
const b = 'bar';
const c = 'baz';
require('assert').ok(a.test(b) || b === c);
```

Si vous collez ceci dans un REPL de Node, il retournera :

```
AssertionError: false == true
```

Cependant avec l'assertion `assert` de AVA, ce test  :

```js
test('assertions améliorées', t => {
	const a = /foo/;
	const b = 'bar';
	const c = 'baz';
	t.true(a.test(b) || b === c);
});
```

Affichera :

```
6:   const c = 'baz';
7:   t.assert(a.test(b) || b === c);
8: });

Value is not truthy:

false

a.test(b) || b === c
=> false

b === c
=> false

c
=> 'baz'

b
=> 'bar'

a.test(b)
=> false

b
=> 'bar'

a
=> /foo/
```

## Personnaliser les assertions

Vous pouvez utiliser une bibliothèque d'assertion qui remplace ou s'ajoute à celui qui est fourni, à condition qu'il renvoie des exceptions lorsque l'assertion échoue.

Cela ne vous donnera pas une aussi bonne expérience que celle obtenue avec les [assertions intégrées](#assertions), et vous ne serez pas en mesure d'utiliser la [planification d'assertion](#planification-d-assertion) ([voir #25](https://github.com/avajs/ava/issues/25)).

Vous devrez configurer AVA pour que les tests n'échouent pas si aucune assertion n'est exécutée, car AVA ne peut pas dire si les assertions personnalisées passent. Définissez l'option `failWithoutAssertions` à `false` dans la [configuration de AVA dans le `package.json`](./06-configuration.md).

```js
const assert = require('assert');

test('assertion personnalisée', t => {
	assert(true);
});
```

## Assertions intégrées

### `.pass([message])`

L'assertion passe.

### `.fail([message])`

L'assertion échoue.

### `.assert(value, [message])`

Affirme que `value` est truthy. C'est [`power-assert`](#messages-dassertions-améliorés) qui est activé.

### `.truthy(value, [message])`

Affirme que `value` est truthy.

### `.falsy(value, [message])`

Affirme que `value` est falsy.

### `.true(value, [message])`

Affirme que `value` est à `true`.

### `.false(value, [message])`

Affirme que `value` est à `false`.

### `.is(value, expected, [message])`

Affirme que `value` est le même que `expected`. Ceci est basé sur [`Object.is()`](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/Object/is).

### `.not(value, expected, [message])`

Affirme que `value` n'est pas le même que `expected`. Ceci est basé sur [`Object.is()`](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/Object/is).

### `.deepEqual(value, expected, [message])`

Affirme que `value` est profondément égale à `expected`. Consulter [Concordance](https://github.com/concordancejs/concordance) pour plus de détails. Cela fonctionne avec les [éléments de React et `react-test-renderer`](https://github.com/concordancejs/react).

### `.notDeepEqual(value, expected, [message])`

Affirme que `value` n'est pas profondément égale à `expected`. L'inverse de `.deepEqual()`.

### `.throws(fn, [expectation, [message]])`

Affirme qu'une erreur est levée. `fn` doit être une fonction qui devrait lever une erreur. La valeur levée *doit* être une erreur. Elle est retournée afin que vous puissiez lancer d'autres assertions.

`expectation` peut être un objet avec une ou plusieurs des propriétés suivantes :

* `instanceOf` : un constructeur, l'erreur levée doit être une "instance de"
* `is` : l'erreur levée doit être strictement égale à `expectation.is`
* `message` : soit une chaîne qui est comparée au message de l'erreur levée, ou une expression régulière qui correspond à ce message
* `name` : la valeur `.name` attendue de l'erreur levée
* `code` : la valeur `.code` attendue de l'erreur levée

`expectation` n'a pas besoin d'être précisé. Si vous n'en avez pas besoin mais que vous voulez définir un message d'assertion, vous devez spécifier `null`.

Exemple :

```js
const fn = () => {
	throw new TypeError('🦄');
};

test('throws', t => {
	const error = t.throws(() => {
		fn();
	}, {instanceOf: TypeError});

	t.is(error.message, '🦄');
});
```

### `.throwsAsync(thrower, [expectation, [message]])`

Affirme qu'une erreur est levée. `thrower` peut être une fonction async qui devrait lever une erreur ou une promesse qui devrait échouée. Cette affirmation doit être attendue (await).

La valeur levée *doit* être une erreur. Elle est renvoyée afin que vous puissiez exécuter d'autres assertions.

`expectation` peut être un objet avec une ou plusieurs des propriétés suivantes :

* `instanceOf` : un constructeur, l'erreur levée doit être une "instance de"
* `is` : l'erreur levée doit être strictement égale à `expectation.is`
* `message` : soit une chaîne qui est comparée au message de l'erreur levée, ou une expression régulière qui correspond à ce message
* `name` : la valeur `.name` attendue de l'erreur levée
* `code` : la valeur `.code` attendue de l'erreur levée

`expectation` n'a pas besoin d'être précisé. Si vous n'en avez pas besoin mais que vous voulez définir un message d'assertion, vous devez spécifier `null`.

Exemple:

```js
test('throws', async t => {
	await t.throwsAsync(async () => {
		throw new TypeError('🦄');
	}, {instanceOf: TypeError, message: '🦄'});
});
```

```js
const promise = Promise.reject(new TypeError('🦄'));

test('rejects', async t => {
	const error = await t.throwsAsync(promise);
	t.is(error.message, '🦄');
});
```

### `.notThrows(fn, [message])`

Affirme qu'aucune erreur n'est levée. `fn` doit être une fonction qui ne doit pas être levée.

### `.notThrowsAsync(nonThrower, [message])`

Affirme qu'aucune erreur n'est levée. `nonThrower` peut être une fonction async qui ne devrait pas lever une erreur ou une promesse qui devrait être résolue.

Comme l'assertion `.throwsAsync()`, vous devez attendre que l'assertion se termine :

```js
test('resolves', async t => {
	await t.notThrowsAsync(promise);
});
```

### `.regex(contents, regex, [message])`

Affirme que `contents` correspond à `regex`.

### `.notRegex(contents, regex, [message])`

Affirme que `contents` ne correspond pas à `regex`.

### `.snapshot(expected, [message])`
### `.snapshot(expected, [options], [message])`

Compare la valeur `expected` avec un instantané enregistré auparavant. Les instantanés sont conservés pour chaque test, donc assurez-vous de donner à vos tests des titres uniques. Sinon, passez un objet `options` pour sélectionner un instantané spécifique, par exemple `{id: 'mon snapshot'}`.

Les assertions d'instantanés ne peuvent pas être ignorées (skip) lors de la mise à jour des instantanés.
