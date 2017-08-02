___
**Note du traducteur**

C'est la traduction du fichier [readme.md](https://github.com/avajs/ava/blob/master/readme.md). Voici un [lien](https://github.com/avajs/ava/compare/465fcecc9ae0d3274d4d41d3baaca241d6a40130...master#diff-0730bb7c2e8f9ea2438b52e419dd86c9) vers les différences avec le master de AVA (Si en cliquant sur le lien, vous ne trouvez pas le fichier `readme.md` parmi les fichiers modifiés, vous pouvez donc en déduire que la traduction est à jour).
___
# [![AVA](https://github.com/avajs/ava/blob/master/media/header.png)](https://ava.li)

> Lanceur de test futuriste

[![Build Status: Linux](https://travis-ci.org/avajs/ava.svg?branch=master)](https://travis-ci.org/avajs/ava) [![Build status: Windows](https://ci.appveyor.com/api/projects/status/e7v91mu2m5x48ehx/branch/master?svg=true)](https://ci.appveyor.com/project/ava/ava/branch/master) [![Coverage Status](https://coveralls.io/repos/github/avajs/ava/badge.svg?branch=master)](https://coveralls.io/github/avajs/ava?branch=master) [![Dependency Status](https://dependencyci.com/github/avajs/ava/badge)](https://dependencyci.com/github/avajs/ava) [![XO code style](https://img.shields.io/badge/code_style-XO-5ed9c7.svg)](https://github.com/sindresorhus/xo) [![Gitter](https://badges.gitter.im/join_chat.svg)](https://gitter.im/avajs/ava)

Même si JavaScript est mono-thread, l'IO dans Node.js peut se lancer en parallèle en raison de sa nature asynchrone. AVA profite de cela et exécute vos tests en même temps, ce qui est particulièrement avantageux pour les tests lourds d'IO. De plus, les fichiers de test sont exécutés en parallèle comme des processus séparés, cela vous donne encore de meilleures performances et un environnement isolé pour chaque fichier de test. Le [passage](https://github.com/sindresorhus/pageres/commit/663be15acb3dd2eb0f71b1956ef28c2cd3fdeed0) de Mocha à AVA dans Pageres a diminué la durée des tests de 31 à 11 secondes. Comme les tests sont exécutés simultanément, cela vous oblige à écrire des tests [atomiques](https://fr.wikipedia.org/wiki/Atomicit%C3%A9_%28informatique%29), ce qui signifie que les tests ne dépendent pas de l'état global ou de l'état des autres tests, ce qui est une bonne chose !

![](https://github.com/avajs/ava/blob/master/media/mini-reporter.gif)

*Lisez notre [guide de contribution](contributing.md) si vous souhaitez contribuer (issues/PRs/etc).*

Suivez le [compte Twitter de AVA](https://twitter.com/ava__js) pour les mises à jour.

Traductions : [Español](https://github.com/avajs/ava-docs/blob/master/es_ES/readme.md), [Français](https://github.com/avajs/ava-docs/blob/master/fr_FR/readme.md), [Italiano](https://github.com/avajs/ava-docs/blob/master/it_IT/readme.md), [日本語](https://github.com/avajs/ava-docs/blob/master/ja_JP/readme.md), [한국어](https://github.com/avajs/ava-docs/blob/master/ko_KR/readme.md), [Português](https://github.com/avajs/ava-docs/blob/master/pt_BR/readme.md), [Русский](https://github.com/avajs/ava-docs/blob/master/ru_RU/readme.md), [简体中文](https://github.com/avajs/ava-docs/blob/master/zh_CN/readme.md)


## Contenus

- [Utilisation](#utilisation)
- [Utilisation du CLI](#cli)
- [Débogage](#débogage)
- [Reporters](#reporters)
- [Configuration](#configuration)
- [Documentation](#documentation)
- [API](#api)
- [Assertions](#assertions)
- [Test d'instantané](#test-dinstantané)
- [Astuces](#astuces)
- [FAQ](#faq)
- [Recettes](#recettes)
- [Support](#support)
- [En relation](#en-relation)
- [Liens](#liens)
- [L'équipe](#léquipe)


## Pourquoi AVA ?

- Minimal et rapide
- Syntaxe de test simple
- Exécution des tests simultanément
- Application de l'écriture des tests atomiques
- Aucunes globales implicites
- Inclus les définitions des types pour TypeScript & Flow
- [Magic assert](#magic-assert)
- [Environnement isolé pour chaque fichier de test](#isolement-du-processus)
- [Écriture de vos tests en ES2017](#prise-en-charge-de-es2017)
- [Prise en charge des promesses](#prise-en-charge-des-promesses)
- [Prise en charge des fonctions génératrices](#prise-en-charge-des-fonctions-génératrices)
- [Prise en charge des fonctions asynchrones](#prise-en-charge-des-fonctions-asynchrones)
- [Prise en charge d'Observable](#prise-en-charge-de-observable)
- [Messages d'assertions améliorés](#messages-dassertions-améliorés)
- [Reporter de TAP](#reporter-de-tap)
- [Migration automatique depuis un autre exécuteur de test](https://github.com/avajs/ava-docs/blob/master/fr_FR/related/ava-codemods/readme.md#migration-vers-ava)


## Syntaxe d'un Test

```js
import test from 'ava';

test(t => {
	t.deepEqual([1, 2], [1, 2]);
});
```

## Utilisation

### Ajoutez AVA à votre projet

Installez globalement AVA et lancez le avec `--init` pour ajouter AVA à votre `package.json`. [Yarn](https://yarnpkg.com/) permet actuellement d'améliorer la vitesse du processus d'installation par rapport à npm. Penser à [utiliser Yarn](https://yarnpkg.com/en/docs/install) si l'installation est trop lente pour vos exigences.


```console
$ yarn global add ava
$ ava --init
```

Si vous préférez l'utilisation de npm :

```console
$ npm install --global ava
$ ava --init
```

Votre `package.json` ressemblera alors à ceci :

```json
{
	"name": "awesome-package",
	"scripts": {
		"test": "ava"
	},
	"devDependencies": {
		"ava": "^0.20.0"
	}
}
```

Tous les arguments passés après `--init` sont ajoutés comme config au `package.json`.

#### Installation manuelle

Vous pouvez également installer AVA directement :

```console
$ yarn add --dev ava
```

Alternativement en utilisant npm :

```console
$ npm install --save-dev ava
```

Vous devez configurer le script `test` dans votre `package.json` pour utiliser `ava` (Voir ci-dessus).

#### Créez votre fichier de test

Créez un fichier nommé `test.js` dans le répertoire racine du projet :

```js
import test from 'ava';

test('foo', t => {
	t.pass();
});

test('bar', async t => {
	const bar = Promise.resolve('bar');

	t.is(await bar, 'bar');
});
```

### Exécutez le test

```console
$ npm test
```

### Scrutez le test

```console
$ npm test -- --watch
```

AVA est livré avec un mode watch intelligent. [Apprenez en plus avec cette recette](docs/recipes/watch-mode.md).

## CLI

```console
$ ava --help

  Usage
    ava [<file|directory|glob> ...]

  Options
    --init                  Add AVA to your project (Ajouter AVA à votre projet)
    --fail-fast             Stop after first test failure (Arrêter dès qu'un test échoue)
    --serial, -s            Run tests serially (Lancer les tests en série)
    --tap, -t               Generate TAP output (Générer une sortie au format TAP)
    --verbose, -v           Enable verbose output (Activer le mode verbose)
    --no-cache              Disable the transpiler cache (Désactiver le cache du transpileur)
    --no-power-assert       Disable Power Assert (Désactiver Power Assert)
    --color                 Force color output (Force l'affichage avec des couleurs)
    --no-color              Disable color output (Désactiver l'affichage avec des couleurs)
    --match, -m             Only run tests with matching title (Can be repeated) (Exécute seulement les tests qui correspondent au titre (peut être répété))
    --watch, -w             Re-run tests when tests and source files change (Re-exécute les tests quand les tests et les fichiers sources ont changé)
    --timeout, -T           Set global timeout (Définir un timeout global)
    --concurrency, -c       Max number of test files running at the same time (Default: CPU cores) (Nombre max de fichiers de test exécutés en même temps (Par défaut : coeurs CPU))
    --update-snapshots, -u  Update all snapshots (Mettre à jour les snapshots)

  Examples (Exemples)
    ava
    ava test.js test2.js
    ava test-*.js
    ava test
    ava --init
    ava --init foo.js

  Default patterns when no arguments (Les patterns par défaut quand il n'y a pas d'arguments) :
  test.js test-*.js test/**/*.js **/__tests__/**/*.js **/*.test.js
```

*Notez que le CLI utilisera votre installation locale de AVA lorsqu'il est disponible, même lorsqu'il est exécuté de manière globale.*

Les répertoires sont récursifs, où tous les fichiers `*.js` sont traités comme des fichiers de test. Les répertoires nommés `fixtures`, `helpers` et `node_modules` sont *toujours* ignorés. C'est aussi le cas pour les fichiers commençant par `_`, cela vous permet de placer des helpers dans le même répertoire que vos fichiers de test.

Lors de l'utilisation de `npm test`, vous pouvez passer directement des arguments `npm test test2.js`, mais pour les options, vous devez les passez ainsi `npm test -- --verbose`.


## Débogage

AVA exécute les tests dans des processus enfants, donc pour déboguer les tests, vous devez utiliser cette solution de contournement :

```console
$ node --inspect node_modules/ava/profile.js some/test/file.js
```

### Astuces pour débogages spécifiques

- [Chrome DevTools](docs/recipes/debugging-with-chrome-devtools.md)
- [WebStorm](docs/recipes/debugging-with-webstorm.md)
- [Visual Studio Code](docs/recipes/debugging-with-vscode.md)


## Reporters

### Mini-reporter

Le mini-reporter est le reporter par défaut.

<img src="https://github.com/avajs/ava/blob/master/media/mini-reporter.gif" width="460">

### Reporter verbose (verbeux)

Utilisez l'option `--verbose` pour activer le reporter verbose. Il est toujours utilisé dans les environnements de CI, sauf si [`--tap`](#reporter-de-tap) est activé.

<img src="https://github.com/avajs/ava/blob/master/media/verbose-reporter.png" width="420">

### Reporter de TAP

AVA prend en charge le format de TAP et est compatible avec [tous les reporters de TAP](https://github.com/sindresorhus/awesome-tap#reporters). Utilisez l'option `--tap` pour activer la restitution de TAP.

```console
$ ava --tap | tap-nyan
```

<img src="https://github.com/avajs/ava/blob/master/media/tap-reporter.png" width="398">

Veuillez noter que le reporter TAP est indisponible lors de l'utilisation du [mode watch](#scrutez-le-test).

### Magic assert

AVA ajoute les extraits de code et les différences pour les valeurs réelles et attendues. Si les valeurs dans l'assertion sont des objets ou des tableaux, seule la différence est affichée, tout ce qui ne concerne pas le sujet est enlevé, pour se concentrer uniquement sur le problème. La différence est aussi mise en évidence ! Si vous comparez des strings, à la fois sur une ou plusieurs lignes, AVA affiche un autre type de sortie, mettant en évidence les caractères supplémentaires ou manquants.

![](https://github.com/avajs/ava/blob/master/media/magic-assert-combined.png)

### Nettoyage de la stack trace

AVA supprime automatiquement les lignes sans rapport dans la stack trace, cela permet de trouver la source d'une erreur plus rapidement, comme présenté ci-dessous.


## Configuration

Toutes les options du CLI peuvent être configurés dans la section `ava` de votre `package.json`. Cela vous permet de modifier le comportement par défaut de la commande `ava`, ainsi vous n'avez plus besoin à chaque fois de taper les mêmes options sur l'invite de commande.

```json
{
	"ava": {
		"files": [
			"my-test-folder/*.js",
			"!**/not-this-file.js"
		],
		"source": [
			"**/*.{js,jsx}",
			"!dist/**/*"
		],
		"match": [
			"*oo",
			"!foo"
		],
		"concurrency": 5,
		"failFast": true,
		"failWithoutAssertions": false,
		"tap": true,
		"powerAssert": false,
		"require": [
			"babel-register"
		],
		"babel": "inherit"
	}
}
```

Les arguments passés au CLI seront toujours prioritaires sur ceux de la configuration dans `package.json`.

Voir la section [prise en charge de ES2017](#prise-en-charge-de-es2017) pour plus de détails sur l'option `babel`.

## Documentation

Les tests sont exécutés simultanément. Vous pouvez spécifier des tests synchrones et asynchrones. Les tests sont considérés synchrones sauf si vous retournez une promesse ou un [observable](https://github.com/zenparsing/zen-observable).

Nous recommandons *fortement* l'utilisation de [fonctions asynchrones](#prise-en-charge-des-fonctions-asynchrones). Elles rendent le code asynchrone concis et lisible, et elles retournent implicitement une promesse, donc vous n'avez pas à le faire.

Si vous n'avez pas la possibilité d'utiliser des promesses ou des observables, vous pouvez activer le "mode callback" en définissant votre test avec `test.cb([title], fn)`. Les tests déclarés de cette manière **doivent** être terminés manuellement avec `t.end()`. Ce mode est principalement destiné pour tester les APIs de style callback. Cependant, nous vous recommandons vivement d'utiliser les API [promisifying](https://github.com/sindresorhus/pify) de style callback au lieu d'utiliser le "mode callback", car cela donne des tests plus corrects et plus lisibles.

Vous devez définir tous les tests de manière synchrone. Ils ne peuvent pas être définis à l'intérieur de `setTimeout`, `setImmediate`, etc.

AVA essaie d'exécuter les fichiers de test dans leur répertoire de travail courant, c'est à dire le répertoire qui contient votre fichier `package.json`.

### Création des tests

Pour créer un test, vous appelez la fonction `test` que vous importez de AVA. Fournissez un titre facultatif et une fonction d'implémentation. La fonction sera appelée lorsque votre test sera exécutée. Un [objet d'exécution](#t) est passé comme premier argument.

**Remarque :** Pour que les [messages d'assertion améliorés](#messages-dassertions-améliorés) se comportent correctement, le premier argument **doit** être nommé `t`.

```js
import test from 'ava';

test('mon test qui passe', t => {
	t.pass();
});
```

#### Les titres

Les titres sont facultatifs, donc vous pouvez faire :

```js
test(t => {
	t.pass();
});
```

Il est recommandé de fournir des titres si vous avez plusieurs tests.

Si vous n'avez pas fourni un titre pour le test et que le callback est une fonction nommée, ce nom sera utilisé comme titre du test :

```js
test(function name(t) {
	t.pass();
});
```

### Assertion planifiée

Une assertion **plan**-ifiée assure que les tests passent uniquement quand un certain nombre d'assertions ont été exécutées. Elle vous aidera à traiter les cas où les tests sortent trop tôt. Elle mettra aussi les tests en échec si trop d'assertions sont exécutées, ce qui peut être utile si vous avez des assertions à l'intérieur des callbacks ou des boucles.

Si vous ne spécifiez pas une assertion planifiée, votre test échouera toujours si aucune assertion n'est exécutée. Définissez l'option `failWithoutAssertions` à `false` dans la [configuration de AVA dans le `package.json`](#configuration) pour désactiver ce comportement.

Il faut savoir que, contrairement à [`tap`](https://www.npmjs.com/package/tap) et [`tape`](https://www.npmjs.com/package/tape), AVA *n'arrête pas* automatiquement un test lorsque le nombre d'assertion prévu est atteint.

Ces exemples se traduiront par un test réussi :

```js
test(t => {
	t.plan(1);

	return Promise.resolve(3).then(n => {
		t.is(n, 3);
	});
});

test.cb(t => {
	t.plan(1);

	someAsyncFunction(() => {
		t.pass();
		t.end();
	});
});
```

Ce n'est pas le cas de ces exemples :

```js
test(t => {
	t.plan(2);

	for (let i = 0; i < 3; i++) {
		t.true(i < 3);
	}
}); // Echec, 3 assertions de trop sont exécutées

test(t => {
	t.plan(1);

	someAsyncFunction(() => {
		t.pass();
	});
}); // Echec, le test se termine de façon synchrone avant que l'assertion soit exécutée
```

### Exécution des tests en série

Par défaut les tests sont exécutés simultanément, ce qui est génial. Parfois, vous devez écrire des tests qui ne peuvent pas fonctionner en même temps. Dans ces rares cas, vous pouvez utiliser `.serial`. Cela forcera les tests à s'exécuter en série *avant* ceux qui s'exécutent en simultané.

```js
test.serial(t => {
	t.pass();
});
```

Notez que cela ne s'applique qu'aux tests dans un fichier de test. AVA exécutera toujours plusieurs fichiers de tests en même temps, sauf si vous passez l'[option `--serial` au CLI](#cli).

### Exécution de tests spécifiques

Lors du développement, il peut être utile d'exécuter seulement quelques tests spécifiques. Ceci peut être effectué en utilisant `.only`.

```js
test('ne sera pas exécuté', t => {
	t.fail();
});

test.only('sera exécuté', t => {
	t.pass();
});
```

*Remarque :* Le modificateur `.only` s'applique au fichier de test où il est défini, donc, si vous exécutez plusieurs fichiers de test, les tests dans les autres fichiers seront encore exécutés. Si vous souhaitez exécuter uniquement le test `test.only`, fournissez juste ce fichier de test à AVA.

### Exécution de tests correspondants à des titres

L'option `--match` permet d'exécuter des tests où le titre correspond à la demande. Ceci est réalisé avec des patterns génériques simples. Les patterns sont insensibles à la casse. Consultez [matcher](https://github.com/sindresorhus/matcher) pour plus d'informations.

Trouve les titres se terminant par 'foo'

```console
$ ava --match='*foo'
```

Trouve les titres commençant par 'foo'

```console
$ ava --match='foo*'
```

Trouve les titres contenant 'foo'

```console
$ ava --match='*foo*'
```

Trouve les titres qui ont *exactement* 'foo' (bien que insensible à la casse)

```console
$ ava --match='foo'
```

Trouve les titres ne contenant pas 'foo'

```console
$ ava --match='!*foo*'
```

Trouve les titres commençant par 'foo' et se terminant par 'bar'

```console
$ ava --match='foo*bar'
```

Trouve les titres commençant par 'foo' ou se terminant par 'bar'

```console
$ ava --match='foo*' --match='*bar'
```

Remarquez qu'un pattern qui correspond, a la priorité sur `.only`. Seuls les tests avec un titre explicite sont recherchés. Les tests, sans titre ou dont le titre est dérivé de la fonction d'implémentation, seront sautés si `--match` est utilisé.

Voici ce qui arrive lorsque vous exécutez AVA avec un pattern `*oo*` pour les tests suivants :

```js
test('foo s\'exécutera', t => {
	t.pass();
});

test('moo s\'exécutera aussi', t => {
	t.pass();
});

test.only('boo s\'exécutera mais pas de manière exclusive', t => {
	t.pass();
});

// Ne s'exécutera pas, car n'a pas de titre
test(function (t) {
	t.fail();
});

// Ne s'exécutera pas, car n'a pas de titre explicite
test(function foo(t) {
	t.fail();
});
```

### Passer des tests

Parfois, des tests défaillants peuvent être difficiles à corriger. Vous pouvez dire à AVA de passer ces tests en utilisant le modificateur `.skip`. Ils vont encore être présentés dans le résultat (comme ayant été passés (skipped)), mais ils ne sont jamais exécutés.

```js
test.skip('ne sera pas exécuté', t => {
	t.fail();
});
```

Vous devez spécifier la fonction d'implémentation.

### Les tests fictifs ("todo")

Vous pouvez utiliser le modificateur `.todo` lorsque vous avez l'intention d'écrire un test. Comme les tests passés (`.skip`), ces tests fictifs sont présentés dans le résultat. Ils exigent seulement un titre : vous ne pouvez pas spécifier la fonction d'implémentation.

```js
test.todo('il faudra penser à écrire cela plus tard');
```

### Test défaillant

Vous pouvez utiliser le modificateur `.failing` pour marquer les anomalies de votre code qui ont besoin d'être corrigées. Les tests défaillants sont exécutés comme les normaux, mais ils attendent un échec, ainsi ils ne casseront pas votre build quand ils le font. Si un test marqué comme défaillant alors qu'il passe, il sera signalé comme une erreur et fera échouer le build avec un message utile demandant de retirer le modificateur `.failing`.

Cela vous permet de merger les tests de `.failing` avant qu'un correctif soit mis en œuvre sans casser le CI. Ceci est un excellent moyen pour reconnaître un PR avec un bon rapport de bogues avec un commit qui lui est crédité, même si celui qui le rapporte est en réalité incapable de résoudre le problème.

```js
// Voir : github.com/user/repo/issues/1234
test.failing('démontrer un bogue', t => {
	t.fail(); // le test sera compté comme passé
});
```

### Les [hooks](https://fr.wikipedia.org/wiki/Hook_%28informatique%29) before & after

AVA vous permet d'enregistrer des hooks qui sont exécutés avant et après vos tests. Cela vous permet d'exécuter le code de configuration et/ou de dé-configuration.

`test.before()` enregistre un hook qui sera exécuté avant le premier test dans votre fichier de test. De même, `test.after()` enregistre un hook qui sera exécuté après le dernier test. Utilisez `test.after.always()` pour enregistrer un hook qui s'exécutera **toujours**, une fois que vos tests et autres hooks sont terminés. Les hooks `.always()` s'exécutent indépendamment des erreurs précédentes, ils sont donc idéals pour des tâches de nettoyage. Il y a cependant deux exceptions à cela. Si vous utilisez `--fail-fast`, AVA arrêtera le test dès qu'un test échoue, et il n'exécutera pas de hooks, y compris les hooks `.always()`. Les exceptions non interceptées planteront vos tests, empêchant éventuellement les hooks `.always ()` de s'exécuter.

`test.beforeEach()` enregistre un hook qui sera exécuté avant chaque test dans votre fichier de test. De même, `test.afterEach()` enregistre un hook qui sera exécuté après chaque test. Utilisez `test.afterEach.always()` pour enregistrer un hook qui est appelé après, même si un autre hook de test ou le test lui-même échouent. Les hooks `.always()` sont idéals pour les tâches de nettoyage.

**Remarque** : Si l'option `--fail-fast` est spécifiée, AVA s'arrêtera après le premier test en échec et le hook `.always` **ne** s'exécutera **pas**.

Comme `test()`, ces méthodes prennent comme arguments un titre facultatif et une fonction callback. Le titre est affiché si votre hook ne parvient pas à s'exécuter. Le callback est appelé avec un [objet d'exécution](#t).

Les hooks `before` s'exécutent avant les hooks `beforeEach`. Les hooks `afterEach` s'exécutent avant les hooks `after`. Au sein de leur catégorie, les hooks s'exécutent dans l'ordre où ils ont été définis.

```js
test.before(t => {
	// Cela s'exécute avant tous les tests
});

test.before(t => {
	// Cela s'exécute après ce qui précède, mais avant les tests
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

test(t => {
	// Test normal
});
```

Les hooks peuvent être synchrones ou asynchrones, comme les tests. Pour rendre un hook asynchrone, retournez une promesse ou un observable, utilisez une fonction async, ou activez le mode callback via `test.cb.before()`, `test.cb.beforeEach()` etc.

```js
test.before(async t => {
	await promiseFn();
});

test.after(t => {
	return new Promise(/* ... */);
});

test.cb.beforeEach(t => {
	setTimeout(t.end);
});

test.afterEach.cb(t => {
	setTimeout(t.end);
});
```

Gardez à l'esprit que les hooks `beforeEach` et `afterEach` s'exécutent juste avant et après un test qui s’exécute, et que par défaut les tests s'exécutent simultanément. Si vous avez besoin de mettre en place un état global pour chaque test ([par exemple](https://github.com/avajs/ava/issues/560) en espionnant `console.log`), vous devez vous assurer que les tests sont [exécutés en série](#exécution-des-tests-en-série).

Rappelez-vous que AVA exécute chaque fichier de test dans son propre processus. Vous n'avez pas besoin de nettoyer l'état global dans le hook `after`, puisqu'on l'appelle seulement lorsqu’on sort du processus.

#### Tester le contexte

Les hooks `beforeEach` & `afterEach` peuvent partager le contexte avec le test :

```js
test.beforeEach(t => {
	t.context.data = generateUniqueData();
});

test(t => {
	t.is(t.context.data + 'bar', 'foobar');
});
```

Le contexte n'est pas partagé entre les tests, cela vous permet donc de configurer les données sans incidence sur les autres tests ou les tests précédents. Par défaut `t.context` est un objet, mais vous pouvez directement le réassigné :

```js
test.beforeEach(t => {
	t.context = 'unicorn';
});

test(t => {
	t.is(t.context, 'unicorn');
});
```

Le partage du contexte *n'est pas* pour les hooks `before` et `after`.

### Chaînage des modificateurs de test

Vous pouvez utiliser les modificateurs `.serial`, `.only` et `.skip` dans n'importe quel ordre, avec `test`, `before`, `after`, `beforeEach` et `afterEach`. Par exemple :

```js
test.before.skip(...);
test.skip.after(...);
test.serial.only(...);
test.only.serial(...);
```

Cela signifie que vous pouvez ajouter temporairement `.skip` ou `.only` à la fin d'un test ou sur la définition d'un hook sans apporter d'autres modifications.

### Macros de test

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

macro.title = (providedTitle, input, expected) => `${providedTitle} ${input} = ${expected}`.trim();

test(macro, '2 + 2', 4);
test(macro, '2 * 3', 6);
test('providedTitle', macro, '3 * 3', 9);
```

L'argument par défaut de `providedTitle` est une chaîne vide si l'utilisateur n'a pas spécifié de titre. Cela permet de faciliter la concaténation sans avoir à se soucier de  `null` / `undefined`. Il convient de rappeler que la chaîne vide est considérée comme une valeur falsy, ainsi vous pouvez toujours utiliser `if (providedTitle) {...}`.

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

### Personnaliser les assertions

Vous pouvez utiliser une bibliothèque d'assertion qui remplace ou s'ajoute à celui qui est fourni, à condition qu'il renvoie des exceptions lorsque l'assertion échoue.

Cela ne vous donnera pas une aussi bonne expérience que celle obtenue avec les [assertions intégrées](#assertions), et vous ne serez pas en mesure d'utiliser l'[assertion planifiée](#assertion-planifiée) ([voir #25](https://github.com/avajs/ava/issues/25)).

Vous devrez configurer AVA pour que les tests n'échouent pas si aucune assertion n'est exécutée, car AVA ne peut pas dire si les assertions personnalisées passent. Définissez l'option `failWithoutAssertions` à `false` dans la [configuration de AVA dans le `package.json`](#configuration).

```js
import assert from 'assert';

test(t => {
	assert(true);
});
```

### Prise en charge de ES2017

AVA est livré avec un support intégré pour ES2017 via [Babel 6](https://babeljs.io). Il suffit d'écrire vos tests en ES2017. Aucune configuration supplémentaire n'est nécessaire. Vous pouvez utiliser n'importe quelle version de Babel dans votre projet. Nous utilisons notre propre bundle Babel avec notre preset [`@ava/stage-4`](https://github.com/avajs/babel-preset-stage-4) ainsi que des [transformations personnalisées](https://github.com/avajs/babel-preset-transform-test-files) pour les fichier test et helper.

La configuration de Babel correspondante à la configuration de AVA est la suivante :

```json
{
	"presets": [
		"@ava/stage-4",
		"@ava/transform-test-files"
	]
}
```

Vous pouvez personnaliser la façon dont AVA transpile les fichiers de test grâce à l'option `babel` dans la [configuration du `package.json`](#configuration) de AVA. Par exemple, pour remplacer les presets, vous pouvez utiliser:

```json
{
	"ava": {
		 "babel": {
			 "presets": [
					"es2015",
					"stage-0",
					"react"
			 ]
		 }
	}
}
```

Vous pouvez également utiliser le mot-clé `"inherit"`. Cela permet à AVA de reporter la configuration de Babel dans votre [fichier `.babelrc` ou `package.json`](https://babeljs.io/docs/usage/babelrc/). De cette façon, vos fichiers de test seront transpilés en utilisant la même configuration que vos fichiers sources sans avoir à le répéter pour AVA :

```json
{
	"babel": {
		"presets": [
			"es2015",
			"stage-0",
			"react"
		]
	},
	"ava": {
		"babel": "inherit"
	}
}
```

Consultez la [recette `.babelrc`](docs/recipes/babelrc.md) de AVA pour d'autres exemples et une explication plus détaillée des options de configuration.

Notez que AVA appliquera *toujours* [quelques plugins internes](docs/recipes/babelrc.md#notes) quelle que soit la configuration, mais ils ne doivent pas affecter le comportement de votre code.

### Prise en charge de TypeScript

AVA contient les typages pour TypeScript. Vous devez configurer la transpilation vous-même. Lorsque vous définissez `module` à `commonjs` dans votre fichier `tsconfig.json`, TypeScript trouvera automatiquement les définitions de type pour AVA. Vous devez définir `target` à `es2015` pour utiliser les promesses et les fonctions async.

Consultez la [recette TypeScript](docs/recipes/typescript.md) pour une explication plus détaillée.

#### Transpilation des modules importés

AVA transpile actuellement seulement les tests que vous lui demandez d'exécuter, ainsi que les helpers de test (les fichiers commençant par `_` ou dans le répertoire `helpers`) dans le répertoire de test. *Il ne transpilera pas les modules importés (```import```) depuis le fichier de test.* Cela peut ne pas être ce que vous attendez, mais il y a des solutions de contournement.

Si vous utilisez Babel, vous pouvez utiliser le [hook require](https://babeljs.io/docs/usage/require/) pour transpiler à la volée des modules importés. Pour l'ajouter,  [configurez le dans votre `package.json`](#configuration).

Vous pouvez également transpiler vos modules dans un processus séparé et référer les fichiers transpilés de vos tests plutôt que les sources. Voici un [exemple](docs/recipes/precompiling-with-webpack.md).

### Prise en charge des promesses

Si vous retournez une promesse dans le test, vous n'avez pas besoin de mettre explicitement un end dans le test car il se terminera lorsque la promesse sera résolue.

```js
test(t => {
	return somePromise().then(result => {
		t.is(result, 'unicorn');
	});
});
```

### Prise en charge des fonctions génératrices

AVA est livré avec un support intégré pour les [fonctions génératrices](https://developer.mozilla.org/fr/docs/Web/JavaScript/Reference/Instructions/function*).

```js
test(function * (t) {
	const value = yield generatorFn();
	t.true(value);
});
```

### Prise en charge des fonctions asynchrones

AVA est livré avec un support pour les [fonctions asynchrones](https://tc39.github.io/ecmascript-asyncawait/) *(async/await)*.

```js
test(async function (t) {
	const value = await promiseFn();
	t.true(value);
});

// Fonction fléchée async
test(async t => {
	const value = await promiseFn();
	t.true(value);
});
```

### Prise en charge de Observable

AVA est livré avec un support pour les [observables](https://github.com/zenparsing/es-observable). Si vous retournez un observable dans un test, AVA le consommera automatiquement jusqu'à la fin avant de terminer le test.

*Vous n'avez pas besoin d'utiliser le "mode callback" ou appeler `t.end()`.*

```js
test(t => {
	t.plan(3);
	return Observable.of(1, 2, 3, 4, 5, 6)
		.filter(n => {
			// Seulement les nombres pairs
			return n % 2 === 0;
		})
		.map(() => t.pass());
});
```

### Prise en charge du Callback

AVA prend en charge l'utilisation de `t.end` comme callback final lors de l'utilisation de callback d'API avec le style node d'["erreur-en-premier"](http://thenodeway.io/posts/understanding-error-first-callbacks/). AVA considérera toute valeur truthy passée comme premier argument à `t.end` comme une erreur. Notez que `t.end` exige le "mode callback", qui peut être activé en utilisant le chaînage `test.cb`.

```js
test.cb(t => {
	// `t.end` vérifie automatiquement le premier argument pour l'erreur
	fs.readFile('data.txt', t.end);
});
```

### Délai (timeout) global

Un délai (timeout) global peut être défini via l'option `--timeout`.
Le délai de AVA se comporte différemment des autres frameworks de test.
AVA réinitialise un minuteur après chaque test, cela oblige les tests à s'arrêter, si aucun nouveau résultat de test est reçu dans le délai imparti.

Vous pouvez définir des délais qui soient lisibles :

```console
$ ava --timeout=10s # 10 secondes
$ ava --timeout=2m # 2 minutes
$ ava --timeout=100 # 100 millisecondes
```

## API

### `test([title], implementation)`
### `test.serial([title], implementation)`
### `test.cb([title], implementation)`
### `test.only([title], implementation)`
### `test.skip([title], implementation)`
### `test.todo(title)`
### `test.failing([title], implementation)`
### `test.before([title], implementation)`
### `test.after([title], implementation)`
### `test.beforeEach([title], implementation)`
### `test.afterEach([title], implementation)`

#### `title`

Type: `string`

Titre du test.

#### `implementation(t)`

Type: `function`

Doit contenir le test réel.

##### `t`

Type: `object`

L'objet d'exécution d'un test particulier. Chaque implémentation de test reçoit un objet différent. Il contient les [assertions](#assertions) ainsi que les méthodes `.plan(count)` et `.end()`. `t.context` peut contenir un état partagé depuis le hook `beforeEach`. `t.title` retourne le titre du test.

###### `t.plan(count)`

Détermine le nombre d'assertion qu'il y a dans le test. Le test échouera si le nombre d'assertion réel ne correspond pas au nombre d'assertion prévu. Consultez l'[assertion planifiée](#assertion-planifiée).

###### `t.end()`

La fin du test. Fonctionne uniquement avec `test.cb()`.

## Assertions

Les assertions sont incluses dans l'[objet d'exécution](#t) fourni à chaque implémentation de test :

```js
test(t => {
	t.truthy('unicorn'); // Assertion
});
```

Si plusieurs échecs d'assertion se produisent à l'intérieur d'un seul test, AVA affichera uniquement le *premier*.

### `.pass([message])`

L'assertion passe.

### `.fail([message])`

L'assertion échoue.

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

### `.throws(function|promise, [error, [message]])`

Affirme que `function` lève une erreur, ou rejète `promise` avec une erreur.

`error` peut-être un constructeur d'une erreur, un message d'erreur, une regex qui correspond à un message d'erreur ou une fonction de validation.

Retourne l'erreur levée par `function` ou une promesse avec le motif de rejet de `promise`

Exemple:

```js
const fn = () => {
	throw new TypeError('🦄');
};

test('throws', t => {
	const error = t.throws(() => {
		fn();
	}, TypeError);

	t.is(error.message, '🦄');
});
```

```js
const promise = Promise.reject(new TypeError('🦄'));

test('rejects', async t => {
	const error = await t.throws(promise);
	t.is(error.message, '🦄');
});
```

Lorsque vous testez une promesse, vous devez attendre l'assertion pour terminer :

```js
test('rejects', async t => {
	await t.throws(promise);
});
```

### `.notThrows(function|promise, [message])`

Affirme que `function` ne lève pas `error` ou que `promise` ne rejette pas une erreur.

Comme l'assertion `.throws()`, lorsque vous testez une promesse, vous devez attendre l'assertion pour terminer :

```js
test('rejects', async t => {
	await t.notThrows(promise);
});
```

### `.regex(contents, regex, [message])`

Affirme que `contents` correspond à `regex`.

### `.notRegex(contents, regex, [message])`

Affirme que `contents` ne correspond pas à `regex`.

### `.ifError(error, [message])`

Affirme que `error` est falsy.

### `.snapshot(expected, [message])`
### `.snapshot(expected, [options], [message])`

Compare la valeur `expected` avec avec un instantané enregistré auparavant. Les instantanés sont conservés pour chaque test, donc assurez-vous de donner à vos tests des titres uniques. Sinon, passez un objet `options` pour sélectionner un instantané spécifique, par exemple `{id: 'mon snapshot'}`.

## Test d'instantané

AVA prend en charge les tests instantanés, [comme présentés par Jest](https://facebook.github.io/jest/docs/snapshot-testing.html), à travers son interface [Assertions](#assertions). Vous pouvez faire un instantané de n'importe quelle valeur ainsi que des éléments de React :

```js
// Votre composant
const HelloWorld = () => <h1>Hello World...!</h1>;

export default HelloWorld;
```

```js
// Votre test
import test from 'ava';
import render from 'react-test-renderer';
import HelloWorld from '.';

test('HelloWorld component', t => {
	const tree = render.create(<HelloWorld/>).toJSON();
	t.snapshot(tree);
});
```

[Essayez-le dans ce projet d'exemple.](https://github.com/avajs/ava-snapshot-example)

Les instantanés sont stockés aux côtés de vos fichiers de test. Si vos tests se trouvent dans un dossier `test` ou `tests`, les instantanés seront stockés dans un dossier `snapshots`. Si vos tests se trouvent dans un dossier `__tests__` alors ils seront stockés dans un dossier `__snapshots__`.

Si nous avons un `~/project/test/main.js` qui contient des assertions d'instantané. AVA créera deux fichiers :

* `~/project/test/snapshots/main.js.snap`
* `~/project/test/snapshots/main.js.md`

Le premier dossier contient l'instantané réel et est requis pour les comparaisons futures. Le deuxième fichier contient votre *rapport d'instantané*. Il est régénéré lorsque vous mettez à jour vos instantanés. Si vous le confiez au contrôle des sources, vous pouvez le comparer pour voir les modifications apportées à votre instantané.

AVA montrera pourquoi votre assertion d'instantané a échoué :

<img src="https://github.com/avajs/ava/blob/master/media/snapshot-testing.png" width="1048">

Vous pouvez ensuite vérifier votre code. Si le changement etait intentionel, vous pouvez utiliser l'option `--update-snapshots` (ou `-u`) pour mettre à jour vos instantanés :

```console
$ ava --update-snapshots
```

## Assertions ignorées

Toute assertion peut être ignorée en utilisant le modificateur `skip`. Les assertions ignorées sont encore comptées, donc il n'y a pas besoin de changer le nombre d'assertion dans `plan`.

```js
test(t => {
	t.plan(2);
	t.skip.is(foo(), 5); // Pas besoin de changer le nombre d'assertion dans `plan`.
	t.is(1, 1);
});
```

## Messages d'assertions améliorés

AVA est livré avec l'intégration de [`power-assert`](https://github.com/power-assert-js/power-assert), cela vous donne des messages d'assertion plus clairs. Il lit votre test et tente de déduire plus d'informations à partir du code.

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

Cependant dans AVA, ce test  :

```js
test(t => {
	const a = /foo/;
	const b = 'bar';
	const c = 'baz';
	t.true(a.test(b) || b === c);
});
```

Affichera :

```
t.true(a.test(b) || b === c)
       |      |     |     |
       |      "bar" "bar" "baz"
       false
```

## Isolement du processus

Chaque fichier de test est exécuté dans un processus Node.js séparé. Cela apporte beaucoup d'avantages. Cela vous permet de changer l'état global ou de substituer un seul fichier de test, sans affecter les autres. C'est également bien pour des performances sur les processeurs multi-cœurs modernes, ce qui permet d'exécuter plusieurs fichiers de test en parallèle.

## Astuces

### Les fichiers temporaires

L'exécution des tests en simultané apporte quelques défis, faire de l'IO est le premier.

Habituellement, les tests en série créent juste des répertoires temporaires dans le répertoire de test en cours et les nettoient à la fin. Cela ne fonctionnera pas lorsque vous exécutez des tests en simultané, car les tests seront en conflit les uns avec les autres. La bonne façon de le faire, est d'utiliser un nouveau répertoire temporaire pour chaque test. Les modules [`tempfile`](https://github.com/sindresorhus/tempfile) et [`temp-write`](https://github.com/sindresorhus/temp-write) peuvent être utiles.

### Couverture de code

Vous ne pouvez pas utiliser [`istanbul`](https://github.com/gotwarlost/istanbul) pour la couverture de code car AVA [génère les fichiers de test](#isolement-du-processus). Vous pouvez utiliser [`nyc`](https://github.com/bcoe/nyc) à la place, c'est essentiellement `istanbul` avec en plus la prise en charge des sous-processus.

Depuis la version `5.0.0`, il utilise les sources maps pour faire le rapport de votre couverture de code, indépendamment de la transpilation. Assurez-vous, que le code que vous testez, comprend une source map ou les références d'un fichier source map. Si vous utilisez `babel-register` vous pouvez définir l'option `sourceMaps` dans votre configuration Babel à `inline`.

### Pièges classiques

Nous avons une liste croissante de [pièges classiques](docs/common-pitfalls.md) que vous pouvez rencontrer lors de l'utilisation AVA. Si vous rencontrez des problèmes que vous jugez habituels, commentez les dans [cette issue](https://github.com/avajs/ava/issues/404).

## FAQ

### Pourquoi ne pas utiliser `mocha`, `tape` ou `tap` ?

Mocha vous oblige à utiliser les globales implicites comme `describe` et `it` avec l'interface par défaut (que la plupart des gens utilisent). Il est pas très opiniâtre et exécute des tests en série sans isolation du processus, ce qui le rend lent.

Tape et node-tap sont bons. AVA est fortement inspiré par leur syntaxe. Tous les deux exécutent les tests en série. La restitution de [TAP](https://testanything.org) n'est pas facile à lire donc vous avez toujours besoin de l'aide d'un "reporter" tap.

Au contraire, AVA est très opiniâtre et exécute les tests en simultané, avec des processus distincts pour chaque fichier de test. Son "reporter" par défaut est agréable à regarder et pourtant AVA soutient encore la restitution de TAP à travers une option du CLI.

### Comment l'écrire et le prononcer ?

AVA, pas Ava ni ava. Prononcez [`/ˈeɪvə/` ay-və](https://github.com/avajs/ava/blob/master/media/pronunciation.m4a?raw=true).

### Que représente l'arrière plan de l'image ?

C'est la [galaxie d'Andromède.](https://simple.wikipedia.org/wiki/Andromeda_galaxy).

### Quelle est la différence entre simultanéité et parallélisme

[La simultanéité n'est pas du parallélisme. Il permet le parallélisme.](http://stackoverflow.com/q/1050222).

## Recettes

- [Couverture de code](docs/recipes/code-coverage.md)
- [Mode watch](docs/recipes/watch-mode.md)
- [Tester un endpoint](docs/recipes/endpoint-testing.md)
- [Quand utiliser `t.plan()` ?](docs/recipes/when-to-use-plan.md)
- [Tests de navigateur](docs/recipes/browser-testing.md)
- [TypeScript](docs/recipes/typescript.md)
- [Configuration de Babel](docs/recipes/babelrc.md)
- [Tester les composants React](docs/recipes/react.md)
- [Tester les composants Vue.js](docs/recipes/vue.md)
- [JSPM et SystemJS](docs/recipes/jspm-systemjs.md)
- [Débogage des tests avec Chrome DevTools](docs/recipes/debugging-with-chrome-devtools.md)
- [Débogage des tests avec WebStorm](docs/recipes/debugging-with-webstorm.md)
- [Précompilation des fichiers sources avec webpack](docs/recipes/precompiling-with-webpack.md)
- [Tests d'intégration isolés de MongoDB](docs/recipes/isolated-mongodb-integration-tests.md)

## Support

- [Stack Overflow](https://stackoverflow.com/questions/tagged/ava)
- [Gitter chat](https://gitter.im/avajs/ava)
- [Twitter](https://twitter.com/ava__js)

## En relation

- [eslint-plugin-ava](https://github.com/avajs/eslint-plugin-ava) - Règles de Lint pour les tests AVA
- [sublime-ava](https://github.com/avajs/sublime-ava) - Snippets pour les tests AVA
- [atom-ava](https://github.com/avajs/atom-ava) - Snippets pour les tests AVA
- [vscode-ava](https://github.com/samverschueren/vscode-ava) - Snippets pour les tests AVA
- [gulp-ava](https://github.com/avajs/gulp-ava) - Exécutez les tests avec gulp
- [grunt-ava](https://github.com/avajs/grunt-ava) - Exécutez les tests avec grunt
- [Plus…](https://github.com/avajs/awesome-ava#packages)

## Liens

- [Acheter des stickers AVA](https://www.stickermule.com/user/1070705604/stickers)
- [La liste Awesome](https://github.com/avajs/awesome-ava)
- [AVA Casts](http://avacasts.com)
- [Plus…](https://github.com/avajs/awesome-ava)

## L'équipe

[![Sindre Sorhus](https://github.com/sindresorhus.png?size=100)](https://github.com/sindresorhus) | [![Vadim Demedes](https://github.com/vadimdemedes.png?size=100)](https://github.com/vadimdemedes) | [![James Talmage](https://github.com/jamestalmage.png?size=100)](https://github.com/jamestalmage) | [![Mark Wubben](https://github.com/novemberborn.png?size=100)](https://github.com/novemberborn) | [![Juan Soto](https://github.com/sotojuan.png?size=100)](https://github.com/sotojuan) | [![Jeroen Engels](https://github.com/jfmengels.png?size=100)](https://github.com/jfmengels)
---|---|---|---|---|---
[Sindre Sorhus](http://sindresorhus.com) | [Vadim Demedes](https://github.com/vadimdemedes) | [James Talmage](https://github.com/jamestalmage) | [Mark Wubben](https://novemberborn.net) | [Juan Soto](http://juansoto.me) | [Jeroen Engels](https://github.com/jfmengels)

### Auparavant dans l'équipe

- [Kevin Mårtensson](https://github.com/kevva)


<div align="center">
	<br>
	<br>
	<br>
	<a href="https://ava.li">
		<img src="https://cdn.rawgit.com/avajs/ava/fe1cea1ca3d2c8518c0cc39ec8be592beab90558/media/logo.svg" width="200" alt="AVA">
	</a>
	<br>
	<br>
</div>
