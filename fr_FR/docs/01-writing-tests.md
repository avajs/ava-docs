___
**Note du traducteur**

C'est la traduction du fichier [01-writing-tests.md](https://github.com/avajs/ava/blob/main/docs/01-writing-tests.md). Voici un [lien](https://github.com/avajs/ava/compare/b208d143ad852dc95aa8b44eed94ac1f404a25f4...main#diff-2805523838e2cb48f81d6f25721aeed4f4bb91d40b101a1acad7d2edafde79cc) vers les différences avec le master de AVA (Si en cliquant sur le lien, vous ne trouvez pas le fichier `01-writing-tests.md` parmi les fichiers modifiés, vous pouvez donc en déduire que la traduction est à jour).
___
# Ecriture des tests

Traductions : [English](https://github.com/avajs/ava/raw/main/docs/01-writing-tests.md)

Les tests sont exécutés simultanément. Vous pouvez spécifier des tests synchrones et asynchrones. Les tests sont considérés synchrones sauf si vous retournez une promesse, un [observable](https://github.com/zenparsing/zen-observable) ou vous le déclarez dans un calback de test.

Vous devez définir tous les tests de manière synchrone. Ils ne peuvent pas être définis à l'intérieur de `setTimeout`, `setImmediate`, etc.

AVA essaie d'exécuter les fichiers de test dans leur répertoire de travail courant, c'est à dire le répertoire qui contient votre fichier `package.json`.

## Isolement du test

Chaque fichier de test est exécuté dans un nouveau processus de travail. C'est une nouveauté d'AVA 4, bien que vous puissiez revenir au comportement d'AVA 3 qui consiste à exécuter des processus séparés.

AVA définira `process.env.NODE_ENV` à `test`, à moins que la variable d'environnement `NODE_ENV` ait été définie. Ceci est utile si le code que vous testez a des valeurs par défaut de test (par exemple lors de la sélection de la base de données à laquelle se connecter). Toutefois, il se peut que votre code ou ses dépendances se comportent différemment. Notez que `'NODE_ENV' dans process.env` sera toujours `true`.

## Déclaration des tests

Pour déclarer un test, vous appelez la fonction `test` que vous importez de AVA. Fournissez un titre obligatoire et une fonction d'implémentation. Les titres doivent être uniques dans chaque fichier de test. La fonction sera appelée lorsque votre test sera exécutée. Un [objet d'exécution](./02-execution-context.md) est passé comme premier argument.

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

Parfois, des tests défaillants peuvent être difficiles à corriger. Vous pouvez dire temporairement à AVA de passer ces tests en utilisant le modificateur `.skip`. Ils vont encore être présentés dans le résultat (comme ayant été passés (skipped)), mais ils ne sont jamais exécutés.

```js
test.skip('ne sera pas exécuté', t => {
	t.fail();
});
```

Vous devez spécifier la fonction d'implémentation. Vous pouvez utiliser le modificateur `.skip` avec tous les tests et les hooks, mais pas avec `.todo()`. Vous ne pouvez pas appliquer d'autres modificateurs à `.skip`.

Si le test est susceptible d'échouer pendant un certain temps, utilisez `.failing()` à la place.

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

*Vous n'aurez peut-être pas besoin d'utiliser les hooks `.afterEach.always()` pour faire le ménage après un test.* Vous pouvez utiliser [`t.teardown()`](./02-execution-context.md#tteardownfn) pour annuler les effets de bord *dans* un test particulier.

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

Les hooks peuvent être synchrones ou asynchrones, comme les tests. Pour rendre un hook asynchrone, retournez une promesse ou un observable, ou utilisez une fonction async.

```js
test.before(async t => {
	await promiseFn();
});

test.after(t => {
	return new Promise(/* ... */);
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

Si les hooks `.before()` traitent `t.context` comme un objet, une copie peu profonde est faite et passée aux hooks `.beforeEach()` et/ou aux tests. Les autres types de valeurs sont passés tels quels. Les hooks `.after()` et `.after.always()` reçoivent la valeur originale du contexte.

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

Accédez aux données concernant le fichier de test actuellement chargé en lisant `test.meta`.

Propriétés disponibles :

* `file` : chemin vers le fichier de test, via une chaîne d'URL de fichier
* `snapshotDirectory` : répertoire où les instantanés sont stockés, comme une chaîne d'URL de fichier

```js
import test from 'ava';

console.log('Fichier de test en cours d\'exécution : ', test.meta.file);
```

## Réutilisation de test logique à travers des macros

[![Ouvrez dans StackBlitz](https://developer.stackblitz.com/img/open_in_stackblitz.svg)](https://stackblitz.com/github/avajs/ava/tree/main/examples/macros?file=test.js&terminal=test&view=editor)

Des arguments supplémentaires passés à la déclaration de test seront transmis à l'implémentation du test. Ceci est utile pour créer des macros de test réutilisables.

Vous _pourriez_ utiliser des fonctions simples :

```js
function macro(t, input, expected) {
	t.is(eval(input), expected);
}

test('2 + 2 = 4', macro, '2 + 2', 4);
test('2 * 3 = 6', macro, '2 * 3', 6);
```

Cependant, l'approche préférée est d'utiliser le helper `test.macro()`:

```js
import test from 'ava';

const macro = test.macro((t, input, expected) => {
	t.is(eval(input), expected);
});

test('title', macro, '3 * 3', 9);
```

Ou avec une fonction title :

```js
import test from 'ava';

const macro = test.macro({
	exec(t, input, expected) {
		t.is(eval(input), expected);
	},
	title(providedTitle = '', input, expected) {
		return `${providedTitle} ${input} = ${expected}`.trim();
	},
});

test(macro, '2 + 2', 4);
test(macro, '2 * 3', 6);
test('providedTitle', macro, '3 * 3', 9);
```

L'argument `providedTitle` prend par défaut la valeur `undefined` si l'utilisateur ne fournit pas de chaîne de titre. Cela signifie que vous pouvez utiliser une affectation de paramètre pour définir la valeur par défaut. L'exemple ci-dessus utilise la chaîne vide comme valeur par défaut.
