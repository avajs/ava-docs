# Konfigurowanie AVA do testowania w przeglądarce

Tłumaczenia: [Español](https://github.com/avajs/ava-docs/blob/master/es_ES/docs/recipes/browser-testing.md), [Français](https://github.com/avajs/ava-docs/blob/master/fr_FR/docs/recipes/browser-testing.md), [Italiano](https://github.com/avajs/ava-docs/blob/master/it_IT/docs/recipes/browser-testing.md), [Русский](https://github.com/avajs/ava-docs/blob/master/ru_RU/docs/recipes/browser-testing.md), [简体中文](https://github.com/avajs/ava-docs/blob/master/zh_CN/docs/recipes/browser-testing.md)

AVA nie obsługuje uruchamiania testów w przeglądarkach [jeszcze](https://github.com/avajs/ava/issues/24). Jednak biblioteki JavaScript wymagające specyficznych globalów dla przeglądarki (`window`, `document`, `navigator`, etc) nadal można przetestować za pomocą AVA, mockując te globale.

Ten przepis działa na każdą bibliotekę, która potrzebuje zmockowanego środowiska przeglądarki.

## Zainstaluj browser-env

> **❗️ Ważna uwaga**
>
>`browser-env` dodaje właściwości z okna `jsdom` przestrzeni nazw do globalnej przestrzeni nazw Node.js. Jest to wyraźnie [zalecane przeciwko](https://github.com/tmpvar/jsdom/wiki/Don't-stuff-jsdom-globals-onto-the-Node-global) od `jsdom`. Przeczytaj podlinkowaną stronę wiki i upewnij się, że rozumiesz ostrzeżenia. Jeśli nie masz wielu zależności, które również wymagają środowiska przeglądarki, to [`window`](https://github.com/lukechilds/window#universal-testing-pattern) może być lepszym rozwiązaniem.

Instalacja [browser-env](https://github.com/lukechilds/browser-env).

> Symuluje globalne środowisko przeglądarki przy użyciu jsdom.

```
$ npm install --save-dev browser-env
```

## Ustaw browser-env

Utwórz plik pomocniczy, poprzedzony znakiem podkreślenia. Dzięki temu AVA nie traktuje tego jako testu.

`test/_setup-browser-env.js`:

```js
const browserEnv = require('browser-env');
browserEnv();
```

Domyślnie, `browser-env` doda wszystkie globalne zmienne przeglądarki do globalnego zasięgu Node.js, tworząc pełne środowisko przeglądarki. Powinno to mieć dobrą zgodność z większością bibliotek front-end, jednak generalnie nie jest dobrym pomysłem tworzenie wielu zmiennych globalnych, jeśli nie jest to konieczne. Jeśli wiesz dokładnie, jakich globałów przeglądarki potrzebujesz, możesz przekazać ich tablicę.

```js
const browserEnv = require('browser-env');
browserEnv(['window', 'document', 'navigator']);
```

Możesz ujawnić więcej zmiennych globalnych, przypisując je do obiektu `global`. Na przykład jQuery jest zazwyczaj dostępny za pośrednictwem zmiennej `$`:

```js
const browserEnv = require('browser-env');
const jQuery = require('jquery');

browserEnv();
global.$ = jQuery(window);
```

## Skonfiguruj testy do użycia browser-env

Skonfiguruj AVA do `require` helpera przed każdym plikiem testowym.

**`package.json`:**

```json
{
	"ava": {
		"require": [
			"./test/_setup-browser-env.js"
		]
	}
}
```

## Enjoy!

Napisz swoje testy i ciesz się zmockowanym środowiskiem przeglądarki.

`test.js`:

```js
const test = require('ava');

test('Insert to DOM', t => {
	const div = document.createElement('div');
	document.body.appendChild(div);

	t.is(document.querySelector('div'), div);
});
```
