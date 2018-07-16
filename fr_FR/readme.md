___
**Note du traducteur**

C'est la traduction du fichier [readme.md](https://github.com/avajs/ava/blob/master/readme.md). Voici un [lien](https://github.com/avajs/ava/compare/d5dd981d0283303ad6cee62b14a59318d2316c85...master#diff-0730bb7c2e8f9ea2438b52e419dd86c9) vers les différences avec le master de AVA (Si en cliquant sur le lien, vous ne trouvez pas le fichier `readme.md` parmi les fichiers modifiés, vous pouvez donc en déduire que la traduction est à jour).
___
# [![AVA](https://github.com/avajs/ava/blob/master/media/header.png)](https://ava.li)

> Lanceur de test futuriste

[![Build Status: Linux](https://travis-ci.org/avajs/ava.svg?branch=master)](https://travis-ci.org/avajs/ava) [![Build status: Windows](https://ci.appveyor.com/api/projects/status/e7v91mu2m5x48ehx/branch/master?svg=true)](https://ci.appveyor.com/project/ava/ava/branch/master) [![Coverage Status](https://coveralls.io/repos/github/avajs/ava/badge.svg?branch=master)](https://coveralls.io/github/avajs/ava?branch=master) [![XO code style](https://img.shields.io/badge/code_style-XO-5ed9c7.svg)](https://github.com/xojs/xo) [![Join the community on Spectrum](https://withspectrum.github.io/badge/badge.svg)](https://spectrum.chat/ava)
[![Mentioned in Awesome Node.js](https://awesome.re/mentioned-badge.svg)](https://github.com/sindresorhus/awesome-nodejs)

Même si JavaScript est mono-thread, l'IO dans Node.js peut se lancer en parallèle en raison de sa nature asynchrone. AVA profite de cela et exécute vos tests en même temps, ce qui est particulièrement avantageux pour les tests lourds d'IO. De plus, les fichiers de test sont exécutés en parallèle comme des processus séparés, cela vous donne encore de meilleures performances et un environnement isolé pour chaque fichier de test. Le [passage](https://github.com/sindresorhus/pageres/commit/663be15acb3dd2eb0f71b1956ef28c2cd3fdeed0) de Mocha à AVA dans Pageres a diminué la durée des tests de 31 à 11 secondes. Comme les tests sont exécutés simultanément, cela vous oblige à écrire des tests [atomiques](https://fr.wikipedia.org/wiki/Atomicit%C3%A9_%28informatique%29), ce qui signifie que les tests ne dépendent pas de l'état global ou de l'état des autres tests, ce qui est une bonne chose !

![](https://github.com/avajs/ava/blob/master/media/mini-reporter.gif)

*Lisez notre [guide de contribution](contributing.md) si vous souhaitez contribuer (issues/PRs/etc).*

Suivez le [compte Twitter de AVA](https://twitter.com/ava__js) pour les mises à jour.

**Cette documentation couvre les versions bêta 1.0, qui utilisent Babel 7. La dernière version qui utilise Babel 6 est la [`v0.25.0`](https://github.com/avajs/ava/tree/v0.25.0).**

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
- [Écriture de vos tests en utilisant la dernière syntaxe de JavaScript](#prise-en-charge-de-la-dernière-syntaxe-de-javascript)
- [Prise en charge des promesses](#prise-en-charge-des-promesses)
- [Prise en charge des fonctions asynchrones](#prise-en-charge-des-fonctions-asynchrones)
- [Prise en charge d'Observable](#prise-en-charge-de-observable)
- [Messages d'assertions améliorés](#messages-dassertions-améliorés)
- [Reporter de TAP](#reporter-de-tap)
- [Migration automatique depuis un autre exécuteur de test](https://github.com/avajs/ava-docs/blob/master/fr_FR/related/ava-codemods/readme.md#migration-vers-ava)


## Syntaxe d'un Test

```js
import test from 'ava';

test('les tableaux sont égaux', t => {
	t.deepEqual([1, 2], [1, 2]);
});
```

## Utilisation

### Ajoutez AVA à votre projet

Pour installer et configurer AVA, exécutez :

```console
$ npx create-ava --next
```

Votre `package.json` ressemblera alors à ceci :

```json
{
	"name": "awesome-package",
	"scripts": {
		"test": "ava"
	},
	"devDependencies": {
		"ava": "1.0.0-beta.4"
	}
}
```

L'initialisation fonctionne avec npm et Yarn, mais en exécutant `npx` qui a besoin de [`npm@5.2.0`](https://github.com/npm/npm/releases/tag/v5.2.0) ou plus pour l'installer. Sinon, vous devez installer manuellement `ava` et configurer le script `test` dans votre `package.json` comme ci-dessus :

```console
$ npm install --save-dev --save-exact ava@next
```

Ou si vous préférez utiliser Yarn :

```console
$ yarn add ava@next --dev --exact
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

### Exécutez le test

```console
$ npm test
```

### Scrutez le test

```console
$ npm test -- --watch
```

AVA est livré avec un mode watch intelligent. [Apprenez en plus avec cette recette](docs/recipes/watch-mode.md).

### Version Node.js supportée

AVA prend en charge la dernière version de toute version majeure [prise en charge par Node.js](https://github.com/nodejs/Release#release-schedule). Lisez la suite dans notre [déclaration de support](docs/support-statement.md).

## CLI

```console
$ ava --help

  Usage
    ava [<file|directory|glob> ...]

  Options
    --watch, -w             Re-run tests when tests and source files change (Re-exécute les tests quand les tests et les fichiers sources ont changé)
    --match, -m             Only run tests with matching title (Can be repeated) (Exécute seulement les tests qui correspondent au titre (peut être répété))
    --update-snapshots, -u  Update all snapshots (Mettre à jour les snapshots)
    --fail-fast             Stop after first test failure (Arrêter dès qu'un test échoue)
    --timeout, -T           Set global timeout (Définir un timeout global)
    --serial, -s            Run tests serially (Lancer les tests en série)
    --concurrency, -c       Max number of test files running at the same time (Default: CPU cores) (Nombre max de fichiers de test exécutés en même temps (Par défaut : coeurs CPU))
    --verbose, -v           Enable verbose output (Activer le mode verbose)
    --tap, -t               Generate TAP output (Générer une sortie au format TAP)
    --color                 Force color output (Force l'affichage avec des couleurs)
    --no-color              Disable color output (Désactiver l'affichage avec des couleurs)

  Examples (Exemples)
    ava
    ava test.js test2.js
    ava test-*.js
    ava test

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

Toutes les options du CLI peuvent être configurés dans la section `ava` de votre `package.json` ou dans un fichier `ava.config.js`. Cela vous permet de modifier le comportement par défaut de la commande `ava`, ainsi vous n'avez plus besoin à chaque fois de taper les mêmes options sur l'invite de commande.

Pour ignorer un fichier ou un répertoire, préfixer le chemin avec un `!` (point d'exclamation).

```json
{
	"ava": {
		"files": [
			"mon-repertoire-test/**/*.js",
			"!mon-repertoire-test/exclus-ce-repertoire/**/*.js",
			"!**/exclus-ce-fichier.js"
		],
		"sources": [
			"**/*.{js,jsx}",
			"!dist/**/*"
		],
		"match": [
			"*oo",
			"!foo"
		],
		"cache": true,
		"concurrency": 5,
		"failFast": true,
		"failWithoutAssertions": false,
		"tap": true,
		"compileEnhancements": false,
		"require": [
			"@babel/register"
		],
		"babel": {
			"extensions": ["jsx"],
			"testOptions": {
				"babelrc": false
			}
		}
	}
}
```

Les arguments passés au CLI seront toujours prioritaires sur ceux de la configuration dans `package.json`.

### Options

- `files` : les chemins des fichiers et des répertoires, ainsi que les modèles de glob (glob patterns) qui sélectionnent les fichiers AVA qui feront des tests. Les fichiers préfixés avec un underscore sont ignorés. Tous les fichiers correspondants dans les répertoires sélectionnés sont exécutés. Par défaut, cela sélectionne uniquement les fichiers avec les extensions `js`, même si le modèle de glob correspond à d'autres fichiers. Spécifier `extensions` et `babel.extensions` pour autoriser d'autres extensions de fichiers
- `source` : les fichiers, lorsqu'ils sont modifiés, provoquent la ré-exécution des tests lors du mode watch. Voir la [recette du mode watch pour plus de détails](docs/recipes/watch-mode.md#les-fichiers-sources-et-les-fichiers-de-test)
- `match` : n'est généralement pas utile dans la configuration du `package.json`, mais est équivalent au [`--match` de la CLI](#exécution-de-tests-correspondants-à-des-titres)
- `cache` : met en cache les fichiers de test compilé et les helpers sous `node_modules/.cache/ava`. Si la valeur est à `false`, les fichiers sont à la place mis en cache dans un répertoire temporaire
- `failFast` : arrête d'exécuter d'autres tests dès qu'un test échoue
- `failWithoutAssertions` : si `false`, ne pas faire échouer un test s'il n'exécute pas des [assertions](#assertions)
- `tap` : si `true`, active le [reporter de TAP](#reporter-de-tap)
- `snapshotDir` : indique l'endroit fixe pour le stockage des fichiers instantanés. Utilisez ceci si vos instantanés se positionnent à un mauvais endroit
- `compileEnhancements` : si `false`, désactive [power-assert](https://github.com/power-assert-js/power-assert) qui aide tout de même à fournir des messages d'erreur plus descriptifs — et la détection d'une mauvaise utilisation de l'assertion `t.throws()`
- `extensions` : les extensions de fichiers de test qui seront précompilées à l'aide des presets Babel de AVA. Notez que les fichiers sont toujours compilés pour activer power-assert et d'autres fonctionnalités, donc vous devrez peut-être aussi définir `compileEnhancements` à `false` si vos fichiers ne sont pas valides JavaScript. La définition de ce paramètre remplace la valeur par défaut `"js"`, alors assurez-vous d'inclure cette extension dans la liste, si celle-ci n'est pas incluse dans `babel.extensions`
- `require` : modules supplémentaires à intégrer avant que les tests ne soient exécutés. Les modules qui sont requis dans le [processus de travail](#isolement-du-processus)
- `babel` : options spécifiques de Babel pour les fichiers de test. Consultez notre [recette Babel] pour plus de détails
- `babel.extensions` : les extensions de fichiers de test qui seront précompilées à l'aide des presets Babel de AVA. Ce réglage remplace la valeur par défaut `"js"`, alors assurez-vous d'inclure cette extension dans la liste

Veuillez notez qu'en fournissant des fichiers à la CLI, cela écrase l'option `files`. Si vous avez configuré un glob pattern, par exemple `test/**/*.test.js`, vous devez peut-être le répéter lors de l'utilisation de la CLI : `ava 'test/integration/*.test.js'`.

### Utilisation de `ava.config.js`

Pour utiliser un fichier `ava.config.js` :

 1. Il doit être dans le même répertoire que votre `package.json`
 2. Votre `package.json` ne doit pas contenir la propriété `ava` (ou si c'est le cas, il doit s'agir d'un objet vide)

Le fichier config doit avoir un export default, en utilisant des modules ES. Il peut s'agir d'un objet simple ou d'une fonction factory qui renvoie un objet brut :

```js
export default {
	require: ['esm']
};
```

```js
export default function factory() {
	return {
		require: ['esm']
	};
};
```

La fonction factory est appelée avec un objet contenant une propriété `projectDir`, vous pourrez l'utiliser pour changer la configuration retournée :

```js
export default ({projectDir}) => {
	if (projectDir === '/Users/username/projects/my-project') {
		return {
			// Config A
		};
	}

	return {
		// Config B
	};
};
```

Notez que la config finale ne doit pas être une promesse.

## Documentation

Les tests sont exécutés simultanément. Vous pouvez spécifier des tests synchrones et asynchrones. Les tests sont considérés synchrones sauf si vous retournez une promesse ou un [observable](https://github.com/zenparsing/zen-observable).

Nous recommandons *fortement* l'utilisation de [fonctions asynchrones](#prise-en-charge-des-fonctions-asynchrones). Elles rendent le code asynchrone concis et lisible, et elles retournent implicitement une promesse, donc vous n'avez pas à le faire.

Si vous n'avez pas la possibilité d'utiliser des promesses ou des observables, vous pouvez activer le "mode callback" en définissant votre test avec `test.cb([title], fn)`. Les tests déclarés de cette manière **doivent** être terminés manuellement avec `t.end()`. Ce mode est principalement destiné pour tester les APIs de style callback. Cependant, nous vous recommandons vivement d'utiliser les API [promisifying](https://github.com/sindresorhus/pify) de style callback au lieu d'utiliser le "mode callback", car cela donne des tests plus corrects et plus lisibles.

Vous devez définir tous les tests de manière synchrone. Ils ne peuvent pas être définis à l'intérieur de `setTimeout`, `setImmediate`, etc.

AVA essaie d'exécuter les fichiers de test dans leur répertoire de travail courant, c'est à dire le répertoire qui contient votre fichier `package.json`.

### Création des tests

Pour créer un test, vous appelez la fonction `test` que vous importez de AVA. Fournissez un titre obligatoire et une fonction d'implémentation. Les titres doivent être uniques dans chaque fichier de test. La fonction sera appelée lorsque votre test sera exécutée. Un [objet d'exécution](#t) est passé comme premier argument.

**Remarque :** Pour que les [messages d'assertion améliorés](#messages-dassertions-améliorés) se comportent correctement, le premier argument **doit** être nommé `t`.

```js
import test from 'ava';

test('mon test qui passe', t => {
	t.pass();
});
```

### Assertion planifiée

Une assertion **plan**-ifiée assure que les tests passent uniquement quand un certain nombre d'assertions ont été exécutées. Elle vous aidera à traiter les cas où les tests sortent trop tôt. Elle mettra aussi les tests en échec si trop d'assertions sont exécutées, ce qui peut être utile si vous avez des assertions à l'intérieur des callbacks ou des boucles.

Si vous ne spécifiez pas une assertion planifiée, votre test échouera toujours si aucune assertion n'est exécutée. Définissez l'option `failWithoutAssertions` à `false` dans la [configuration de AVA dans le `package.json`](#configuration) pour désactiver ce comportement.

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

### Exécution des tests en série

Par défaut les tests sont exécutés simultanément, ce qui est génial. Parfois, vous devez écrire des tests qui ne peuvent pas fonctionner en même temps. Dans ces rares cas, vous pouvez utiliser `.serial`. Cela forcera les tests à s'exécuter en série *avant* ceux qui s'exécutent en simultané.

```js
test.serial('passe en série', t => {
	t.pass();
});
```

Notez que cela ne s'applique qu'aux tests dans un fichier de test. AVA exécutera toujours plusieurs fichiers de tests en même temps, sauf si vous passez l'[option `--serial` au CLI](#cli).

Vous pouvez utiliser le modificateur `.serial` avec tous les tests, les hooks et même avec `.todo()`, mais il n'est disponible que sur la fonction `test`.

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

Vous pouvez utiliser le modificateur `.only` avec tous les tests. Il ne peut pas être utilisé avec des hooks ou `.todo ()`.

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

Vous devez spécifier la fonction d'implémentation. Vous pouvez utiliser le modificateur `.skip` avec tous les tests et les hooks, mais pas avec `.todo()`. Vous ne pouvez pas appliquer d'autres modificateurs à `.skip`.

### Les tests fictifs ("todo")

Vous pouvez utiliser le modificateur `.todo` lorsque vous avez l'intention d'écrire un test. Comme les tests passés (`.skip`), ces tests fictifs sont présentés dans le résultat. Ils exigent seulement un titre : vous ne pouvez pas spécifier la fonction d'implémentation.

```js
test.todo('il faudra penser à écrire cela plus tard');
```

Vous pouvez signaler que vous devez écrire un test en série :

```js
test.serial.todo('penser à l''écrire plus tard');
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

`test.before()` enregistre un hook qui sera exécuté avant le premier test dans votre fichier de test. De même, `test.after()` enregistre un hook qui sera exécuté après le dernier test. Utilisez `test.after.always()` pour enregistrer un hook qui s'exécutera **toujours**, une fois que vos tests et autres hooks sont terminés. Les hooks `.always()` s'exécutent indépendamment des erreurs précédentes, ils sont donc idéals pour des tâches de nettoyage. Notez cependant que les exceptions non interceptées, les rejets non traités et les timeouts planteront vos tests, empêchant éventuellement les hooks `.always ()` de s'exécuter.

`test.beforeEach()` enregistre un hook qui sera exécuté avant chaque test dans votre fichier de test. De même, `test.afterEach()` enregistre un hook qui sera exécuté après chaque test. Utilisez `test.afterEach.always()` pour enregistrer un hook qui est appelé après, même si un autre hook de test ou le test lui-même échouent.

Si un test est ignoré avec le modificateur `.skip`, les hooks respectifs `.beforeEach()`, `.afterEach()` et `.afterEach.always()` ne sont pas exécutés. De même, si tous les tests dans un fichier de test sont ignorés, les hooks `.before()`, `.after()` et `.after.always()` pour le fichier ne sont pas exécutés.

Comme `test()`, ces méthodes prennent comme arguments un titre facultatif et une implémentation d'une fonction. Le titre est affiché si votre hook ne parvient pas à s'exécuter. L'implémentation est appelé avec un [objet d'exécution](#t). Vous pouvez utiliser des assertions dans vos hooks. Vous pouvez également passer une [fonction macro](#test-macros) et des arguments supplémentaires.

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

#### Tester le contexte

Les hooks peuvent partager le contexte avec le test :

```js
test.beforeEach(t => {
	t.context.data = generateUniqueData();
});

test('les données du contexte sont foo', t => {
	t.is(t.context.data + 'bar', 'foobar');
});
```

Le contexte créé dans les hooks `.before()` est [cloné](https://www.npmjs.com/package/lodash.clone) avant d'être passé aux hooks `.beforeEach()` et / ou aux tests. Les hooks `.after()` et `.after.always()` recoive la valeur originale du contexte.

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

test('assertion personnalisée', t => {
	assert(true);
});
```

### Prise en charge de la dernière syntaxe de Javascript

AVA utilise [Babel 7](https://babeljs.io) donc vous pouvez utiliser la dernière syntaxe JavaScript dans vos tests. Il n'y a pas d'installation supplémentaire requise. Vous n'avez pas non plus besoin d'utiliser Babel dans votre propre projet pour que cela fonctionne.

Nous visons la prise en charge de toutes les [propositions de syntaxe terminées](https://github.com/tc39/proposals/blob/master/finished-proposals.md), ainsi que toute la syntaxe de la version JavaScript ratifiée (par exemple ES2017). Consultez notre preset [`@ava/stage-4`](https://github.com/avajs/babel-preset-stage-4) pour les propositions actuellement supportées.

Veuillez noter que nous n'ajoutons pas ou ne modifions pas les built-ins. Par exemple, si vous utilisez [`Object.entries()`](https://github.com/tc39/proposal-object-values-entries) dans vos tests, ils planteront dans Node.js 6 car il n'implémente pas cette méthode.

Vous pouvez désactiver cette prise en charge de la syntaxe ou personnaliser le pipeline Babel de AVA. Consultez notre [recette Babel] pour plus de détails.

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
test('se résout avec unicorn', t => {
	return somePromise().then(result => {
		t.is(result, 'unicorn');
	});
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
test('les promesses, les vraies', async t => {
	const value = await promiseFn();
	t.true(value);
});
```

### Prise en charge de Observable

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

### Prise en charge du Callback

AVA prend en charge l'utilisation de `t.end` comme callback final lors de l'utilisation de callback d'API avec le style node d'["erreur-en-premier"](http://thenodeway.io/posts/understanding-error-first-callbacks/). AVA considérera toute valeur truthy passée comme premier argument à `t.end` comme une erreur. Notez que `t.end` exige le "mode callback", qui peut être activé en utilisant le chaînage `test.cb`.

```js
test.cb('data.txt peut être lu', t => {
	// `t.end` vérifie automatiquement le premier argument pour l'erreur
	fs.readFile('data.txt', t.end);
});
```

### Délai (timeout) global

Un délai (timeout) global peut être défini via l'option `--timeout`. Le délai de AVA se comporte différemment des autres frameworks de test. AVA réinitialise un minuteur après chaque test, cela oblige les tests à s'arrêter, si aucun nouveau résultat de test est reçu dans le délai imparti. Cela peut être utilisé pour gérer les tests bloqués.

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

L'objet d'exécution d'un test particulier. Chaque implémentation de test reçoit un objet différent. Il contient les [assertions](#assertions) ainsi que les méthodes `.plan(count)` et `.end()`. `t.context` peut contenir un état partagé depuis les hooks. `t.title` retourne le titre du test.

###### `t.plan(count)`

Détermine le nombre d'assertion qu'il y a dans le test. Le test échouera si le nombre d'assertion réel ne correspond pas au nombre d'assertion prévu. Consultez l'[assertion planifiée](#assertion-planifiée).

###### `t.end()`

La fin du test. Fonctionne uniquement avec `test.cb()`.

###### `t.log(...values)`

Publie les valeurs contextuelles à côté du résultat du test au lieu de les imprimer immédiatement dans `stdout`. Le comportement est un peu comme `console.log`, mais sans le support des jetons d'espace réservé.

## Assertions

Les assertions sont incluses dans l'[objet d'exécution](#t) fourni à chaque implémentation de test :

```js
test('unicorns sont truthy', t => {
	t.truthy('unicorn'); // Assertion
});
```

Les assertions sont liées à leur test de sorte que vous puissiez les affecter à une variable ou les transmettre :

```js
test('unicorns are truthy', t => {
	const truthy = t.truthy;
	truthy('unicorn');
});
```

Les assertions peuvent être ignorées en ajoutant `.skip()` :

```js
test('unicorns are truthy', t => {
	t.truthy.skip('unicorn');
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

### `.throws(thrower, [expected, [message]])`

Affirme qu'une erreur est levée. `thrower` peut être une fonction qui devrait lever une erreur ou retourner une promesse qui devrait être rejetée. Alternativement une promesse peut être passée directement.

La valeur levée *doit* être une erreur. Elle est retournée afin que vous puissiez lancer d'autres assertions.

`expected` peut être un constructeur, auquel cas l'erreur levée doit être une instance du constructeur. Cela peut être une chaîne, qui est comparée au message de l'erreur levée, ou une expression régulière qui correspond à ce message. Vous pouvez également spécifier un objet de correspondance (matcher) avec une ou plusieurs des propriétés suivantes :

* `instanceOf` : un constructeur, l'erreur levée doit être une "instance de"
* `is` : l'erreur levée doit être strictement égale à `expected.is`
* `message` : soit une chaîne qui est comparée au message de l'erreur levée, ou une expression régulière qui correspond à ce message
* `name` : la valeur `.name` attendue de l'erreur levée
* `code` : la valeur `.code` attendue de l'erreur levée

`expected` n'a pas besoin d'être précisé. Si vous n'en avez pas besoin mais que vous voulez définir un message d'assertion, vous devez spécifier `null`.

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

Lors du test d'une fonction asynchrone, vous devez également attendre que l'assertion soit terminée :

```js
test('throws', async t => {
	await t.throws(async () => {
		throw new TypeError('🦄');
	}, {instanceOf: TypeError, message: '🦄'});
});
```

### `.notThrows(nonThrower, [message])`

Affirme qu'aucune erreur est levée. `thrower` peut être une fonction qui ne devrait pas lever une erreur ou retourner une promesse qui devrait être résolue. Alternativement une promesse peut être passée directement.

Comme l'assertion `.throws()`, lorsque vous testez une promesse, vous devez attendre l'assertion pour terminer :

```js
test('resolves', async t => {
	await t.notThrows(promise);
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

Vous pouvez ensuite vérifier votre code. Si le changement était intentionnel, vous pouvez utiliser l'option `--update-snapshots` (ou `-u`) pour mettre à jour vos instantanés :

```console
$ ava --update-snapshots
```

Vous pouvez définir un emplacement fixe pour stocker les fichiers d'instantanés dans la configuration d'AVA [inclus dans le `package.json`](#configuration) :

```json
{
	"ava": {
		"snapshotDir": "custom-directory"
	}
}
```

Les fichiers instantanés seront enregistrés dans une arborescence qui reflète celle de vos fichiers de test.

Si vous utilisez AVA avec des fichiers de test précompilés, AVA essayera d'utiliser les source maps pour déterminer l'emplacement des fichiers d'origine. Les instantanés seront stockés à côté de ces fichiers, en suivant les mêmes règles que si AVA avait exécuté les fichiers originaux directement. C'est bien si vous écrivez vos tests en TypeScript (consultez notre [recette TypeScript](docs/recipes/typescript.md)).

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
test('assertions améliorées', t => {
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

AVA définira `process.env.NODE_ENV` à `test`, à moins que la variable d'environnement `NODE_ENV` ait été définie. Ceci est utile si le code que vous testez a des valeurs par défaut de test (par exemple lors de la sélection de la base de données à laquelle se connecter ou des options Babel spécifiques à l'environnement). Toutefois, il se peut que votre code ou ses dépendances se comportent différemment. Notez que `'NODE_ENV' dans process.env` sera toujours `true`.

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

AVA, pas Ava ni ava. Prononcez [/ˈeɪvə/ ay-və](https://github.com/avajs/ava/blob/master/media/pronunciation.m4a?raw=true).

### Que représente l'arrière plan de l'image ?

C'est la [galaxie d'Andromède.](https://simple.wikipedia.org/wiki/Andromeda_galaxy).

### Quelle est la différence entre simultanéité et parallélisme

[La simultanéité n'est pas du parallélisme. Il permet le parallélisme.](http://stackoverflow.com/q/1050222).

## Recettes

- [Configuration de test](docs/recipes/test-setup.md)
- [Couverture de code](docs/recipes/code-coverage.md)
- [Mode watch](docs/recipes/watch-mode.md)
- [Tester un endpoint](docs/recipes/endpoint-testing.md)
- [Quand utiliser `t.plan()` ?](docs/recipes/when-to-use-plan.md)
- [Tests de navigateur](docs/recipes/browser-testing.md)
- [TypeScript](docs/recipes/typescript.md)
- [Flow](docs/recipes/flow.md)
- [Configuration de Babel][recette Babel]
- [Utilisation des modules ES](docs/recipes/es-modules.md)
- [Passer des arguments à vos fichiers de test](docs/recipes/passing-arguments-to-your-test-files.md)
- [Tester les composants React](docs/recipes/react.md)
- [Tester les composants Vue.js](docs/recipes/vue.md)
- [JSPM et SystemJS](docs/recipes/jspm-systemjs.md)
- [Débogage des tests avec Chrome DevTools](docs/recipes/debugging-with-chrome-devtools.md)
- [Débogage des tests avec WebStorm](docs/recipes/debugging-with-webstorm.md)
- [Précompilation des fichiers sources avec webpack](docs/recipes/precompiling-with-webpack.md)
- [Tests d'intégration isolés de MongoDB](docs/recipes/isolated-mongodb-integration-tests.md)

## Support

- [Stack Overflow](https://stackoverflow.com/questions/tagged/ava)
- [Spectrum](https://spectrum.chat/ava)
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

- [Stickers AVA, t-shirts, etc](https://www.redbubble.com/people/sindresorhus/works/30330590-ava-logo)
- [La liste Awesome](https://github.com/avajs/awesome-ava)
- [AVA Casts](http://avacasts.com)
- [Plus…](https://github.com/avajs/awesome-ava)

## L'équipe

[![Mark Wubben](https://github.com/novemberborn.png?size=100)](https://github.com/novemberborn) | [![Sindre Sorhus](https://github.com/sindresorhus.png?size=100)](https://github.com/sindresorhus) | [![Vadim Demedes](https://github.com/vadimdemedes.png?size=100)](https://github.com/vadimdemedes)
---|---|---
[Mark Wubben](https://novemberborn.net) | [Sindre Sorhus](http://sindresorhus.com) | [Vadim Demedes](https://github.com/vadimdemedes)

###### Auparavant dans l'équipe

- [Kevin Mårtensson](https://github.com/kevva)
- [James Talmage](https://github.com/jamestalmage)
- [Juan Soto](https://github.com/sotojuan)
- [Jeroen Engels](https://github.com/jfmengels)


<div align="center">
	<br>
	<br>
	<br>
	<a href="https://ava.li">
		<img src="https://github.com/avajs/ava/blob/master/media/logo.svg" width="200" alt="AVA">
	</a>
	<br>
	<br>
</div>

[recette Babel]: docs/recipes/babel.md
