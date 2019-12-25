___
**Note du traducteur**

C'est la traduction du fichier [readme.md](https://github.com/avajs/ava/blob/master/readme.md). Voici un [lien](https://github.com/avajs/ava/compare/b4ea43529a6d058a96055735cfa6e7056c009112...master#diff-0730bb7c2e8f9ea2438b52e419dd86c9) vers les différences avec le master de AVA (Si en cliquant sur le lien, vous ne trouvez pas le fichier `readme.md` parmi les fichiers modifiés, vous pouvez donc en déduire que la traduction est à jour).
___
# <img src="https://github.com/avajs/ava/blob/master/media/header.png" title="AVA" alt="AVA logo" width="530">

[![Build Status](https://travis-ci.org/avajs/ava.svg?branch=master)](https://travis-ci.org/avajs/ava)  [![Coverage Status](https://codecov.io/gh/avajs/ava/branch/master/graph/badge.svg)](https://codecov.io/gh/avajs/ava/branch/master) [![XO code style](https://img.shields.io/badge/code_style-XO-5ed9c7.svg)](https://github.com/xojs/xo) [![Join the community on Spectrum](https://withspectrum.github.io/badge/badge.svg)](https://spectrum.chat/ava)
[![Mentioned in Awesome Node.js](https://awesome.re/mentioned-badge.svg)](https://github.com/sindresorhus/awesome-nodejs)

Les tests peuvent être un frein. AVA vous aide à le faire. AVA est un lanceur de tests pour Node.js avec une API concise, une sortie d'erreur détaillée, de nouvelles fonctionnalités de langage et une isolation des processus qui vous permet d'écrire des tests plus efficacement. Donc vous pouvez embarquer du code plus impressionnant. 🚀

Suivez le [compte Twitter de AVA](https://twitter.com/ava__js) pour les mises à jour.

Lisez notre [guide de contribution](contributing.md) si vous souhaitez contribuer (issues / PRs / etc).

![](https://github.com/avajs/ava/blob/master/media/mini-reporter.gif)


Traductions : [Español](https://github.com/avajs/ava-docs/blob/master/es_ES/readme.md), [Français](https://github.com/avajs/ava-docs/blob/master/fr_FR/readme.md), [Italiano](https://github.com/avajs/ava-docs/blob/master/it_IT/readme.md), [日本語](https://github.com/avajs/ava-docs/blob/master/ja_JP/readme.md), [한국어](https://github.com/avajs/ava-docs/blob/master/ko_KR/readme.md), [Português](https://github.com/avajs/ava-docs/blob/master/pt_BR/readme.md), [Русский](https://github.com/avajs/ava-docs/blob/master/ru_RU/readme.md), [简体中文](https://github.com/avajs/ava-docs/blob/master/zh_CN/readme.md)


## Pourquoi AVA ?

- Minimal et rapide
- Syntaxe de test simple
- Exécution des tests simultanément
- Application de l'écriture des tests atomiques
- Aucunes globales implicites
- Inclus les définitions des types pour TypeScript
- [Magic assert](#magic-assert)
- [Environnement isolé pour chaque fichier de test](./docs/01-writing-tests.md#isolement-du-processus)
- [Écriture de vos tests en utilisant la dernière syntaxe de JavaScript](https://github.com/avajs/babel)
- [Prise en charge des promesses](./docs/01-writing-tests.md#prise-en-charge-des-promesses)
- [Prise en charge des fonctions asynchrones](./docs/01-writing-tests.md#prise-en-charge-des-fonctions-asynchrones)
- [Prise en charge d'Observable](./docs/01-writing-tests.md#prise-en-charge-de-observable)
- [Messages d'assertions améliorés](./docs/03-assertions.md#messages-dassertions-améliorés)
- [Test automatiquement en parallèle dans CI](#exécution-en-parallèle-dans-ci)
- [Reporter de TAP](./docs/05-command-line.md#reporter-de-tap)


## Utilisation

Pour installer et configurer AVA, exécutez :

```console
npm init ava
```

Votre `package.json` ressemblera alors à ceci (version exacte) :

```json
{
	"name": "awesome-package",
	"scripts": {
		"test": "ava"
	},
	"devDependencies": {
		"ava": "^1.0.0"
	}
}
```

Ou si vous préférez utiliser Yarn :

```console
yarn add ava --dev
```

Sinon, vous pouvez installer manuellement `ava` :

```console
npm install --save-dev ava
```

N'oubliez pas de configurer le script `test` dans votre `package.json` comme indiqué ci-dessus.

### Créez votre fichier de test

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

### Exécution de vos tests

```console
npm test
```

Ou avec `npx` :

```console
npx ava
```

Exécutez avec l'option `--watch` pour activé le [mode watch](docs/recipes/watch-mode.md) de AVA.

```console
npx ava --watch
```

## Version Node.js supportée

AVA prend en charge la dernière version de toute version majeure [prise en charge par Node.js](https://github.com/nodejs/Release#release-schedule). Lisez la suite dans notre [déclaration de support](docs/support-statement.md).

## Points forts

### Magic assert

AVA ajoute les extraits de code et les différences pour les valeurs réelles et attendues. Si les valeurs dans l'assertion sont des objets ou des tableaux, seule la différence est affichée, tout ce qui ne concerne pas le sujet est enlevé, pour se concentrer uniquement sur le problème. La différence est aussi mise en évidence ! Si vous comparez des strings, à la fois sur une ou plusieurs lignes, AVA affiche un autre type de sortie, mettant en évidence les caractères supplémentaires ou manquants.

![](https://github.com/avajs/ava/blob/master/media/magic-assert-combined.png)

### Nettoyage de la stack trace

AVA supprime automatiquement les lignes sans rapport dans la stack trace, cela permet de trouver la source d'une erreur plus rapidement, comme présenté ci-dessous.

### Exécution en parallèle dans CI

AVA détecte automatiquement si votre environnement CI prend en charge les builds en parallèle. Chaque build exécutera un sous-ensemble de tous les fichiers de test, tout en veillant à ce que tous les tests soient exécutés. Consultez le package [`ci-parallel-vars`](https://www.npmjs.com/package/ci-parallel-vars) pour visualiser la liste des environnements CI pris en charge.

## Documentation

Veuillez consulter les [fichiers dans le répertoire `docs`](./docs) :

* [Ecriture des tests](./docs/01-writing-tests.md)
* [Contexte d'exécution](./docs/02-execution-context.md)
* [Assertions](./docs/03-assertions.md)
* [Test d'instantané](./docs/04-snapshot-testing.md)
* [Ligne de commande (CLI)](./docs/05-command-line.md)
* [Configuration](./docs/06-configuration.md)
* [Timeouts des tests](./docs/07-test-timeouts.md)

### Pièges classiques

Nous avons une liste croissante de [pièges classiques](docs/08-common-pitfalls.md) que vous pouvez rencontrer lors de l'utilisation AVA. Si vous rencontrez des problèmes que vous jugez habituels, commentez les dans [cette issue](https://github.com/avajs/ava/issues/404).

## Recettes

- [Configuration de test](docs/recipes/test-setup.md)
- [Couverture de code](docs/recipes/code-coverage.md)
- [Mode watch](docs/recipes/watch-mode.md)
- [Tester un endpoint](docs/recipes/endpoint-testing.md)
- [Quand utiliser `t.plan()` ?](docs/recipes/when-to-use-plan.md)
- [Tests de navigateur](docs/recipes/browser-testing.md)
- [TypeScript](docs/recipes/typescript.md)
- [Flow](docs/recipes/flow.md)
- [Configuration de Babel](https://github.com/avajs/babel)
- [Utilisation des modules ES](docs/recipes/es-modules.md)
- [Passer des arguments à vos fichiers de test](docs/recipes/passing-arguments-to-your-test-files.md)
- [Tester les composants React](docs/recipes/react.md)
- [Tester les composants Vue.js](docs/recipes/vue.md)
- [JSPM et SystemJS](docs/recipes/jspm-systemjs.md)
- [Débogage des tests avec Chrome DevTools](docs/recipes/debugging-with-chrome-devtools.md)
- [Débogage des tests avec VSCode](docs/recipes/debugging-with-vscode.md)
- [Débogage des tests avec WebStorm](docs/recipes/debugging-with-webstorm.md)
- [Tests d'intégration isolés de MongoDB](docs/recipes/isolated-mongodb-integration-tests.md)
- [Test d'applications Web à l'aide de Puppeteer](docs/recipes/puppeteer.md)

## FAQ

### Pourquoi ne pas utiliser `mocha`, `tape` ou `tap` ?

Mocha vous oblige à utiliser les globales implicites comme `describe` et `it` avec l'interface par défaut (que la plupart des gens utilisent). Il est pas très opiniâtre et exécute des tests en série sans isolation du processus, ce qui le rend lent.

Tape et node-tap sont bons. AVA est fortement inspiré par leur syntaxe. Tous les deux exécutent les tests en série. La restitution de [TAP](https://testanything.org) n'est pas facile à lire donc vous avez toujours besoin de l'aide d'un "reporter" tap.

Au contraire, AVA est très opiniâtre et exécute les tests en simultané, avec des processus distincts pour chaque fichier de test. Son "reporter" par défaut est agréable à regarder et pourtant AVA soutient encore la restitution de TAP à travers une option du CLI.

### Comment l'écrire et le prononcer ?

AVA, pas Ava ni ava. Prononcez [`/ˈeɪvə/`](https://github.com/avajs/ava/blob/master/media/pronunciation.m4a?raw=true) : eil (ort**eil**, or**eil**ler) V (**v**ie, a**v**oir) A (m**a**tin, **a**voir).

### Que représente l'arrière plan de l'image ?

C'est la [galaxie d'Andromède.](https://simple.wikipedia.org/wiki/Andromeda_galaxy).

### Quelle est la différence entre simultanéité et parallélisme

[La simultanéité n'est pas du parallélisme. Il permet le parallélisme.](http://stackoverflow.com/q/1050222).

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
- [Vous aimez AVA ? Faites un don ici !](https://opencollective.com/ava)
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
