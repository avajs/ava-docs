___
**Note du traducteur**

C'est la traduction du fichier [readme.md](https://github.com/sindresorhus/ava/blob/master/readme.md). Voici un [lien](https://github.com/sindresorhus/ava/compare/4c69c796d53f56b20de0d2c75d90366b7a32e029...master#diff-0730bb7c2e8f9ea2438b52e419dd86c9) vers les différences avec le master de AVA (Si en cliquant sur le lien, vous ne trouvez pas le fichier `readme.md` parmi les fichiers modifiés, vous pouvez donc en déduire que la traduction est à jour).
___
# ![AVA](https://github.com/sindresorhus/ava/blob/master/media/header.png)

> Lanceur de test futuriste

[![Build Status: Linux](https://travis-ci.org/sindresorhus/ava.svg?branch=master)](https://travis-ci.org/sindresorhus/ava) [![Build status: Windows](https://ci.appveyor.com/api/projects/status/igogxrcmhhm085co/branch/master?svg=true)](https://ci.appveyor.com/project/sindresorhus/ava/branch/master) [![Coverage Status](https://coveralls.io/repos/sindresorhus/ava/badge.svg?branch=master&service=github)](https://coveralls.io/github/sindresorhus/ava?branch=master) [![Gitter](https://badges.gitter.im/join_chat.svg)](https://gitter.im/sindresorhus/ava)

Même si JavaScript est mono-thread, l'IO dans Node.js peut se lancer en parallèle en raison de sa nature asynchrone. AVA profite de cela et exécute vos tests en même temps, ce qui est particulièrement avantageux pour les tests lourds d'IO. De plus, les fichiers de test sont exécutés en parallèle comme des processus séparés, cela vous donne encore de meilleures performances et un environnement isolé pour chaque fichier de test. Le [passage](https://github.com/sindresorhus/pageres/commit/663be15acb3dd2eb0f71b1956ef28c2cd3fdeed0) de Mocha à AVA dans Pageres a diminué la durée des tests de 31 à 11 secondes. Comme les tests sont exécutés simultanément, cela vous oblige à écrire des tests [atomiques](https://fr.wikipedia.org/wiki/Atomicit%C3%A9_%28informatique%29), ce qui signifie que les tests ne dépendent pas de l'état global ou de l'état des autres tests, ce qui est une bonne chose !

*Lisez notre [guide de contribution](contributing.md) si vous souhaitez contribuer (issues/PRs/etc).*

Suivez le [compte Twitter de AVA](https://twitter.com/ava__js) pour les mises à jour.

Traductions : [Español](https://github.com/sindresorhus/ava-docs/blob/master/es_ES/readme.md), [Français](https://github.com/sindresorhus/ava-docs/blob/master/fr_FR/readme.md), [Italiano](https://github.com/sindresorhus/ava-docs/blob/master/it_IT/readme.md), [日本語](https://github.com/sindresorhus/ava-docs/blob/master/ja_JP/readme.md), [Português](https://github.com/sindresorhus/ava-docs/blob/master/pt_BR/readme.md), [Русский](https://github.com/sindresorhus/ava-docs/blob/master/ru_RU/readme.md), [简体中文](https://github.com/sindresorhus/ava-docs/blob/master/zh_CN/readme.md)

## Table des matières

- [Utilisation](#utilisation)
- [Utilisation du CLI](#cli)
- [Configuration](#configuration)
- [Documentation](#documentation)
- [API](#api)
- [Assertions](#assertions)
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
- [Environnement isolé pour chaque fichier de test](#isolement-du-processus)
- [Écriture de vos tests en ES2015](#prise-en-charge-de-es2015)
- [Prise en charge des promesses](#prise-en-charge-des-promesses)
- [Prise en charge des fonctions génératrices](#prise-en-charge-des-fonctions-génératrices)
- [Prise en charge des fonctions asynchrones](#prise-en-charge-des-fonctions-asynchrones)
- [Prise en charge d'Observable](#prise-en-charge-de-observable)
- [Messages d'assertions améliorés](#messages-dassertions-améliorés)
- [Sortie facultative au format TAP](#sortie-facultative-au-format-tap)
- [Nettoyage de la stack trace](#nettoyage-de-la-stack-trace)

## Syntaxe d'un Test

```js
import test from 'ava';

test(t => {
	t.deepEqual([1, 2], [1, 2]);
});
```

## Utilisation

### Ajoutez AVA à votre projet

Installez globalement AVA, puis lancez le avec `--init` pour ajouter AVA à votre `package.json` :

```console
$ npm install --global ava
$ ava --init
```

```json
{
  "name": "awesome-package",
  "scripts": {
    "test": "ava"
  },
  "devDependencies": {
    "ava": "^0.11.0"
  }
}
```

Tous les arguments passés après `--init` sont ajoutés dans le `package.json`.

#### Installation manuelle

Vous pouvez également installer AVA directement :

```console
$ npm install --save-dev ava
```

Vous devez configurer le script `test` dans votre `package.json` pour utiliser `ava`
(Voir ci-dessus).

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

<img src="https://github.com/sindresorhus/ava/blob/master/screenshot.png" width="150" align="right">

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

![](https://github.com/sindresorhus/ava/blob/master/screenshot-mini-reporter.gif)

```console
$ ava --help

  Usage (Utilisation)
    ava [<file|directory|glob> ...]

  Options
    --init           Add AVA to your project (Ajouter AVA à votre projet)
    --fail-fast      Stop after first test failure (Arrêter dès qu'un test échoue)
    --serial, -s     Run tests serially (Lancer les tests  )
    --require, -r    Module to preload (Can be repeated) (Module à précharger (peut être répété))
    --tap, -t        Generate TAP output (Générer une sortie au format TAP)
    --verbose, -v    Enable verbose output (Activer le mode verbose)
    --no-cache       Disable the transpiler cache (Désactive le cache du transpileur)
    --match, -m      Only run tests with matching title (Can be repeated) (Exécute seulement les tests qui correspondent au titre (peut être répété))
		--watch, -w      Re-run tests when tests and source files change (Re-exécute les tests quand les tests et les fichiers sources ont changé)
    --source, -S     Pattern to match source files so tests can be re-run (Can be repeated) (Pattern pour rechercher les fichiers sources afin de re-exécuter les tests (peut être répété))
    --timeout, -T    Set global timeout (Définit un timeout global)

  Examples (Exemples)
    ava
    ava test.js test2.js
    ava test-*.js
    ava test
    ava --init
    ava --init foo.js

  Default patterns when no arguments (Les patterns par défaut quand il n'y a pas d'arguments):
  test.js test-*.js test/**/*.js **/__tests__/**/*.js **/*.test.js
```

*Notez que le CLI utilisera votre installation locale de AVA lorsqu'il est disponible, même lorsqu'il est exécuté de manière globale.*

Les répertoires sont récursifs, où tous les fichiers `*.js` sont traités comme des fichiers de test. Les répertoires nommés `fixtures`, `helpers` et `node_modules` sont *toujours* ignorés. C'est aussi le cas pour les fichiers commençant par `_`, cela vous permet de placer des helpers dans le même répertoire que vos fichiers de test.

Lors de l'utilisation de `npm test`, vous pouvez passer directement des arguments `npm test test2.js`, mais pour les options, vous devez les passez ainsi `npm test -- --verbose`.

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
    "failFast": true,
    "tap": true,
    "require": [
      "babel-register"
		],
    "babel": "inherit"
  }
}
```

Les arguments passés au CLI seront toujours prioritaires sur ceux de la configuration dans `package.json`.

Voir la section [prise en charge de ES2015](#prise-en-charge-de-es2015) pour plus de détails sur l'option `babel`.

## Documentation

Les tests sont exécutés simultanément. Vous pouvez spécifier des tests synchrones et asynchrones. Les tests sont considérés synchrones sauf si vous retournez une promesse ou un [observable](https://github.com/zenparsing/zen-observable).

Nous recommandons *fortement* l'utilisation de [fonctions asynchrones](#prise-en-charge-des-fonctions-asynchrones). Elles rendent le code asynchrone concis et lisible, et elles retournent implicitement une promesse, donc vous n'avez pas à le faire.

Si vous n'avez pas la possibilité d'utiliser des promesses ou des observables, vous pouvez activer le "mode callback" en définissant votre test avec `test.cb([title], fn)`. Les tests déclarés de cette manière **doivent** être terminés manuellement avec `t.end()`. Ce mode est principalement destiné pour tester les APIs de style callback.

Vous devez définir tous les tests de manière synchrone. Ils ne peuvent pas être définis à l'intérieur de `setTimeout`, `setImmediate`, etc.

Les fichiers de test sont exécutés à partir de leur répertoire courant, donc [`process.cwd()`](https://nodejs.org/api/process.html#process_process_cwd) est toujours identique à [`__dirname`](https://nodejs.org/api/globals.html#globals_dirname). Vous pouvez simplement utiliser des chemins relatifs au lieu de faire `path.join(__dirname, 'relative/path')`.

### Création des tests

Pour créer un test, vous appelez la fonction `test` que vous importez de AVA. Fournissez un titre facultatif et une fonction d'implémentation. La fonction sera appelée lorque votre test sera exécutée. Un [objet d'exécution](#t) est passé comme premier et unique argument. Par convention cet argument est nommé `t`.

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
}); // échec, 3 assertions de trop sont exécutées

test(t => {
	t.plan(1);

	someAsyncFunction(() => {
		t.pass();
	});
}); // échec, le test se termine de façon synchrone avant que l'assertion soit exécutée
```

### Exécution des tests en série

Par défaut les tests sont exécutés simultanément, ce qui est génial. Parfois, vous devez écrire des tests qui ne peuvent pas fonctionner en même temps.

Dans ces rares cas, vous pouvez utiliser `.serial`. Cela forcera les tests à s'exécuter en série *avant* ceux qui s'exécutent en simultané.

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

`.only` s'applique à tous les fichiers de test, donc si vous l'utilisez que dans un seul fichier, les tests des autres fichiers ne seront pas exécutés.

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

// ne s'exécutera pas, car n'a pas de titre
test(function (t) {
	t.fail();
});

// ne s'exécutera pas, car n'a pas de titre explicite
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

### Les [hooks](https://fr.wikipedia.org/wiki/Hook_%28informatique%29) before & after

AVA vous permet d'enregistrer des hooks qui sont exécutés avant et après vos tests. Cela vous permet d'exécuter le code de configuration et/ou de déconfiguration.

`test.before()` enregistre un hook qui sera exécuté avant le premier test dans votre fichier de test. De même, `test.after()` enregistre un hook qui sera exécuté après le dernier test.

`test.beforeEach()` enregistre un hook qui sera exécuté avant chaque test dans votre fichier de test. De même, `test.afterEach()` enregistre un hook qui sera exécuté après chaque test.

Comme `test()`, ces méthodes prennent comme arguments un titre facultatif et une fonction callback. Le titre est affiché si votre hook ne parvient pas à s'exécuter. Le callback est appelé avec un [objet d'exécution](#t).

Les hooks `before` s'exécutent avant les hooks `beforeEach`. Les hooks `afterEach` s'exécutent avant les hooks `after`. Au sein de leur catégorie, les hooks s'exécutent dans l'ordre où ils ont été définis.

```js
test.before(t => {
	// cela s'exécute avant tous les tests
});

test.before(t => {
	// cela s'exécute après ce qui précède, mais avant les tests
});

test.after('cleanup', t => {
	// cela s'exécute après tous les tests
});

test.beforeEach(t => {
	// cela s'exécute avant chaque test
});

test.afterEach(t => {
	// cela s'exécute après chaque test
});

test(t => {
	// test normal
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

Gardez à l'esprit que les hooks `beforeEach` et `afterEach` s'exécutent juste avant et après un test qui s'éxécute, et que par défaut les tests s'exécutent simultanément. Si vous avez besoin de mettre en place un état global pour chaque test ([par exemple](https://github.com/sindresorhus/ava/issues/560) en espionnant `console.log`), vous devez vous assurer que les tests sont [exécutés en série](#exécution-des-tests-en-série).

Rappelez-vous que AVA exécute chaque fichier de test dans son propre processus. Vous n'avez pas besoin de nettoyer l'état global dans le hook `after`, puisqu'on l'appelle seulement lorqu'on sort du processus.

Les hooks `beforeEach` & `afterEach` peuvent partager le contexte avec le test :

```js
test.beforeEach(t => {
	t.context.data = generateUniqueData();
});

test(t => {
	t.is(t.context.data + 'bar', 'foobar');
});
```

Par défaut `t.context` est un objet, mais vous pouvez directement le réassigné :

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

### Personnaliser les assertions

Vous pouvez utiliser une bibliothèque d'assertion qui remplace ou s'ajoute à celui qui est fourni, à condition qu'il renvoie des exceptions lorsque l'assertion échoue.

Cela ne vous donnera pas une aussi bonne expérience que celle obtenue avec les [assertions intégrées](#assertions), et vous ne serez pas en mesure d'utiliser l'[assertion planifiée](#assertion-planifiée) ([voir #25](https://github.com/sindresorhus/ava/issues/25)).

```js
import assert from 'assert';

test(t => {
	assert(true);
});
```

### Prise en charge de ES2015

AVA est livré avec un support intégré pour ES2015 via [Babel 6](https://babeljs.io). Il suffit d'écrire vos tests en ES2015. Aucune configuration supplémentaire n'est nécessaire. Vous pouvez utiliser n'importe quelle version de Babel dans votre projet. Nous utilisons notre propre bundle Babel avec les presets [`es2015`](http://babeljs.io/docs/plugins/preset-es2015/) et [`stage-2`](http://babeljs.io/docs/plugins/preset-stage-2/) ainsi que les plugins [`espower`](https://github.com/power-assert-js/babel-plugin-espower) et [`transform-runtime`](https://babeljs.io/docs/plugins/transform-runtime/).

La configuration de Babel correspondante à la configuration de AVA est la suivante :

```json
{
  "presets": [
    "es2015",
    "stage-2"
  ],
  "plugins": [
    "espower",
    "transform-runtime"
  ]
}
```

Vous pouvez personnaliser la façon dont AVA transpiles les fichiers de test grâce à l'option `babel` dans la [configuration du `package.json`](#configuration) de AVA. Par exemple, pour remplacer les presets, vous pouvez utiliser:

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
	},
}
```

Vous pouvez également utiliser le mot-clé `"inherit"`. Cela permet à AVA de reporter la configuration de Babel dans votre [fichier `.babelrc` ou `package.json`](https://babeljs.io/docs/usage/babelrc/). De cette façon, vos fichiers de test seront transpilé en utilisant la même configuration que vos fichiers sources sans avoir à le répéter pour AVA :

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
	},
}
```

Consultez la [recette `.babelrc`](docs/recipes/babelrc.md) de AVA pour d'autres exemples et une explication plus détaillée des options de configuration.

Notez que AVA appliquera *toujours* [quelques plugins internes](docs/recipes/babelrc.md#notes) quelle que soit la configuration, mais ils ne doivent pas affecter le comportement de votre code.

### Prise en charge de TypeScript

AVA contient les typages pour TypeScript. Vous devez configurer la transpilation vous-même. Lorsque vous définissez `module` à `commonjs` dans votre fichier `tsconfig.json`, TypeScript trouvera automatiquement les définitions de type pour AVA. Vous devez définir `target` à `es2015` pour utiliser les promesses et les fonctions async.

#### Transpilation des modules importés

AVA transpile actuellement seulement les tests que vous lui demandez d'exécuter. *Il ne transpilera pas les modules importés (```import```) depuis le fichier de test.* Cela peut ne pas être ce que vous attendez, mais il y a des solutions de contournement.

Si vous utilisez Babel, vous pouvez utiliser le [hook require](https://babeljs.io/docs/usage/require/) pour transpiler à la volée des modules importés. Exécutez AVA avec `--require babel-register` (consulter le [CLI](#cli)) ou [configurez le dans votre `package.json`](#configuration).

Vous pouvez également transpiler vos modules dans un processus séparé et référer les fichiers transpilés de vos tests plutôt que les sources.

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
			// seulement les nombres pairs
			return n % 2 === 0;
		})
		.map(() => t.pass());
});
```

### Prise en charge du Callback

AVA prend en charge l'utilisation de `t.end` comme callback final lors de l'utilisation de callback d'API avec le style node d'["erreur-en-premier"](http://thenodeway.io/posts/understanding-error-first-callbacks/). AVA considérera toute valeur truthy passée comme premier argument à `t.end` comme une erreur. Notez que `t.end` exige le "mode callback", qui peut être activé en utilisant le chainage `test.cb`.

```js
test.cb(t => {
	// t.end vérifie automatiquement le premier argument pour l'erreur
	fs.readFile('data.txt', t.end);
});
```

### Sortie facultative au format TAP

AVA peut générer une sortie au format TAP via l'option `--tap` pour utiliser un ["reporter TAP"](https://github.com/sindresorhus/awesome-tap#reporters).

```console
$ ava --tap | tap-nyan
```

<img src="https://github.com/sindresorhus/ava/blob/master/media/tap-output.png" width="398">

Veuillez noter que le reporter TAP est indisponible lors de l'utilisation du [mode watch](#scrutez-le-test).

### Nettoyage de la stack trace

AVA supprime automatiquement les lignes sans rapport dans la stack trace, cela permet de trouver la source d'une erreur plus rapidement.

<img src="https://github.com/sindresorhus/ava/blob/master/media/stack-traces.png" width="300">

### Délai (timeout) global

Un délai (timeout) global peut être défini via l'option `--timeout`.
Le délai de AVA se comporte différemment des autres frameworks de test.
AVA réinitialise un minuteur après chaque test, cela oblige les tests à s'arrêter, si aucun nouveau résultat de test est reçu dans le délai imparti.

Vous pouvez définir des délais qui soient lisibles :

```
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

L'objet d'exécution d'un test particulier. Chaque implémentation de test reçoit un objet différent. Il contient les [assertions](#assertions) ainsi que les méthodes `.plan(count)` et `.end()`. `t.context` peut contenir un état partagé depuis le hook `beforeEach`.

###### `t.plan(count)`

Détermine le nombre d'assertion qu'il y a dans le test. Le test échouera si le nombre d'assertion réel ne correspond pas au nombre d'assertion prévu. Consultez l'[assertion planifiée](#assertion-planifiée).

###### `t.end()`

La fin du test. Fonctionne uniquement avec `test.cb()`.

## Assertions

Les assertions sont incluses dans l'[objet d'exécution](#t) fourni à chaque implémentation de test :

```js
test(t => {
	t.truthy('unicorn'); // assertion
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

Affirme que `value` est égal à `expected`.

### `.not(value, expected, [message])`

Affirme que `value` n'est pas égal à `expected`.

### `.deepEqual(value, expected, [message])`

Affirme que `value` est deep equal à `expected`.

### `.notDeepEqual(value, expected, [message])`

Affirme que `value` n'est pas deep equal à `expected`.

### `.throws(function|promise, [error, [message]])`

Affirme que `function` lève une erreur, ou rejète `promise` avec une erreur.

`error` peut-être un constructeur, une regex, un message d'erreur ou une fonction de validation.

Retourne l'erreur levée par `function` ou le motif du rejet de la `promise`

### `.notThrows(function|promise, [message])`

Affirme que `function` ne lève pas `error` ou résout `promise`.

### `.regex(contents, regex, [message])`

Affirme que `contents` correspond à `regex`.

### `.ifError(error, [message])`

Affirme que `error` est falsy.

## Assertions ignorées

Toute assertion peut être ignorée en utilisant le modificateur `skip`. Les assertions ignorées sont encore comptées, donc il n'y a pas besoin de changer le nombre d'assertion dans `plan`.

```js
+test(t => {
	t.plan(2);
	t.skip.is(foo(), 5); // pas besoin de changer le nombre d'assertion dans `plan`.
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

### Débogage

AVA exécute par défaut les tests en simultané, ce qui n'est pas optimal lorsque vous avez besoin de déboguer quelque chose. Pour cela, exécuter les tests en série avec l'option `--serial` :

```console
$ ava --serial
```

### Couverture de code

Vous ne pouvez pas utiliser [`istanbul`](https://github.com/gotwarlost/istanbul) pour la couverture de code car AVA [génère les fichiers de test](#isolement-du-processus). Vous pouvez utiliser [`nyc`](https://github.com/bcoe/nyc) à la place, c'est essentiellement `istanbul` avec en plus la prise en charge des sous-processus.

Depuis la version `5.0.0`, il utilise les source maps pour faire le rapport de votre couverture de code, indépendamment de la transpilation. Assurez-vous, que le code que vous testez, comprend une source map ou les références d'un fichier source map. Si vous utilisez `babel-register` vous pouvez définir l'option `sourceMaps` dans votre configuration Babel à `inline`.

## FAQ

### Pourquoi ne pas utiliser `mocha`, `tape` ou `tap` ?

Mocha vous oblige à utiliser les globales implicites comme `describe` et `it` avec l'interface par défaut (que la plupart des gens utilisent). Il est pas très opiniâtre et exécute des tests en série sans isolation du processus, ce qui le rend lent.

Tape et node-tap sont bons. AVA est fortement inspiré par leur syntaxe. Tous les deux exécutent les tests en série. La restitution de [TAP](https://testanything.org) n'est pas facile à lire donc vous avez toujours besoin de l'aide d'un "reporter" tap.

Au contraire, AVA est très opiniâtre et exécute les tests en simultané, avec des processus distincts pour chaque fichier de test. Son "reporter" par défaut est agréable à regarder et pourtant AVA soutient encore la restitution de TAP à travers une option du CLI.

### Comment puis-je utiliser des "reporters" personnalisés ?

AVA prend en charge le format de TAP et est compatible avec tous les [reporters de TAP](https://github.com/sindresorhus/awesome-tap#reporters). Utilisez l'[option `--tap`](#sortie-facultative-au-format-tap) pour activer la restitution de TAP.

### Comment l'écrire et le prononcer ?

AVA, pas Ava ni ava. Prononcez [`/ˈeɪvə/` ay-və](https://github.com/sindresorhus/ava/blob/master/media/pronunciation.m4a?raw=true).

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

## Support

- [Stack Overflow](https://stackoverflow.com/questions/tagged/ava)
- [Gitter chat](https://gitter.im/sindresorhus/ava)
- [Twitter](https://twitter.com/ava__js)

## En relation

- [sublime-ava](https://github.com/sindresorhus/sublime-ava) - Snippets pour les tests AVA
- [atom-ava](https://github.com/sindresorhus/atom-ava) - Snippets pour les tests AVA
- [vscode-ava](https://github.com/samverschueren/vscode-ava) - Snippets pour les tests AVA
- [eslint-plugin-ava](https://github.com/sindresorhus/eslint-plugin-ava) - Règles de Lint pour les tests AVA
- [gulp-ava](https://github.com/sindresorhus/gulp-ava) - Exécutez les tests avec gulp
- [grunt-ava](https://github.com/sindresorhus/grunt-ava) - Exécutez les tests avec grunt
- [fly-ava](https://github.com/pine613/fly-ava) - Exécutez les tests avec fly
- [start-ava](https://github.com/start-runner/ava) - Exécutez les tests avec start

[Plus...](https://github.com/sindresorhus/awesome-ava#packages)

## Liens

- [Acheter des stickers AVA](https://www.stickermule.com/user/1070705604/stickers)
- [La liste Awesome](https://github.com/sindresorhus/awesome-ava)

## L'équipe

[![Sindre Sorhus](https://avatars.githubusercontent.com/u/170270?s=130)](http://sindresorhus.com) | [![Vadim Demedes](https://avatars.githubusercontent.com/u/697676?s=130)](https://github.com/vdemedes) | [![James Talmage](https://avatars.githubusercontent.com/u/4082216?s=130)](https://github.com/jamestalmage) | [![Mark Wubben](https://avatars.githubusercontent.com/u/33538?s=130)](https://novemberborn.net) | [![Juan Soto](https://avatars.githubusercontent.com/u/8217766?s=130)](https://juansoto.me)
---|---|---|---|---|---
[Sindre Sorhus](http://sindresorhus.com) | [Vadim Demedes](https://github.com/vdemedes) | [James Talmage](https://github.com/jamestalmage) | [Mark Wubben](https://novemberborn.net) | [Juan Soto](https://juansoto.me)

### Auparavant dans l'équipe

- [Kevin Mårtensson](https://github.com/kevva)

<div align="center">
	<br>
	<br>
	<br>
	<img src="https://cdn.rawgit.com/sindresorhus/ava/fe1cea1ca3d2c8518c0cc39ec8be592beab90558/media/logo.svg" width="200" alt="AVA">
	<br>
	<br>
</div>
