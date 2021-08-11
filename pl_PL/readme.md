# <img src="https://github.com/avajs/ava/raw/main/media/header.png" title="AVA" alt="AVA logo" width="530">

[![Build Status](https://travis-ci.org/avajs/ava.svg?branch=master)](https://travis-ci.org/avajs/ava)  [![Coverage Status](https://codecov.io/gh/avajs/ava/branch/master/graph/badge.svg)](https://codecov.io/gh/avajs/ava/branch/master) [![XO code style](https://img.shields.io/badge/code_style-XO-5ed9c7.svg)](https://github.com/xojs/xo) [![Join the community on Spectrum](https://withspectrum.github.io/badge/badge.svg)](https://spectrum.chat/ava)
[![Mentioned in Awesome Node.js](https://awesome.re/mentioned-badge.svg)](https://github.com/sindresorhus/awesome-nodejs)

Testowanie może być trudne. AVA pomaga je zrobić. AVA jest programem uruchamiającym testy dla Node.js ze zwięzłym API, szczegółowym wyjściem błędu, nowymi funkcjami języka i izolacją procesów, które pozwalają na bardziej efektywne pisanie testów. Więc możesz wysłać więcej niesamowitego kodu. 🚀

Obserwuj [konto AVA na Twitter](https://twitter.com/ava__js) dla aktualizacji.

Przeczytaj nasz [contributing guide](https://github.com/mbiesiad/ava-docs/blob/pl_PL/pl_PL/contributing.md) jeśli chcesz pomóc (issues / PRs / etc).

![](https://github.com/avajs/ava/raw/main/media/mini-reporter.gif)


Tłumaczenia: [Español](https://github.com/avajs/ava-docs/blob/master/es_ES/readme.md), [Français](https://github.com/avajs/ava-docs/blob/master/fr_FR/readme.md), [Italiano](https://github.com/avajs/ava-docs/blob/master/it_IT/readme.md), [日本語](https://github.com/avajs/ava-docs/blob/master/ja_JP/readme.md), [한국어](https://github.com/avajs/ava-docs/blob/master/ko_KR/readme.md), [Português](https://github.com/avajs/ava-docs/blob/master/pt_BR/readme.md), [Русский](https://github.com/avajs/ava-docs/blob/master/ru_RU/readme.md), [简体中文](https://github.com/avajs/ava-docs/blob/master/zh_CN/readme.md)


## Dlaczego AVA?

- Minimalna i szybka
- Prosta składnia testu
- Prowadzi testy jednocześnie
- Wymusza pisanie testów atomowych
- Żadnych niejawnych globalów
- Zawiera definicje TypeScript
- [Magic assert](#magic-assert)
- [Izolowane środowisko dla każdego pliku testowego](./docs/01-writing-tests.md#proces-izolacji)
- [Napisz swoje testy przy użyciu najnowszej składni JavaScript](https://github.com/avajs/babel)
- [Wsparcie promise](./docs/01-writing-tests.md#wsparcie-promise)
- [Wsparcie funkcji asynchronicznych](./docs/01-writing-tests.md#wsparcie-funkcji-async)
- [Wsparcie Observable](./docs/01-writing-tests.md#wsparcie-observable)
- [Ulepszone komunikaty assertion](./docs/03-assertions.md#wzmocnione-komunikaty-asercji)
- [Uruchamiany automatyczny test równoległy CI](#równoległe-działanie-w-ci)
- [TAP reporter](./docs/05-command-line.md#tap-reporter)


## Stosowanie

Aby zainstalować i skonfigurować AVA, uruchom:

```console
npm init ava
```

Twój `package.json` będzie wyglądać następująco (niezależnie od dokładnej wersji):

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

Lub jeśli wolisz używać Yarn:

```console
yarn add ava --dev
```

Alternatywnie możesz zainstalować `ava` manualnie:

```console
npm install --save-dev ava
```

Nie zapomnij skonfigurować skryptu `test` w swoim `package.json` jak powyżej.

### Utwórz plik testowy

Stwórz plik nazwany `test.js` w katalogu głównym projektu:

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

### Przeprowadzanie testów

```console
npm test
```

Lub z `npx`:

```console
npx ava
```

Uruchom z flagą `--watch` aby włączyć AVA'y [watch mode](docs/recipes/watch-mode.md):

```console
npx ava --watch
```

## Wspierane wersje Node.js

AVA obsługuje najnowszą wersję dowolnej głównej wersji, która [jest obsługiwana przez sam Node.js](https://github.com/nodejs/Release#release-schedule). Przeczytaj więcej w naszej [deklaracji wsparcia](docs/support-statement.md).

## Najważniejsze

### Magic assert

AVA dodaje fragmenty kodu i czyści różnice dla wartości rzeczywistych i oczekiwanych. Jeśli wartości w asercji są obiektami lub tablicami, wyświetlana jest tylko różnica, aby usunąć szum i skupić się na problemie. Różnica jest również wyróżniona w składni! Jeśli porównujesz ciągi znaków, zarówno jedno jak i wieloliniowe, AVA wyświetla inny rodzaj danych wyjściowych, podkreślając dodane lub brakujące znaki.

![](https://github.com/avajs/ava/raw/main/media/magic-assert-combined.png)

### Czyszczenie śladów stosu

AVA automatycznie usuwa niepowiązane linie ze śladów stosu, co pozwala znacznie szybciej znaleźć źródło błędu, jak pokazano powyżej.

### Równoległe działanie w CI

AVA automatycznie wykrywa, czy środowisko CI obsługuje równoległe buildy. Każda kompilacja uruchomi podzbiór wszystkich plików testowych, jednocześnie upewniając się, że wszystkie testy zostaną wykonane. Zobacz [`ci-parallel-vars`](https://www.npmjs.com/package/ci-parallel-vars) pakiet zawierający listę obsługiwanych środowisk CI.

## Dokumentacja

Proszę zobacz [pliki w katalogu `docs`](./docs):

* [Pisanie testów](./docs/01-writing-tests.md)
* [Kontekst wykonania](./docs/02-execution-context.md)
* [Asercje](./docs/03-assertions.md)
* [Snapshot testing](./docs/04-snapshot-testing.md)
* [Linia poleceń (CLI)](./docs/05-command-line.md)
* [Konfiguracja](./docs/06-configuration.md)
* [Test timeouts](./docs/07-test-timeouts.md)

### Częste pułapki

Mamy coraz więcej [typowych pułapek](docs/08-common-pitfalls.md) których możesz doświadczyć podczas korzystania z AVA. Jeśli napotkasz jakieś problemy, które uważasz za typowe, skomentuj w [tym issue](https://github.com/avajs/ava/issues/404).

### Recipes

- [Konfiguracja testowa](docs/recipes/test-setup.md)
- [Pokrycie kodu](docs/recipes/code-coverage.md)
- [Watch mode](docs/recipes/watch-mode.md)
- [Testowanie Endpoint](docs/recipes/endpoint-testing.md)
- [Kiedy użyć `t.plan()`](docs/recipes/when-to-use-plan.md)
- [Testowanie przeglądarki](docs/recipes/browser-testing.md)
- [TypeScript](docs/recipes/typescript.md)
- [Flow](docs/recipes/flow.md)
- [Konfiguracja Babel](https://github.com/avajs/babel)
- [Korzystanie z modułów ES](docs/recipes/es-modules.md)
- [Przekazywanie argumentów do plików testowych](docs/recipes/passing-arguments-to-your-test-files.md)
- [Testowanie komponentów React](docs/recipes/react.md)
- [Testowanie komponentów Vue.js](docs/recipes/vue.md)
- [JSPM oraz SystemJS](docs/recipes/jspm-systemjs.md)
- [Debugowanie testów z Chrome DevTools](docs/recipes/debugging-with-chrome-devtools.md)
- [Debugowanie testów z VSCode](docs/recipes/debugging-with-vscode.md)
- [Debugowanie testów z WebStorm](docs/recipes/debugging-with-webstorm.md)
- [Izolowane testy integracyjne MongoDB](docs/recipes/isolated-mongodb-integration-tests.md)
- [Testowanie aplikacji internetowych używając Puppeteer](docs/recipes/puppeteer.md)
- [Testowanie aplikacji internetowych używając Selenium WebDriverJS](docs/recipes/testing-with-selenium-webdriverjs.md)

## FAQ

### Dlaczego nie `mocha`, `tape`, `tap`?

Mocha wymaga, abyś używał bezwględnych globali takich jak `describe` i `it` z domyślnym interfejsem (z którego korzysta większość osób). Nie jest bardzo opiniotwórcza i wykonuje testy szeregowo bez izolacji procesu, co spowalnia.

Tape i tap są całkiem niezłe. AVA jest wysoce zainspirowana ich składnią. One również wykonują testy szeregowo. Ich domyślne [TAP](https://testanything.org) wyjście nie jest jednak zbyt przyjazne dla użytkownika, więc zawsze korzystasz z zewnętrznego tap reportera.

W przeciwieństwie, AVA jest bardzo pewny siebie i uruchamia testy jednocześnie, z osobnym procesem dla każdego pliku testowego. Domyślny reporter jest przyjemny dla oczu, a mimo to AVA nadal obsługuje wyjście TAP poprzez flagę CLI.

### Jak zapisuje się i wymawia nazwę?

AVA, nie Ava lub ava. Wymawiane [`/ˈeɪvə/`](media/pronunciation.m4a?raw=true): Ay (f**a**ce, m**a**de) V (**v**ie, ha**v**e) A (comm**a**, **a**go)

### Jakie jest tło nagłówka?

Jest to [galaktyka Andromeda](https://simple.wikipedia.org/wiki/Andromeda_galaxy).

### Jaka jest różnica między współbieżnością a równoległością?

[Współbieżność nie jest równoległością. Umożliwia równoległość.](https://stackoverflow.com/q/1050222)

## Wsparcie

- [Stack Overflow](https://stackoverflow.com/questions/tagged/ava)
- [Spectrum](https://spectrum.chat/ava)
- [Twitter](https://twitter.com/ava__js)

## Powiązane

- [eslint-plugin-ava](https://github.com/avajs/eslint-plugin-ava) - Zasady Linta dla testów AVA
- [sublime-ava](https://github.com/avajs/sublime-ava) - Snippets dla testów AVA
- [atom-ava](https://github.com/avajs/atom-ava) - Snippets dla testów AVA
- [vscode-ava](https://github.com/samverschueren/vscode-ava) - Snippets dla testów AVA
- [gulp-ava](https://github.com/avajs/gulp-ava) - Uruchamianie testów z gulp
- [grunt-ava](https://github.com/avajs/grunt-ava) - Uruchamianie testów z grunt
- [Więcej…](https://github.com/avajs/awesome-ava#packages)

## Linki

- [AVA stickers, t-shirts, etc](https://www.redbubble.com/people/sindresorhus/works/30330590-ava-logo)
- [Awesome list](https://github.com/avajs/awesome-ava)
- [AVA Casts](http://avacasts.com)
- [Lubisz AVA? Wesprzyj tutaj!](https://opencollective.com/ava)
- [Więcej…](https://github.com/avajs/awesome-ava)

## Zespół

[![Mark Wubben](https://github.com/novemberborn.png?size=100)](https://github.com/novemberborn) | [![Sindre Sorhus](https://github.com/sindresorhus.png?size=100)](https://github.com/sindresorhus) | [![Vadim Demedes](https://github.com/vadimdemedes.png?size=100)](https://github.com/vadimdemedes)
---|---|---
[Mark Wubben](https://novemberborn.net) | [Sindre Sorhus](http://sindresorhus.com) | [Vadim Demedes](https://github.com/vadimdemedes)

###### Poprzedni członkowie

- [Kevin Mårtensson](https://github.com/kevva)
- [James Talmage](https://github.com/jamestalmage)
- [Juan Soto](https://github.com/sotojuan)
- [Jeroen Engels](https://github.com/jfmengels)

<div align="center">
	<br>
	<br>
	<br>
	<a href="https://avajs.dev">
		<img src="https://github.com/avajs/ava/raw/main/media/logo.svg" width="200" alt="AVA">
	</a>
	<br>
	<br>
</div>
