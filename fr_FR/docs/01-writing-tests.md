___
**Note du traducteur**

C'est la traduction du fichier [01-writing-tests.md](https://github.com/avajs/ava/blob/master/docs/01-writing-tests.md). Voici un [lien](https://github.com/avajs/ava/compare/b4ea43529a6d058a96055735cfa6e7056c009112...master#diff-78477dfeef8b2282ba186c90de932951) vers les différences avec le master de AVA (Si en cliquant sur le lien, vous ne trouvez pas le fichier `01-writing-tests.md` parmi les fichiers modifiés, vous pouvez donc en déduire que la traduction est à jour).
___
# Ecriture des tests

Traductions : [English](https://github.com/avajs/ava/blob/master/docs/01-writing-tests.md)

Les tests sont exécutés simultanément. Vous pouvez spécifier des tests synchrones et asynchrones. Les tests sont considérés synchrones sauf si vous retournez une promesse, un [observable](https://github.com/zenparsing/zen-observable) ou vous le déclarez dans un calback de test.

Vous devez définir tous les tests de manière synchrone. Ils ne peuvent pas être définis à l'intérieur de `setTimeout`, `setImmediate`, etc.

AVA essaie d'exécuter les fichiers de test dans leur répertoire de travail courant, c'est à dire le répertoire qui contient votre fichier `package.json`.

## Isolement du processus

Chaque fichier de test est exécuté dans un processus Node.js séparé. Cela apporte beaucoup d'avantages. Cela vous permet de changer l'état global ou de substituer un seul fichier de test, sans affecter les autres. C'est également bien pour des performances sur les processeurs multi-cœurs modernes, ce qui permet d'exécuter plusieurs fichiers de test en parallèle.

AVA définira `process.env.NODE_ENV` à `test`, à moins que la variable d'environnement `NODE_ENV` ait été définie. Ceci est utile si le code que vous testez a des valeurs par défaut de test (par exemple lors de la sélection de la base de données à laquelle se connecter). Toutefois, il se peut que votre code ou ses dépendances se comportent différemment. Notez que `'NODE_ENV' dans process.env` sera toujours `true`.

## Déclaration des tests

Pour déclarer un test, vous appelez la fonction `test` que vous importez de AVA. Fournissez un titre obligatoire et une fonction d'implémentation. Les titres doivent être uniques dans chaque fichier de test. La fonction sera appelée lorsque votre test sera exécutée. Un [objet d'exécution](./02-execution-context.md) est passé comme premier argument.

**Remarque :** Pour que les [messages d'assertion améliorés](./03-assertions.md#enhanced-assertion-messages) se comportent correctement, le premier argument **doit** être nommé `t`.

```js
import test from 'ava';

test('mon test qui passe', t => {
	t.pass();
});
```

## Exécution des tests en série

Par défaut les tests sont exécutés simultanément, parfois, vous devez écrire des tests qui ne peuvent pas fonctionner en même temps. Dans ces rares cas, vous pouvez utiliser `.serial`. Cela forcera les tests à s'exécuter en série *avant* ceux qui s'exécutent en simultané.

```js
test.serial('passe en série', t => {
	t.pass();
});
```

Notez que cela ne s'applique qu'aux tests dans un fichier de test. AVA exécutera toujours plusieurs fichiers de tests en même temps, sauf si vous passez l'[option `--serial` au CLI](./05-command-line.md).

Vous pouvez utiliser le modificateur `.serial` avec tous les tests, les hooks et même avec `.todo()`, mais il n'est disponible que sur la fonction `test`.

## Prise en charge des promesses

Les tests doivent retourner une promesse. AVA attendra que la promesse soit résolue avant de terminer le test. Si la promesse est rejetée, le test échouera.

```js
test('se résout avec unicorn', t => {
	return somePromise().then(result => {
		t.is(result, 'unicorn');
	});
});
```

## Prise en charge des fonctions asynchrones

AVA est livré avec un support pour les [fonctions asynchrones](https://developer.mozilla.org/fr/docs/Web/JavaScript/Reference/Instructions/async_function).

```js
test(async function (t) {
	const value = await promiseFn();
	t.true(value);
});

// Fonction fléchée async
test('promises the truth', async t => {
	const value = await promiseFn();
	t.true(value);
});
```

## Prise en charge de Observable

AVA est livré avec un support pour les [observables](https://github.com/zenparsing/es-observable). Si vous retournez un observable dans un test, AVA le consommera automatiquement jusqu'à la fin avant de terminer le test.

*Vous n'avez pas besoin d'utiliser le "mode callback" ou appeler `t.end()`.*

```js
test('gère les observables', t => {
	t.plan(3);
	return Observable.of(1, 2, 3, 4, 5, 6)
		.filter(n => {
			// Seulement les nombres pairs
			return n % 2 === 0;
		})
		.map(() => t.pass());
});
```

## Prise en charge du Callback

AVA prend en charge l'utilisation de `t.end` comme callback final lors de l'utilisation de callback d'API avec le style node d'["erreur-en-premier"](http://thenodeway.io/posts/understanding-error-first-callbacks/). AVA considérera toute valeur truthy passée comme premier argument à `t.end` comme une erreur. Notez que `t.end` exige le "mode callback", qui peut être activé en utilisant le chaînage `test.cb`.

```js
test.cb('data.txt peut être lu', t => {
	// `t.end` vérifie automatiquement le premier argument pour l'erreur
	fs.readFile('data.txt', t.end);
});
```

## Exécution de tests spécifiques

Lors du développement, il peut être utile d'exécuter seulement quelques tests spécifiques. Ceci peut être effectué en utilisant `.only` :

```js
test('ne sera pas exécuté', t => {
	t.fail();
});

test.only('sera exécuté', t => {
	t.pass();
});
```

Vous pouvez utiliser le modificateur `.only` avec tous les tests. Il ne peut pas être utilisé avec des hooks ou `.todo ()`.

*Remarque :* Le modificateur `.only` s'applique au fichier de test où il est défini, donc, si vous exécutez plusieurs fichiers de test, les tests dans les autres fichiers seront encore exécutés. Si vous souhaitez exécuter uniquement le test `test.only`, fournissez juste ce fichier de test à AVA.

### Passer des tests

Parfois, des tests défaillants peuvent être difficiles à corriger. Vous pouvez dire à AVA de passer ces tests en utilisant le modificateur `.skip`. Ils vont encore être présentés dans le résultat (comme ayant été passés (skipped)), mais ils ne sont jamais exécutés.

```js
test.skip('ne sera pas exécuté', t => {
	t.fail();
});
```

Vous devez spécifier la fonction d'implémentation. Vous pouvez utiliser le modificateur `.skip` avec tous les tests et les hooks, mais pas avec `.todo()`. Vous ne pouvez pas appliquer d'autres modificateurs à `.skip`.

## Les tests fictifs ("todo")

Vous pouvez utiliser le modificateur `.todo` lorsque vous avez l'intention d'écrire un test. Comme les tests passés (`.skip`), ces tests fictifs sont présentés dans le résultat. Ils exigent seulement un titre : vous ne pouvez pas spécifier la fonction d'implémentation.

```js
test.todo('il faudra penser à écrire cela plus tard');
```

Vous pouvez signaler que vous devez écrire un test en série :

```js
test.serial.todo('penser à l\'écrire plus tard');
```

## Test défaillant

Vous pouvez utiliser le modificateur `.failing` pour marquer les anomalies de votre code qui ont besoin d'être corrigées. Les tests défaillants sont exécutés comme les normaux, mais ils attendent un échec, ainsi ils ne casseront pas votre build quand ils le font. Si un test marqué comme défaillant alors qu'il passe, il sera signalé comme une erreur et fera échouer le build avec un message utile demandant de retirer le modificateur `.failing`.

Cela vous permet de merger les tests de `.failing` avant qu'un correctif soit mis en œuvre sans casser le CI. Ceci est un excellent moyen pour reconnaître un PR avec un bon rapport de bogues avec un commit qui lui est crédité, même si celui qui le rapporte est en réalité incapable de résoudre le problème.

```js
// Voir : github.com/user/repo/issues/1234
test.failing('démontrer un bogue', t => {
	t.fail(); // le test sera compté comme passé
});
```

## Les [hooks](https://fr.wikipedia.org/wiki/Hook_%28informatique%29) before & after

AVA vous permet d'enregistrer des hooks qui sont exécutés avant et après vos tests. Cela vous permet d'exécuter le code de configuration et/ou de dé-configuration.

`test.before()` enregistre un hook qui sera exécuté avant le premier test dans votre fichier de test. De même, `test.after()` enregistre un hook qui sera exécuté après le dernier test. Utilisez `test.after.always()` pour enregistrer un hook qui s'exécutera **toujours**, une fois que vos tests et autres hooks sont terminés. Les hooks `.always()` s'exécutent indépendamment des erreurs précédentes, ils sont donc idéals pour des tâches de nettoyage. Notez cependant que les exceptions non interceptées, les rejets non traités et les timeouts planteront vos tests, empêchant éventuellement les hooks `.always ()` de s'exécuter.

`test.beforeEach()` enregistre un hook qui sera exécuté avant chaque test dans votre fichier de test. De même, `test.afterEach()` enregistre un hook qui sera exécuté après chaque test. Utilisez `test.afterEach.always()` pour enregistrer un hook qui est appelé après, même si un autre hook de test ou le test lui-même échouent.

Si un test est ignoré avec le modificateur `.skip`, les hooks respectifs `.beforeEach()`, `.afterEach()` et `.afterEach.always()` ne sont pas exécutés. De même, si tous les tests dans un fichier de test sont ignorés, les hooks `.before()`, `.after()` et `.after.always()` pour le fichier ne sont pas exécutés.

Comme `test()`, ces méthodes prennent comme arguments un titre facultatif et une implémentation d'une fonction. Le titre est affiché si votre hook ne parvient pas à s'exécuter. L'implémentation est appelé avec un [objet d'exécution](./02-execution-context.md). Vous pouvez utiliser des assertions dans vos hooks. Vous pouvez également passer une [fonction macro](#réutilisation-de-test-logique-à-travers-des-macros) et des arguments supplémentaires.

Les hooks `before` s'exécutent avant les hooks `beforeEach`. Les hooks `afterEach` s'exécutent avant les hooks `after`. Au sein de leur catégorie, les hooks s'exécutent dans l'ordre où ils ont été définis. Par défaut, les hooks s'exécutent simultanément, mais vous pouvez utiliser `test.serial` pour s'assurer qu'un seul hook s'exécute à la fois. Contrairement aux tests, les hooks en série *ne* sont *pas* lancés avant les autres hooks :

```js
test.before(t => {
	// Cela s'exécute avant tous les tests
});

test.before(t => {
	// Cela s'exécute simultanément avec ce qui précède
});

test.serial.before(t => {
	// Cela s'exécute après ce qui précède
});

test.serial.before(t => {
	// Cela s'exécute aussi après ce qui précède, et avant les tests
});

test.after('cleanup', t => {
	// Cela s'exécute après tous les tests
});

test.after.always('guaranteed cleanup', t => {
	// Cela s'exécutera toujours, indépendamment des erreurs précédentes
});

test.beforeEach(t => {
	// Cela s'exécute avant chaque test
});

test.afterEach(t => {
	// Cela s'exécute après chaque test
});

test.afterEach.always(t => {
	// Cela s'exécute après chaque test et autres hooks de test, même s'ils échouent
});

test('titre', t => {
	// Test normal
});
```

Les hooks peuvent être synchrones ou asynchrones, comme les tests. Pour rendre un hook asynchrone, retournez une promesse ou un observable, utilisez une fonction async, ou activez le mode callback via `test.before.cb()`, `test.beforeEach.cb()` etc.

```js
test.before(async t => {
	await promiseFn();
});

test.after(t => {
	return new Promise(/* ... */);
});

test.beforeEach.cb(t => {
	setTimeout(t.end);
});

test.afterEach.cb(t => {
	setTimeout(t.end);
});
```

Gardez à l'esprit que les hooks `.beforeEach()` et `.afterEach()` s'exécutent juste avant et après un test qui s’exécute, et que par défaut les tests s'exécutent simultanément. Cele signifie que plusieurs hooks `.beforeEach()` peuvent s'exécuter simultanément. L'utilisation de `test.serial.beforeEach()` ne change pas ça. Si vous avez besoin de mettre en place un état global pour chaque test ([par exemple](https://github.com/avajs/ava/issues/560) en espionnant `console.log`), vous devez vous assurer que les tests eux-mêmes sont [exécutés en série](#exécution-des-tests-en-série).

Rappelez-vous que AVA exécute chaque fichier de test dans son propre processus. Vous n'avez pas besoin de nettoyer l'état global dans le hook `.after()`, puisqu'on l'appelle seulement lorsqu’on sort du processus.

## Tester le contexte

Les hooks peuvent partager le contexte avec le test :

```js
test.beforeEach(t => {
	t.context.data = generateUniqueData();
});

test('les données du contexte sont foo', t => {
	t.is(t.context.data + 'bar', 'foobar');
});
```

Le contexte créé dans les hooks `.before()` est [cloné](https://www.npmjs.com/package/lodash.clone) avant d'être passé aux hooks `.beforeEach()` et / ou aux tests. Les hooks `.after()` et `.after.always()` reçoive la valeur originale du contexte.

Pour les hooks `.beforeEach()`, `.afterEach()` et `.afterEach.always()` le contexte *n'est pas* partagé entre les différents tests, cela vous permet de configurer des données afin qu'elles ne fuient pas vers d'autres tests.

Par défaut `t.context` est un objet, mais vous pouvez directement le réassigné :

```js
test.before(t => {
	t.context = 'unicorn';
});

test('le contexte est unicorn', t => {
	t.is(t.context, 'unicorn');
});
```

## Récupération des métadonnées de test

Les fichiers helper peuvent déterminer le nom du fichier de test en cours en lisant `test.meta.file`. Ceci élimine le besoin de passer `__filename` depuis le test aux helpers.

```js
import test from 'ava';

console.log('Test en cours d\'exécution : ', test.meta.file);
```

## Réutilisation de test logique à travers des macros

Des arguments supplémentaires passés à la déclaration de test seront transmis à l'implémentation du test. Ceci est utile pour créer des macros de test réutilisables.

```js
function macro(t, input, expected) {
	t.is(eval(input), expected);
}

test('2 + 2 = 4', macro, '2 + 2', 4);
test('2 * 3 = 6', macro, '2 * 3', 6);
```

Vous pouvez construire le titre du test par programmation en attachant une fonction `title` à la macro :

```js
function macro(t, input, expected) {
	t.is(eval(input), expected);
}

macro.title = (providedTitle = '', input, expected) => `${providedTitle} ${input} = ${expected}`.trim();

test(macro, '2 + 2', 4);
test(macro, '2 * 3', 6);
test('providedTitle', macro, '3 * 3', 9);
```

L'argument `providedTitle` est par défaut à `undefined` si l'utilisateur ne fournit pas de titre. Cela signifie que vous pouvez utiliser un paramétrage pour définir la valeur par défaut. L'exemple ci-dessus utilise la chaîne vide comme valeur par défaut.

Vous pouvez aussi passer des tableaux de fonction de macro :

```js
const safeEval = require('safe-eval');

function evalMacro(t, input, expected) {
	t.is(eval(input), expected);
}

function safeEvalMacro(t, input, expected) {
	t.is(safeEval(input), expected);
}

test([evalMacro, safeEvalMacro], '2 + 2', 4);
test([evalMacro, safeEvalMacro], '2 * 3', 6);
```

Nous vous encourageons à utiliser des macros au lieu de construire vos propres générateurs de test ([voici un exemple](https://github.com/avajs/ava-codemods/blob/47073b5b58aa6f3fb24f98757be5d3f56218d160/test/ok-to-truthy.js#L7-L9) de code qui devrait être remplacé par une macro). Les macros sont conçues pour effectuer une analyse statique de votre code, ce qui peut conduire à améliorer la performance, l'intégration des IDE et les règles de linter.
