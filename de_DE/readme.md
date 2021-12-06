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
- [Isolated environment for each test file](./docs/01-writing-tests.md#process-isolation)
- [Promise support](./docs/01-writing-tests.md#promise-support)
- [Async function support](./docs/01-writing-tests.md#async-function-support)
- [Observable support](./docs/01-writing-tests.md#observable-support)
- [Enhanced assertion messages](./docs/03-assertions.md#enhanced-assertion-messages)
- [Automatic parallel test runs in CI](#parallel-runs-in-ci)
- [TAP reporter](./docs/05-command-line.md#tap-reporter)


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
