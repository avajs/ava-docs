# <img src="media/header.png" title="AVA" alt="AVA logo" width="530">

AVA ist ein Test-Runner für Node.js mit einer übersichtlichen API, detaillierter Fehlerausgabe, Einführung neuer Sprachfeatures und Prozessisolierung, die Ihnen eine sichere Entwicklung ermöglicht. 🚀

Folge dem [AVA Twitter account](https://twitter.com/ava__js) für updates.

Lies unsere [contributing guide](.github/CONTRIBUTING.md) wenn du zu dem Projekt beitragen möchtest (issues / PRs / etc).

![](media/mini-reporter.gif)


Übersetzungen: [German](https://github.com/avajs/ava-docs/blob/master/de_DE/readme.md), [Español](https://github.com/avajs/ava-docs/blob/master/es_ES/readme.md), [Français](https://github.com/avajs/ava-docs/blob/master/fr_FR/readme.md), [Italiano](https://github.com/avajs/ava-docs/blob/master/it_IT/readme.md), [日本語](https://github.com/avajs/ava-docs/blob/master/ja_JP/readme.md), [한국어](https://github.com/avajs/ava-docs/blob/master/ko_KR/readme.md), [Português](https://github.com/avajs/ava-docs/blob/master/pt_BR/readme.md), [Русский](https://github.com/avajs/ava-docs/blob/master/ru_RU/readme.md), [简体中文](https://github.com/avajs/ava-docs/blob/master/zh_CN/readme.md)


## Warum AVA?

- Minimal und schnell
- Einfache Testsyntax
- Führt gleichzeitig Tests durch
- Erzwingt das Schreiben atomarer Tests
- Keine impliziten Globals
- Enthält TypeScript-Definitionen
- [Magic assert](#magic-assert)
- [Isolierte Umgebung für jede Testdatei](./docs/01-writing-tests.md#process-isolation)
- [Promise-Unterstützung](./docs/01-writing-tests.md#promise-support)
- [Async-Funktionsunterstützung](./docs/01-writing-tests.md#async-function-support)
- [Beobachtbare Unterstützung](./docs/01-writing-tests.md#observable-support)
- [Erweiterte Assertion-Nachrichten](./docs/03-assertions.md#enhanced-assertion-messages)
- [Automatische parallele Testläufe in CI](#parallel-runs-in-ci)
- [TAP-Reporter](./docs/05-command-line.md#tap-reporter)


## Benutzung

Um AVA zu installieren und anzupassen, führe folgendes aus:

```console
npm init ava
```

Dein `package.json` sieht dann so aus (ungeachtet der genauen Version):

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

Oder falls du Yarn bevorzugst:

```console
yarn add ava --dev
```

Alternativ kannst du `ava` auch manuell installieren:

```console
npm install --save-dev ava
```

Vergiss nicht das `test` script in deinem `package.json` wie oben zu konfigurieren.

### Erstelle deine eigene Test Datei

Erstelle eine Datei namens `test.js` im Stammverzeichnis deines Projekts:

```js
const test = require('ava');

test('foo', t => {
	t.pass();
});

test('bar', async t => {
	const bar = Promise.resolve('bar');
	t.is(await bar, 'bar');
});
```

### Deine Tests ausführen

```console
npm test
```

oder mit `npx`:

```console
npx ava
```

Führe mit der `--watch` Option aus, um AVA's [watch mode](docs/recipes/watch-mode.md) zu benutzen:

```console
npx ava --watch
```

## Unterstützte Node.js Versionen

AVA unterstützt die neueste Version jeder Hauptversion, die [von Node.js selbst unterstützt wird](https://github.com/nodejs/Release#release-schedule). Lesen Sie mehr in unserer [support-Anweisung](docs/support-statement.md).

## Highlights

### Magie

AVA fügt Codeauszüge und saubere Diffs für tatsächliche und erwartete Werte hinzu. Wenn es sich bei den Werten in der Assertion um Objekte oder Arrays handelt, wird nur ein Diff angezeigt, um das Rauschen zu entfernen und sich auf das Problem zu konzentrieren. Das Diff ist auch Syntax-hervorgehoben! Wenn Sie sowohl einzeilige als auch mehrzeilige Zeichenfolgen vergleichen, zeigt AVA eine andere Art der Ausgabe an und hebt die hinzugefügten oder fehlenden Zeichen hervor.

![](media/magic-assert-combined.png)

### Saubere Stack-Traces

AVA entfernt automatisch nicht verwandte Zeilen in Stack-Traces, sodass Sie die Fehlerquelle viel schneller finden können, wie oben gezeigt.

### Parallele Läufe in CI

AVA erkennt automatisch, ob Ihre CI-Umgebung parallele Builds unterstützt. Jeder Build führt eine Teilmenge aller Testdateien aus und stellt gleichzeitig sicher, dass alle Tests ausgeführt werden. Eine Liste der unterstützten CI-Umgebungen finden Sie im Paket [`ci-parallel-vars`](https://www.npmjs.com/package/ci-parallel-vars).

## Dokumentation

Bitte sehen Sie sich die [Dateien im `docs`-Verzeichnis](./docs) an:

* [Writing tests](./docs/01-writing-tests.md)
* [Execution context](./docs/02-execution-context.md)
* [Assertions](./docs/03-assertions.md)
* [Snapshot testing](./docs/04-snapshot-testing.md)
* [Command line (CLI)](./docs/05-command-line.md)
* [Configuration](./docs/06-configuration.md)
* [Test timeouts](./docs/07-test-timeouts.md)

### Häufige Fehler

Wir haben eine wachsende Liste von [gemeinsamen Fallstricken](docs/08-common-pitfalls.md), die bei der Verwendung von AVA auftreten können. Wenn Sie auf Probleme stoßen, die Sie für häufig halten, kommentieren Sie in [diesem Problem](https://github.com/avajs/ava/issues/404).

### Recipes

- [Shared Workers](docs/recipes/shared-workers.md)
- [Testeinrichtung](docs/recipes/test-setup.md)
- [Codeabdeckung](docs/recipes/code-coverage.md)
- [Watch-Modus](docs/recipes/watch-mode.md)
- [Endpoint-Testing](docs/recipes/endpoint-testing.md)
- [Wann `t.plan()` verwendet werden sollte](docs/recipes/when-to-use-plan.md)
- [Browsertests](docs/recipes/browser-testing.md)
- [TypeScript](docs/recipes/typescript.md)
- [Verwenden von ES-Modulen](docs/recipes/es-modules.md)
- [Übergeben von Argumenten an Ihre Testdateien](docs/recipes/passing-arguments-to-your-test-files.md)
- [Testen von React-Komponenten](docs/recipes/react.md)
- [Testen von Vue.js-Komponenten](docs/recipes/vue.md)
- [Debugging-Tests mit Chrome DevTools](docs/recipes/debugging-with-chrome-devtools.md)
- [Debugging-Tests mit VSCode](docs/recipes/debugging-with-vscode.md)
- [Debugging-Tests mit WebStorm](docs/recipes/debugging-with-webstorm.md)
- [Isolierte MongoDB-Integrationstests](docs/recipes/isolated-mongodb-integration-tests.md)
- [Testen von Web-Apps mit Puppeteer](docs/recipes/puppeteer.md)
- [Testen von Web-Apps mit Selenium WebDriverJS](docs/recipes/testing-with-selenium-webdriverjs.md)

## FAQ

### Warum nicht `mocha`, `tape`, `tap`?

Mocha erfordert, dass Sie implizite Globals wie `describe` und `it` mit der Standardschnittstelle (die die meisten Leute verwenden) verwenden. Es ist nicht sehr eigensinnig und führt Tests seriell ohne Prozessisolierung aus, was es langsam macht.

Tape und Tap sind ziemlich gut. AVA ist stark von ihrer Syntax inspiriert. Auch sie führen Tests seriell durch. Ihre Standardausgabe [TAP](https://testanything.org) ist jedoch nicht sehr benutzerfreundlich, sodass Sie am Ende immer einen externen Tap-Reporter verwenden.

Im Gegensatz dazu ist AVA sehr eigenwillig und führt gleichzeitig Tests durch, mit einem separaten Prozess für jede Testdatei. Der Standardreporter ist angenehm für die Augen und dennoch unterstützt AVA weiterhin die TAP-Ausgabe über ein CLI-Flag.

### Wie wird der Name geschrieben und ausgesprochen?

AVA, nicht Ava oder ava. Ausgesprochen [`/ˈeɪvə/`](media/pronunciation.m4a?raw=true): Ay (f**a**ce, m**a**de) V (**v**ie, ha**v**e) A (comm**a**, **a**go)

### Was ist der Header-Hintergrund?

Es ist die [Andromeda galaxy](https://simple.wikipedia.org/wiki/Andromeda_galaxy).

### Was ist der Unterschied zwischen Gleichzeitigkeit und Parallelität?

[Gleichzeitigkeit ist keine Parallelität. Es ermöglicht Parallelität.](https://stackoverflow.com/q/1050222)

## Unterstützung

- [GitHub Discussions](https://github.com/avajs/ava/discussions)

## Verwandt

- [eslint-plugin-ava](https://github.com/avajs/eslint-plugin-ava) — Lint-Regeln für AVA-Tests
- [@ava/typescript](https://github.com/avajs/typescript) — TypeScript-Projekte testen
- [@ava/babel](https://github.com/avajs/babel) — Kompilieren Sie Testdateien mit Babel, für AVA 3
- [@ava/cooperate](https://github.com/avajs/cooperate) — Low-Level-Primitive, um die Zusammenarbeit zwischen Testdateien zu ermöglichen
- [@ava/get-port](https://github.com/avajs/get-port) – Reservieren Sie einen Port während des Tests

## Links

- [AVA stickers, t-shirts, etc](https://www.redbubble.com/people/sindresorhus/works/30330590-ava-logo)
- [Awesome list](https://github.com/avajs/awesome-ava)
- [Do you like AVA? Donate here!](https://opencollective.com/ava)
- [More…](https://github.com/avajs/awesome-ava)

## Team

[![Mark Wubben](https://github.com/novemberborn.png?size=100)](https://github.com/novemberborn) | [![Sindre Sorhus](https://github.com/sindresorhus.png?size=100)](https://github.com/sindresorhus)
---|---
[Mark Wubben](https://novemberborn.net) | [Sindre Sorhus](https://sindresorhus.com)

###### Former

- [Kevin Mårtensson](https://github.com/kevva)
- [James Talmage](https://github.com/jamestalmage)
- [Juan Soto](https://github.com/sotojuan)
- [Jeroen Engels](https://github.com/jfmengels)
- [Vadim Demedes](https://github.com/vadimdemedes)


<div align="center">
	<br>
	<br>
	<br>
	<a href="https://avajs.dev">
		<img src="media/logo.svg" width="200" alt="AVA">
	</a>
	<br>
	<br>
</div>