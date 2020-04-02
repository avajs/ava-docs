# Konfiguracja

Tłumaczenia: [Français](https://github.com/avajs/ava-docs/blob/master/fr_FR/docs/06-configuration.md)

Wszystkie [opcje CLI][CLI] można skonfigurować w sekcji `ava` któregoś z twoich plików `package.json`, lub w pliku `ava.config.*`. Pozwala to zmodyfikować domyślne zachowanie polecenia `ava`, więc nie musisz wielokrotnie wpisywać tych samych opcji w wierszu polecenia.

Aby zignorować pliki, poprzedź wzorzec znakiem `!` (wykrzyknik).

**`package.json`:**

```json
{
	"ava": {
		"files": [
			"test/**/*",
			"!test/exclude-files-in-this-directory",
			"!**/exclude-files-with-this-name.*"
		],
		"match": [
			"*oo",
			"!foo"
		],
		"concurrency": 5,
		"failFast": true,
		"failWithoutAssertions": false,
		"environmentVariables": {
			"MY_ENVIRONMENT_VARIABLE": "some value"
		},
		"verbose": true,
		"require": [
			"./my-helper-module.js"
		],
		"nodeArguments": [
			"--trace-deprecation",
			"--napi-modules"
		]
	}
}
```

Argumenty przekazywane do interfejsu CLI zawsze będą miały pierwszeństwo przed opcjami CLI skonfigurowanymi w `package.json`.

## Opcje

- `files`: tablica wzorców globalnych do wybierania plików testowych. Pliki z prefiksem podkreślenia są ignorowane. Domyślnie wybiera tylko pliki z rozszerzeniami `cjs`, `mjs` & `js`, nawet jeśli wzór pasuje do innych plików. Sprecyzuj `extensions` aby zezwolić na inne rozszerzenia plików
- `ignoredByWatcher`: tablica wzorców globu pasujących do plików, które nawet jeśli zostaną zmienione, są ignorowane przez obserwatora. Zobacz [przepis trybu watch, aby poznać szczegóły](https://github.com/avajs/ava/blob/master/docs/recipes/watch-mode.md)
- `match`: zwykle nie jest użyteczne w konfiguracji `package.json`, ale jest równoważne z [określaniem `--match` w CLI](./05-command-line.md#running-tests-with-matching-titles)
- `cache`: cache kompiluje pliki pod `node_modules/.cache/ava`. Jeśli `false`, zamiast tego pliki są buforowane w katalogu tymczasowym
- `failFast`: przestań uruchamiać kolejne testy, gdy test się nie powiedzie
- `failWithoutAssertions`: jeśli `false`, nie zawiedzie testu, jeśli nie uruchomi się [asercje](./03-assertions.md)
- `environmentVariables`: określa zmienne środowiskowe, które zostaną udostępnione testom. Zdefiniowane tutaj zmienne środowiskowe zastępują te z `process.env`
- `tap`: jeśli `true`, umożliwia [TAP reporter](./05-command-line.md#tap-reporter)
- `verbose`: jeśli `true`, umożliwia verbose output
- `snapshotDir`: określa stałą lokalizację przechowywania plików migawek. Użyj tego, jeśli twoje migawki kończą się w złym miejscu
- `extensions`: rozszerzenia plików testowych. Ustawienie to zastępuje wartość domyślną `["cjs", "mjs", "js"]`, więc pamiętaj, aby uwzględnić te rozszerzenia na liście
- `require`: dodatkowe moduły wymagane przed uruchomieniem testów. Moduły są wymagane w [procesach roboczych](./01-writing-tests.md#process-isolation)
- `timeout`: Limity czasu w AVA zachowują się inaczej niż w innych ramach testowych. AVA resetuje licznik czasu po każdym teście, zmuszając testy do zakończenia, jeśli nie otrzymano nowych wyników testu w określonym czasie. Można to wykorzystać do obsługi zablokowanych testów. Zobacz naszą [dokumentację timeouts](./07-test-timeouts.md) for more options.
- `nodeArguments`: Skonfiguruj argumenty Node.js używane do uruchamiania procesów roboczych.

Pamiętaj, że udostępnianie plików w interfejsie CLI zastępuje tę opcję `files`.

Podaj opcję `babel` (i zainstaluj [`@ava/babel`](https://github.com/avajs/babel) jako dodatkową zależność), aby włączyć kompilację Babel.

Podaj opcję `typescript` (i zainstaluj[`@ava/typescript`](https://github.com/avajs/typescript) jako dodatkowa zależność), aby włączyć (podstawową) obsługę TypeScript.

## Używanie plików `ava.config.*`

Zamiast określać konfigurację w pliku `package.json` możesz użyć plików `ava.config.js` lub `ava.config.cjs`.

Aby użyć tych plików:

1. Muszą znajdować się w tym samym katalogu co twój `package.json`
2. Twój `package.json` nie może zawierać właściwości `ava` (lub jeśli tak, musi to być pusty obiekt)
3. Nie możesz mieć obu `ava.config.js` *oraz* pliku `ava.config.cjs`

AVA rozpoznaje pliki `ava.config.mjs` ale odmawia ich załadowania.

### `ava.config.js`

Dla plików `ava.config.js` musisz użyć `export default`. Nie możnesz użyć ["zakresu modułu"](https://nodejs.org/docs/latest-v12.x/api/modules.html#modules_the_module_scope). Nie możesz importować zależności.

Domyślnym eksportem może być zwykły obiekt lub fabryczna funkcja, która zwraca zwykły obiekt:

```js
export default {
	require: ['./_my-test-helper']
};
```

```js
export default function factory() {
	return {
		require: ['./_my-test-helper']
	};
};
```

Funkcja fabryczna jest wywoływana z obiektem zawierającym właściwość `projectDir`, którego możesz użyć do zmiany zwróconej konfiguracji:

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

Pamiętaj, że ostateczna konfiguracja nie może być obietnicą (promise).

### `ava.config.cjs`

Dla plików `ava.config.cjs` musisz przypisać `module.exports`. ["Zakres modułu"](https://nodejs.org/docs/latest-v12.x/api/modules.html#modules_the_module_scope) jest dostępny. Możesz `require()` zależności.

Eksport modułu może być zwykłym obiektem lub funkcją fabryczną, która zwraca zwykły obiekt:

```js
module.exports = {
	require: ['./_my-test-helper']
};
```

```js
module.exports = () => {
	return {
		require: ['./_my-test-helper']
	};
};
```

Funkcja fabryczna jest wywoływana z obiektem zawierającym właściwość `projectDir`, której możesz użyć do zmiany zwróconej konfiguracji:

```js
module.exports = ({projectDir}) => {
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

Pamiętaj, że ostateczna konfiguracja nie może być promise.

## Alternatywne pliki konfiguracyjne

[CLI] pozwala określić konkretny plik konfiguracyjny za pomocą flagi `--config`. Ten plik musi mieć albo `.js` lub rozszerzenie `.cjs` i jest przetwarzany jak `ava.config.js` lub plik `ava.config.cjs` byłby.

Kiedy flaga `--config` jest ustawiona, podany plik zastąpi całą konfigurację z `package.json` i `ava.config.js` lub plików `ava.config.cjs`. Konfiguracja nie jest mergowana.

Plik konfiguracyjny *musi* znajdować się w tym samym katalogu, co plik `package.json`.

Możesz użyć tego do dostosowania konfiguracji dla konkretnego uruchomienia testowego. Na przykład możesz chcieć uruchomić testy jednostkowe oddzielnie od testów integracyjnych:

`ava.config.cjs`:

```js
module.exports = {
	files: ['unit-tests/**/*']
};
```

`integration-tests.config.cjs`:

```js
const baseConfig = require('./ava.config.cjs');

module.exports = {
	...baseConfig,
	files: ['integration-tests/**/*']
};
```

Możesz teraz przeprowadzić testy jednostkowe `npx ava` i testy integracyjne poprzez `npx ava --config integration-tests.config.cjs`.

## Głębokość drukowania obiektu

Domyślnie AVA drukuje zagnieżdżone obiekty na głębokości `3`. Jednak podczas debugowania testów z głęboko zagnieżdżonymi obiektami przydatne może być drukowanie z większą ilością szczegółów. Można to zrobić przez ustawienie [`util.inspect.defaultOptions.depth`](https://nodejs.org/api/util.html#util_util_inspect_defaultoptions) na żądaną głębokość przed wykonaniem testu:

```js
const util = require('util');

const test = require('ava');

util.inspect.defaultOptions.depth = 5;  // Increase AVA's printing depth

test('My test', t => {
	t.deepEqual(someDeeplyNestedObject, theExpectedValue);
});
```

AVA ma minimalną głębokość `3`.

## Eksperymenty

Od czasu do czasu AVA będzie wdrażać funkcje eksperymentalne. Mogą one ulec zmianie lub zostać usunięte w dowolnym momencie, nie tylko wtedy, gdy pojawi się nowa wersja główna. Możesz włączyć taką funkcję, włączając ją w konfiguracji `nonSemVerExperiments`.

`ava.config.js`:
```js
export default {
	nonSemVerExperiments: {
		feature: true
	}
};
```

## Argumenty Node

Konfiguracja `nodeArguments` może być użyta do podania dodatkowych argumentów do uruchomienia procesów roboczych. Są one połączone z `--node-arguments` przekazanych do CLI i wszelkimi argumentami przekazanymi do binarki `node` podczas uruchamiania AVA.

[CLI]: ./05-command-line.md
