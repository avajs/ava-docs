___
**Note du traducteur**

C'est la traduction du fichier [03-assertions.md](https://github.com/avajs/ava/blob/main/docs/03-assertions.md). Voici un [lien](https://github.com/avajs/ava/compare/b208d143ad852dc95aa8b44eed94ac1f404a25f4...main#diff-491b7f8141be104f0e85aed88fc1e100b192c25883c5f293457e5f85a299020c) vers les différences avec le master de AVA (Si en cliquant sur le lien, vous ne trouvez pas le fichier `03-assertions.md` parmi les fichiers modifiés, vous pouvez donc en déduire que la traduction est à jour).
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

Les assertions retournent un booléen indiquant si elles ont réussi. Vous pouvez utiliser ceci pour revenir plus tôt d'un test. Notez que cela ne s'applique pas aux assertions "throws" et `snapshot()`.

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

## Personnaliser les assertions

Vous pouvez utiliser une bibliothèque d'assertion qui remplace ou s'ajoute à celui qui est fourni, à condition qu'il renvoie des exceptions lorsque l'assertion échoue.

Cela ne vous donnera pas une aussi bonne expérience que celle obtenue avec les [assertions intégrées](#assertions), et vous ne serez pas en mesure d'utiliser la [planification d'assertion](#planification-d-assertion) ([voir #25](https://github.com/avajs/ava/issues/25)).

Vous devrez configurer AVA pour que les tests n'échouent pas si aucune assertion n'est exécutée, car AVA ne peut pas dire si les assertions personnalisées passent. Définissez l'option `failWithoutAssertions` à `false` dans la [configuration de AVA dans le `package.json`](./06-configuration.md).

```js
import assert from 'assert';

test('assertion personnalisée', t => {
	assert(true);
});
```

## Assertions intégrées

### `.pass(message?)`

L'assertion passe. Renvoie un booléen indiquant si l'assertion est passée.

### `.fail(message?)`

L'assertion échoue. Renvoie un booléen indiquant si l'assertion est passée.

### `.assert(actual, message?)`

Affirme que `actual` est truthy. Renvoie un booléen indiquant si l'assertion est passée.

### `.truthy(actual, message?)`

Affirme que `actual` est truthy. Renvoie un booléen indiquant si l'assertion est passée.

### `.falsy(actual, message?)`

Affirme que `actual` est falsy. Renvoie un booléen indiquant si l'assertion est passée.

### `.true(actual, message?)`

Affirme que `actual` est à `true`. Renvoie un booléen indiquant si l'assertion est passée.

### `.false(actual, message?)`

Affirme que `actual` est à `false`. Renvoie un booléen indiquant si l'assertion est passée.

### `.is(actual, expected, message?)`

Affirme que `actual` est le même que `expected`. Ceci est basé sur [`Object.is()`](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/Object/is). Renvoie un booléen indiquant si l'assertion est passée.

### `.not(actual, expected, message?)`

Affirme que `actual` n'est pas le même que `expected`. Ceci est basé sur [`Object.is()`](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/Object/is). Renvoie un booléen indiquant si l'assertion est passée.

### `.deepEqual(actual, expected, message?)`

Affirme que `actual` est profondément égale à `expected`. Consulter [Concordance](https://github.com/concordancejs/concordance) pour plus de détails.

### `.notDeepEqual(actual, expected, message?)`

Affirme que `actual` n'est pas profondément égale à `expected`. L'inverse de `.deepEqual()`. Renvoie un booléen indiquant si l'assertion est passée.

### `.like(actual, selector, message?)`

Affirme que `actual` est comme `selector`. C'est une variante de `.deepEqual()`, cependant `selector` n'a pas besoin d'avoir les mêmes propriétés énumérables que `actual`.

Au lieu de cela, AVA dérive un objet *comparable* de `actual`, basé sur les propriétés profondément imbriquées de `selector`. Cet objet est ensuite comparé à `selector` en utilisant `.deepEqual()`.

Toutes les valeurs de `selector` qui ne sont pas des objets courants doivent être profondément égales aux valeurs correspondantes de `actual`.

Dans l'exemple suivant, la propriété `map` de `actual` doit être profondément égale à celle de `selector`. Cependant, `nested.qux` est ignoré, car il n'est pas dans `selector`.

```js
t.like({
	map: new Map([['foo', 'bar']]),
	nested: {
		baz: 'thud',
		qux: 'quux'
	}
}, {
	map: new Map([['foo', 'bar']]),
	nested: {
		baz: 'thud',
	}
})
```

Enfin, il renvoie un booléen indiquant si l'assertion a réussi.

### `.throws(fn, [expectation, [message]])`

Affirme qu'une erreur est levée. `fn` doit être une fonction qui devrait lever une erreur. La valeur levée *doit* être une erreur. Elle est retournée afin que vous puissiez lancer d'autres assertions. Si l'assertion échoue, alors `undefined` est retourné.

`expectation` peut être un objet avec une ou plusieurs des propriétés suivantes :

* `instanceOf` : un constructeur, l'erreur levée doit être une "instance de"
* `is` : l'erreur levée doit être strictement égale à `expectation.is`
* `message` : les types suivants sont valables :
  * une chaîne - comparée au message de l'erreur levée
  * une expression régulière - qui correspond à ce message
  * une fonction - on lui transmet le message d'erreur et il doit renvoyer un booléen pour indiquer si l'assertion a passé
* `name` : la valeur `.name` attendue de l'erreur levée
* `code` : la valeur `.code` attendue de l'erreur levée

`expectation` n'a pas besoin d'être précisé. Si vous n'en avez pas besoin mais que vous voulez définir un message d'assertion, vous devez spécifier `undefined`.

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

### `.throwsAsync(thrower, expectation?, message?)`

Affirme qu'une erreur est levée. `thrower` peut être une fonction async qui devrait lever une erreur ou une promesse qui devrait échouée. Cette affirmation doit être attendue (await).

La valeur levée *doit* être une erreur. Elle est renvoyée afin que vous puissiez exécuter d'autres assertions. Si l'assertion échoue, alors `undefined` est retourné.

`expectation` peut être un objet avec une ou plusieurs des propriétés suivantes :

* `instanceOf` : un constructeur, l'erreur levée doit être une "instance de"
* `is` : l'erreur levée doit être strictement égale à `expectation.is`
* `message` : les types suivants sont valables :
  * une chaîne - comparée au message de l'erreur levée
  * une expression régulière - qui correspond à ce message
  * une fonction - on lui transmet le message d'erreur et il doit renvoyer un booléen pour indiquer si l'assertion a passé
* `name` : la valeur `.name` attendue de l'erreur levée
* `code` : la valeur `.code` attendue de l'erreur levée

`expectation` n'a pas besoin d'être précisé. Si vous n'en avez pas besoin mais que vous voulez définir un message d'assertion, vous devez spécifier `undefined`.

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

### `.notThrows(fn, message?)`

Affirme qu'aucune erreur n'est levée. `fn` doit être une fonction qui ne doit pas être levée. Ne renvoie rien.

### `.notThrowsAsync(nonThrower, message?)`

Affirme qu'aucune erreur n'est levée. `nonThrower` peut être une fonction async qui ne devrait pas lever une erreur ou une promesse qui devrait être résolue.

Comme l'assertion `.throwsAsync()`, vous devez attendre que l'assertion se termine :

```js
test('resolves', async t => {
	await t.notThrowsAsync(promise);
});
```

 Ne renvoie rien.

### `.regex(contents, regex, message?)`

Affirme que `contents` correspond à `regex`. Renvoie un booléen indiquant si l'assertion est passée.

### `.notRegex(contents, regex, message?)`

Affirme que `contents` ne correspond pas à `regex`. Renvoie un booléen indiquant si l'assertion est passée.

### `.snapshot(expected, message?)`

Compare la valeur `expected` avec un instantané enregistré auparavant. Les instantanés sont conservés pour chaque test, donc assurez-vous de donner à vos tests des titres uniques.

### `.try(title?, implementation | macro, ...args?)`

`.try()` vous permet d'*essayer* des assertions sans faire échouer le test.

La fonction d'implémentation se comporte de la même manière que toute autre fonction de test. Vous pouvez même utiliser des macros. Le premier argument title est toujours facultatif. Les arguments supplémentaires sont passés à la fonction d'implémentation ou à la fonction macro.

`.try()` est une fonction asynchrone. Vous devez indiquer `await`. L'objet résultat possède les méthodes `commit()` et `discard()`. Vous devez décider si vous voulez valider ou rejeter le résultat. Si vous validez un résultat qui a échoué, votre test échouera.

Vous pouvez vérifier si la tentative a réussi en utilisant la propriété `passed`. Les éventuelles erreurs d'assertion sont disponibles via la propriété `errors`. Le titre de la tentative est disponible via la propriété `title`.

Les journaux de `t.log()` sont disponibles via la propriété `logs`. Vous pouvez choisir de conserver ces journaux dans le cadre de votre test en passant `{retainLogs : true}` aux méthodes `commit()` et `discard()`.

La fonction d'implémentation reçoit son propre [contexte d'exécution](./02-execution-context.md), tout comme une fonction de test. Vous devez faire attention à ne réaliser des assertions qu'en utilisant le contexte d'exécution de la tentative. Au moins une assertion doit réussir pour que votre tentative réussisse.

Vous pouvez exécuter plusieurs tentatives simultanément, dans un seul test. Cependant, vous ne pouvez pas utiliser d'instantanés lorsque vous le faites.

Exemple :

```js
const twoRandomIntegers = () => {
	const rnd = Math.round(Math.random() * 100);
	const x = rnd % 10;
	const y = Math.floor(rnd / 10);
	return [x, y];
};

test('flaky macro', async t => {
	const firstTry = await t.try((tt, a, b) => {
		tt.is(a, b);
	}, ...twoRandomIntegers());

	if (firstTry.passed) {
		firstTry.commit();
		return;
	}

	firstTry.discard();
	t.log(firstTry.errors);

	const secondTry = await t.try((tt, a, b) => {
		tt.is(a, b);
	}, ...twoRandomIntegers());
	secondTry.commit();
});
```

Renvoie un booléen indiquant si l'assertion est passée.
