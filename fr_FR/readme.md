___
**Note du traducteur**

C'est la traduction du fichier [readme.md](https://github.com/sindresorhus/ava/blob/master/readme.md). Voici un [lien](https://github.com/sindresorhus/ava/compare/780d48f89906257910c1999514a2c980ed5f303c...master#diff-0730bb7c2e8f9ea2438b52e419dd86c9) vers les différences avec le master de AVA (Si en cliquant sur le lien, vous ne trouvez pas le fichier `readme.md` parmi les fichiers modifiés, vous pouvez donc en déduire que la traduction est à jour).
___
# ![AVA](https://github.com/sindresorhus/ava/blob/master/media/header.png)

> Lanceur de test futuriste

[![Build Status: Linux](https://travis-ci.org/sindresorhus/ava.svg?branch=master)](https://travis-ci.org/sindresorhus/ava) [![Build status: Windows](https://ci.appveyor.com/api/projects/status/igogxrcmhhm085co/branch/master?svg=true)](https://ci.appveyor.com/project/sindresorhus/ava/branch/master) [![Coverage Status](https://coveralls.io/repos/sindresorhus/ava/badge.svg?branch=master&service=github)](https://coveralls.io/github/sindresorhus/ava?branch=master) [![Gitter](https://img.shields.io/badge/Gitter-Join_the_AVA_chat_%E2%86%92-00d06f.svg)](https://gitter.im/sindresorhus/ava)

Même si JavaScript est mono-thread, l'IO dans Node.js peut se lancer en parallèle en raison de sa nature asynchrone. AVA profite de cela et exécute vos tests en même temps, ce qui est particulièrement avantageux pour les tests lourds d'IO. De plus, les fichiers de test sont exécutés en parallèle comme des processus séparés, cela vous donne encore de meilleures performances et un environnement isolé pour chaque fichier de test. Le [passage](https://github.com/sindresorhus/pageres/commit/663be15acb3dd2eb0f71b1956ef28c2cd3fdeed0) de Mocha à AVA dans Pageres a diminué la durée des tests de 31 à 11 secondes. Comme les tests sont exécutés simultanément, cela vous oblige à écrire des tests [atomiques](https://fr.wikipedia.org/wiki/Atomicit%C3%A9_%28informatique%29), ce qui signifie que les tests ne dépendent pas de l'état global ou de l'état des autres tests, ce qui est une bonne chose !

*Lisez notre [guide de contribution](contributing.md) si vous souhaitez contribuer (issues/PRs/etc).*

Suivez le [compte Twitter de AVA](https://twitter.com/ava__js) pour les mises à jour.

Traductions : [Español](https://github.com/sindresorhus/ava-docs/blob/master/es_ES/readme.md), [Français](https://github.com/sindresorhus/ava-docs/blob/master/fr_FR/readme.md), [日本語](https://github.com/sindresorhus/ava-docs/blob/master/ja_JP/readme.md), [Português](https://github.com/sindresorhus/ava-docs/blob/master/pt_BR/readme.md)


## Table des matières

- [Utilisation](#utilisation)
- [Utilisation du CLI](#cli)
- [Configuration](#configuration)
- [Documentation](#documentation)
- [API](#api)
- [Assertions](#assertions)
- [FAQ](#faq)
- [Recettes](#recettes)


## Pourquoi AVA?

- Minimal et rapide
- Syntaxe de test simple
- Exécution des tests simultanément
- Application de l'écriture des tests atomiques
- Aucunes globales implicites
- [Environnement isolé pour chaque fichier de test](#environnement-isolé)
- [Écriture de vos tests en ES2015](#prise-en-charge-de-es2015)
- [Prise en charge des promesses](#prise-en-charge-des-promesses)
- [Prise en charge des fonctions génératrices](#prise-en-charge-des-fonctions-génératrices)
- [Prise en charge des fonctions asynchrones](#prise-en-charge-des-fonctions-asynchrones)
- [Prise en charge d'Observable](#prise-en-charge-de-observable)
- [Asserts améliorées](#asserts-améliorées)
- [Sortie facultative au format TAP](#sortie-facultative-au-format-tap)
- [Nettoyage de la stack trace](#nettoyage-de-la-stack-trace)


## Syntaxe d'un Test

```js
import test from 'ava';

test(t => {
	t.same([1, 2], [1, 2]);
});
```


## Utilisation

#### Initialisez

Installez globalement AVA `$ npm install --global ava` et lancez `$ ava --init` (avec des options) pour ajouter AVA à votre package.json ou en créer un.

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

#### Exécutez le test

```
$ npm test
```


## CLI

```
$ ava --help

  Usage (Utilisation)
    ava [<file|folder|glob> ...]

  Options
    --init           Add AVA to your project (Ajouter AVA à votre projet)
    --fail-fast      Stop after first test failure (Arrêter dès qu'un test échoue)
    --serial, -s     Run tests serially (Lancer les tests  )
    --require, -r    Module to preload (Can be repeated) (Module à précharger (peut être répété))
    --tap, -t        Generate TAP output (Générer une sortie au format TAP)
    --verbose, -v    Enable verbose output (Activer le mode verbose)
    --no-cache       Disable the transpiler cache (Désactive le cache du transpileur)

  Examples (Exemples)
    ava
    ava test.js test2.js
    ava test-*.js
    ava test
    ava --init
    ava --init foo.js

  Default patterns when no arguments (Les patterns par défaut quand il n'y a pas d'arguments):
  test.js test-*.js test/**/*.js
```

Les répertoires sont par défaut récursifs. Les répertoires nommés `fixtures` et `helpers` sont ignorés, de la même manière que les fichiers qui commencent par `_`. Cela peut être utile pour inclure des helpers dans le même répertoire que vos fichiers de test.

Lors de l'utilisation de `npm test`, vous pouvez passer directement des arguments `npm test test2.js`, mais pour les options, vous devez les passez ainsi `npm test -- --verbose`.

*AVERTISSEMENT : Comportement non-standard :* Le CLI de AVA essaiera toujours d'utiliser l'installation AVA locale dans vos projets. Cela est vrai même lorsque vous exécutez la commande globale `ava`. Ce comportement non-standard résout une importante [issue](https://github.com/sindresorhus/ava/issues/157) et ne devrait avoir aucun impact sur l'utilisation quotidienne.

## Configuration

Toutes les options du CLI peuvent être configurés dans la section `ava` de votre `package.json`. Cela vous permet de modifier le comportement par défaut de la commande `ava`, ainsi vous n'avez plus besoin à chaque fois de taper les mêmes options sur l'invite de commande.

```json
{
  "ava": {
    "files": [
      "my-test-folder/*.js",
      "!**/not-this-file.js"
    ],
    "failFast": true,
    "serial": true,
    "tap": true,
    "verbose": true,
    "require": [
      "babel-core/register", "coffee-script/register"
    ]
  }
}
```

Les arguments passés au CLI seront toujours prioritaires sur ceux de la configuration dans `package.json`.

## Documentation

Les tests sont exécutés de façon asynchrone, cela vous oblige à retourner un objet async supporté (une promesse ou un [observable](https://github.com/zenparsing/zen-observable)). Nous recommandons *fortement* l'utilisation de [fonctions asynchrones](#prise-en-charge-des-fonctions-asynchrones). Elles rendent le code asynchrone concis et lisible, et elles retournent implicitement une promesse, donc vous n'avez pas à le faire.

Si vous ne retournez pas l'un des objets async supportés, mentionnés ci-dessus, le test est considéré comme synchrone et se termine immédiatement.

Si vous n'avez pas la possibilité d'utiliser les promesses ou les autres objets async supportés, vous pouvez activer le "mode callback" en définissant votre test avec `test.cb([title], fn)`. Les tests déclarés de cette manière **doivent** être terminés manuellement avec `t.end()`. Ce mode est principalement destiné pour tester les APIs de style callback.

Vous devez définir tous les tests de manière synchrone. Ils ne peuvent pas être définis à l'intérieur de `setTimeout`, `setImmediate`, etc.

Les fichiers de test sont exécutés à partir de leur répertoire courant, donc [`process.cwd()`](https://nodejs.org/api/process.html#process_process_cwd) est toujours identique à [`__dirname`](https://nodejs.org/api/globals.html#globals_dirname). Vous pouvez simplement utiliser des chemins relatifs au lieu de faire `path.join(__dirname, 'relative/path')`.

### L'anatomie du test

Pour créer un test, vous appelez la fonction `test` que vous avez obtenu (`require`) de AVA, vous lui passez un nom de test facultatif et une fonction contenant l'exécution du test. La fonction passée est donnée au contexte comme premier argument, où vous pouvez appeler les différentes méthodes et [assertions](#assertions) de AVA.

```js
test('name', t => {
	t.pass();
});
```

### Nom facultatif du test

La dénomination du test est facultative, mais nous vous recommandons d'en utiliser une si vous avez plusieurs tests.

```js
test(t => {
	t.pass();
});
```

Vous pouvez également choisir à la place d'utiliser une fonction nommée :

```js
test(function name(t) {
	t.pass();
});
```

### Assertion planifiée

Une assertion **plan**-ifiée peut être utilisée pour s'assurer qu'un certain nombre d'assertions soient faites. Dans le scénario le plus courant, il confirme que le test n'est pas sorti avant d'exécuter le nombre prévu d'assertions. Il fait également échouer le test si trop d'assertions sont exécutées, ce qui peut être utile si vous avez des assertions à l'intérieur des callbacks ou des boucles.

Cela se traduira par un test réussi :

```js
test(t => {
	t.plan(1);

	return Promise.resolve(3).then(n => {
		t.is(n, 3);
	});
});

test.cb(t => {
	setTimeout(() => {
		t.pass();
		t.end();
	}, 100);
});
```

#### AVERTISSEMENT : modifications récentes qui change le comportement

AVA ne supporte plus les tests qui se terminent automatiquement via `t.plan(...)`. Cela permet d'éviter les faux positifs si vous ajoutez des assertions et que vous oubliez d'augmenter le nombre pour plan.

```js
// Ceci ne fonctionne plus

test('auto ending is dangerous', t => {
	t.plan(2);

	t.pass();
	t.pass();

	// la fin automatique après avoir atteint les deux assertions prévues manquera ce dernier test
	setTimeout(() => t.fail(), 10000);
});
```

Pour que cela fonctionne, vous devez utiliser le "mode callback" et appelez explicitement `t.end()`.

```js
test.cb('end explicite pour vos tests', t => {
	t.plan(2);

	t.pass();
	t.pass();

	setTimeout(() => {
		// Cet échec est maintenant pris en compte
		t.fail();
		t.end();
	}, 1000);
});
```

### Test en série

Bien que la simultanéité soit géniale, il y a certaines choses qui ne peuvent pas être faites en même temps. Dans ces rares cas, vous pouvez appeler `test.serial` : cela forcera les tests à fonctionner en série avant ceux qui s'exécutent en simultanée.

```js
test.serial(t => {
	t.pass();
});
```

### Les tests avec only

Les tests avec `only` sont les seuls tests qui seront exécutés. Cela peut être utile pour exécuter seulement quelques tests lors du développement.

```js
test('ne sera pas exécuté', t => {
	t.fail();
})

test.only('sera exécuté', t => {
	t.pass();
});
```

### Les tests avec skip

Les tests avec `skip` apparaissent dans le résultat comme ignorés mais ne sont jamais exécutés.

```js
test.skip('ne sera pas exécuté', t => {
	t.fail();
});
```

### Les [hooks](https://fr.wikipedia.org/wiki/Hook_%28informatique%29) before & after

Lorsqu'une configuration et/ou une déconfiguration est nécessaire, vous pouvez utiliser `test.before()` et `test.after()`,
de la même manière que `test()`. Les fonctions de test `test.before()` et `test.after()` sont appelées avant/après tous les tests. Vous pouvez aussi utiliser `test.beforeEach()` et `test.afterEach()` si vous avez besoin de configurer/déconfigurer pour chaque test. Les hooks sont exécutés en série dans le fichier de test. Ajoutez-en autant que vous le voulez. Vous pouvez éventuellement spécifier un titre qui sera affiché en cas d'échec.


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

Vous pouvez utiliser les fonctions asynchrones, retourner des objets async ou activer le "mode callback" dans l'un des hooks.

```js
test.before(async t => {
	await promiseFn();
});

test.cb.beforeEach(t => {
	setTimeout(t.end);
});

test.afterEach.cb(t => {
	setTimeout(t.end);
});

test.after(t => {
	return new Promise(/* ... */);
});
```

Les hooks `beforeEach` & `afterEach` peuvent partager le contexte avec le test :

```js
test.beforeEach(t => {
	t.context.data = generateUniqueData();
});

test(t => {
	t.is(t.context.data + 'bar', 'foobar');
});
```

Le context est par défaut un objet, mais il peut être directement assigné :

```js
test.beforeEach(t => {
	t.context = 'unicorn';
});

test(t => {
	t.is(t.context, 'unicorn');
});
```

### Chaînage des modificateurs de test

Vous pouvez chaîner des modificateurs de test de la manière suivante :

```js
test.before.skip([title], testFn);
test.skip.after(....);
test.serial.only(...);
test.only.serial(...);
```

Ceci est particulièrement utile pour utiliser temporairement `skip` ou `only` sur un test, sans perdre les informations et le comportement que les autres modificateurs fournissent.

### Personnaliser le module d'assertion

Vous pouvez utiliser un module d'assertion qui remplace ou s'ajoute à celui qui est livré avec AVA, mais vous ne pourrez pas ([pour l'instant](https://github.com/sindresorhus/ava/issues/25)) utiliser la méthode `.plan()`.

```js
import assert from 'assert';

test(t => {
	assert(true);
});
```

### Prise en charge de ES2015

AVA est livré avec un support intégré pour ES2015 via [Babel 6](https://babeljs.io). Il suffit d'écrire vos tests en ES2015. Aucune configuration supplémentaire n'est nécessaire. Vous pouvez utiliser n'importe quelle version de Babel dans votre projet. Nous utilisons notre propre bundle Babel avec les presets [`es2015`](http://babeljs.io/docs/plugins/preset-es2015/) et [`stage-2`](http://babeljs.io/docs/plugins/preset-stage-2/).

#### Transpilation des modules importés

AVA transpiles actuellement seulement les tests que vous lui demandez d'exécuter. *Il ne transpilera pas les modules importés (```import```) depuis le fichier de test.* Même s'il existe des raisons valables à cette approche, cela peut ne pas être ce que vous attendez !

Comme solution de contournement simple, vous pouvez utiliser le [hook require de Babel](https://babeljs.io/docs/usage/require/) afin de faire la transpilation à la volée des modules qui sont importés par la suite. Comme AVA prend en charge la syntaxe du module ES2015, vous pouvez l'utiliser pour importer le hook require :

```js
import test from 'ava';
import 'babel-core/register';
import foo from './foo'; // <-- foo peut-être écrit en ES2015 !

test('foo bar', t => {
	t.same('baz', foo('bar'));
});
```

[#111](https://github.com/sindresorhus/ava/issues/111) est le suivi de ce point pour une éventuelle amélioration.

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

AVA est livré avec un support pour les [observables](https://github.com/zenparsing/es-observable).
Si vous retournez un observable dans un test, AVA le consommera automatiquement jusqu'à la fin avant de terminer le test.

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

```
$ ava --tap | tap-nyan
```

<img src="https://github.com/sindresorhus/ava/blob/master/media/tap-output.png" width="398">


### Nettoyage de la stack trace

AVA supprime automatiquement les lignes sans rapport dans la stack trace, cela permet de trouver la source d'une erreur plus rapidement.

<img src="https://github.com/sindresorhus/ava/blob/master/media/stack-traces.png" width="300">


## API

### test([title], body)
### test.serial([title], body)
### test.cb([title], body)
### test.only([title], body)
### test.skip([title], body)
### test.before([title], body)
### test.after([title], body)
### test.beforeEach([title], body)
### test.afterEach([title], body)

#### title

Type: `string`

Titre du test.

#### body(context)

Type: `function`

Doit contenir le test réel.

##### context

Passé dans la fonction de test et contient les différentes méthodes et [assertions](#assertions) d'AVA.

###### .plan(count)

Détermine le nombre d'assertion qu'il y a dans le test. Le test échouera si le nombre d'assertion réel ne correspond pas au nombre d'assertions prévues.

###### .end()

La fin du test. Fonctionne uniquement avec `test.cb()`.


## Assertions

Les assertions sont mélangées dans le [contexte](#context) du test :

```js
test(t => {
	t.ok('unicorn'); // assertion
});
```

Si plusieurs échecs d'assertion se produisent à l'intérieur d'un seul test, AVA affichera uniquement le *premier*.

### .pass([message])

L'assertion passe.

### .fail([message])

L'assertion échoue.

### .ok(value, [message])

Affirme que `value` est truthy.

### .notOk(value, [message])

Affirme que `value` est falsy.

### .true(value, [message])

Affirme que `value` est à `true`.

### .false(value, [message])

Affirme que `value` est à `false`.

### .is(value, expected, [message])

Affirme que `value` est égal à `expected`.

### .not(value, expected, [message])

Affirme que `value` n'est pas égal à `expected`.

### .same(value, expected, [message])

Affirme que `value` est deep equal à `expected`.

### .notSame(value, expected, [message])

Affirme que `value` n'est pas deep equal à `expected`.

### .throws(function|promise, [error, [message]])

Affirme que `function` lève `error` ou rejète `promise`.

`error` peut-être un constructeur, une regex, un message d'erreur ou une fonction de validation.

### .notThrows(function|promise, [message])

Affirme que `function` ne lève pas `error` ou résout `promise`.

### .regex(contents, regex, [message])

Affirme que `contents` correspond à `regex`.

### .ifError(error, [message])

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

## Asserts améliorées

AVA est livré avec l'intégration de [`power-assert`](https://github.com/power-assert-js/power-assert), cela vous donne des messages d'assertion plus clairs. Il lit votre test et tente de déduire plus d'informations à partir du code.

Le test suivant :

```js
test(t => {
	const x = 'foo';
	t.ok(x === 'bar');
});
```

Donnera normalement l'affichage inutile suivant :

```
false === true
```

Avec les assertions améliorées, vous obtiendrez :

```
t.ok(x === 'bar')
     |
     "foo"
```

Certes, vous pouvez et devriez probablement utiliser `t.is()` dans ce cas, mais cela est juste un exemple simple.

Essayez un exemple plus complexe :

```js
test(t => {
	const a = /foo/;
	const b = 'bar';
	const c = 'baz';
	t.ok(a.test(b) || b === c);
});
```

Et vous aurez :

```
t.ok(a.test(b) || b === c)
       |    |     |     |
       |    "bar" "bar" "baz"
       false
```

Toutes les méthodes d'assertions sont améliorées.

Amusez-vous !


## Environnement isolé

Chaque fichier de test est exécuté dans un processus Node.js séparé. Cela apporte beaucoup d'avantages. Les différents fichiers de test ne peuvent plus affecter les autres. Comme pour les fichiers de test maquettés avec l'environnement global, réécriture de fonction interne, etc. Cependant, cela a été principalement fait pour des raisons de performance. Même si Node.js peut exécuter des IO simultanément en asynchrone, cela ne vous aide pas beaucoup lorsque les tests sont lourds sur des opérations synchrones, qui bloque le thread principal. En exécutant des tests simultanément et des fichiers de test en parallèle, nous tirons pleinement parti des systèmes modernes.


## Astuces

### Les fichiers temporaires

L'exécution des tests en simultané apporte quelques défis, faire de l'IO est le premier. Habituellement, les tests en série créent juste des répertoires temporaires dans le répertoire de test en cours et les nettoient à la fin. Cela ne fonctionnera pas lorsque vous exécutez des tests en simultané, car les tests seront en conflit les uns avec les autres. La bonne façon de le faire, est d'utiliser un nouveau répertoire temporaire pour chaque test. Les modules [`tempfile`](https://github.com/sindresorhus/tempfile) et [`temp-write`](https://github.com/sindresorhus/temp-write) peuvent être utiles.

### Débogage

AVA exécute par défaut les tests en simultané, ce qui n'est pas optimal lorsque vous avez besoin de déboguer quelque chose. Pour cela, exécuter les tests en série avec l'option `--serial` :

```
$ ava --serial
```

### Couverture de code

Vous ne pouvez pas utiliser [`istanbul`](https://github.com/gotwarlost/istanbul) pour la couverture de code car AVA [génère les fichiers de test](#environnement-isolé), mais vous pouvez utiliser [`nyc`](https://github.com/bcoe/nyc) à la place, c'est essentiellement `istanbul` avec en plus la prise en charge des sous-processus (subprocesses).

Depuis la version `5.0.0`, il utilise les source maps pour faire le rapport de votre couverture de code, indépendamment de la transpilation. Assurez-vous, que le code que vous testez, comprend une source map ou les références d'un fichier source map. Si vous utilisez `babel/register` vous pouvez définir l'option `sourceMaps` dans votre `.babelrc` à `inline`.


## FAQ

### Pourquoi ne pas utiliser `mocha`, `tape` ou `node-tap` ?

Mocha vous oblige à utiliser les globales implicites comme `describe` et `it` avec l'interface par défaut (que la plupart des gens utilisent), il est trop flexible, gourmand, synchrone par défaut, une API de programmation inutilisable, l'exécution des tests se fait en série et il est lent. Tape et node-tap sont bons. AVA est fortement inspiré par leur syntaxe. Cependant, tous les deux exécutent les tests en série et ils ont rendu [TAP](https://testanything.org) comme un élément indispensable, ce qui a mon avis, rend leurs codes de base un peu compliqué et sans possibilité de s'en séparer. La restitution de TAP est difficile à lire donc vous avez toujours besoin de l'aide d'un "reporter". AVA est très opiniâtre et s'exécute en simultané. Il est livré avec un "reporter" simple par défaut et il supporte TAP grâce à une option du CLI.

### Comment puis-je utiliser des "reporters" personnalisés ?

Utilisez l'[option `--tap`](#sortie-facultative-au-format-tap) avec un [reporter TAP](https://github.com/sindresorhus/awesome-tap#reporters).

### Comment l'écrire et le prononcer ?

AVA, pas Ava ni ava. Prononcez [`/ˈeɪvə/` ay-və](https://github.com/sindresorhus/ava/blob/master/media/pronunciation.m4a?raw=true).

### Que représente l'arrière plan de l'image ?

La [galaxie d'Andromède.](https://simple.wikipedia.org/wiki/Andromeda_galaxy)

### Simultanéité et parallélisme

[La simultanéité n'est pas du parallélisme. Il permet le parallélisme.](http://stackoverflow.com/q/1050222).


## Recettes

- [Couverture de code](docs/recipes/code-coverage.md)
- [Tester un endpoint](docs/recipes/endpoint-testing.md)
- [Quand utiliser `t.plan()` ?](docs/recipes/when-to-use-plan.md)


## Support

- [Stack Overflow](https://stackoverflow.com/questions/tagged/ava)
- [Gitter chat](https://gitter.im/sindresorhus/ava)
- [Twitter](https://twitter.com/ava__js)


## Autre

- [Stickers du logo AVA](https://www.stickermule.com/user/1070705604/stickers)


## En relation

- [gulp-ava](https://github.com/sindresorhus/gulp-ava) - Exécutez les tests avec gulp
- [grunt-ava](https://github.com/sindresorhus/grunt-ava) - Exécutez les tests avec grunt
- [fly-ava](https://github.com/pine613/fly-ava) - Exécutez les tests avec fly
- [start-ava](https://github.com/start-runner/ava) - Exécutez les tests avec start


## L'équipe

[![Sindre Sorhus](https://avatars.githubusercontent.com/u/170270?s=130)](http://sindresorhus.com) | [![Vadim Demedes](https://avatars.githubusercontent.com/u/697676?s=130)](https://github.com/vdemedes) | [![James Talmage](https://avatars.githubusercontent.com/u/4082216?s=130)](https://github.com/jamestalmage)
---|---|---|---
[Sindre Sorhus](http://sindresorhus.com) | [Vadim Demedes](https://github.com/vdemedes) | [James Talmage](https://github.com/jamestalmage)

#### Auparavant dans l'équipe

- [Kevin Mårtensson](https://github.com/kevva)



<div align="center">
	<br>
	<br>
	<br>
	<img src="https://cdn.rawgit.com/sindresorhus/ava/fe1cea1ca3d2c8518c0cc39ec8be592beab90558/media/logo.svg" width="200" alt="AVA">
	<br>
	<br>
</div>
