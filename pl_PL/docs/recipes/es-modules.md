# Korzystanie z modułów ES w AVA

Tłumaczenie: [Français](https://github.com/avajs/ava-docs/blob/main/fr_FR/docs/recipes/es-modules.md)

Od Node.js 13, [ECMAScript Modules](https://nodejs.org/docs/latest/api/esm.html#esm_introduction) są natywnie obsługiwane w samym Node.js. AVA 3.3 obsługuje pliki testowe ESM, jednak obsługa jest niepełna. [Projekt wsparcia ESM](https://github.com/orgs/avajs/projects/2) śledzi nasze postępy.

Obsługa ESM w Node.js jest eksperymentalna, choć domyślnie włączona w Node.js 13. *Zobaczysz takie komunikaty `ExperimentalWarning: The ESM module loader is experimental` w output'cie AVA. Są one emitowane przez Node.js, a nie AVA.*

## Włączanie eksperymentalnej obsługi ESM w Node.js 12

W Node.js 12 musisz włączyć obsługę ESM. Możesz to zrobić za pomocą AVA, konfigurując `nodeArguments` w twoim `package.json` lub pliku `ava.config.*`:

**`package.json`:**

```json
{
	"ava": {
		"nodeArguments": [
			"--experimental-modules"
		]
	}
}
```

Lub w wierszu poleceń:

```console
npx ava --node-arguments '--experimental-modules' test.mjs
```

## Używanie pakietu `esm`

Jeśli chcesz korzystać ze składni ESM, bez polegania na implementacji Node.js, najlepszym rozwiązaniem jest użycie pakietu [`esm`](https://github.com/standard-things/esm). Pamiętaj, aby użyć rozszerzenia `.js` i *nie* ustawić `"type": "module"` w `package.json`.

Oto, jak możesz pracować z AVA.

Najpierw zainstaluj `esm`:

```
$ npm install esm
```

Skonfiguruj go w swoim pliku `package.json` lub `ava.config.*`, i dodaj go do opcji AVA `"require"` tak właśnie. Pamiętaj, aby dodać go jako pierwszy element.

**`package.json`:**

```json
{
	"ava": {
		"require": [
			"esm"
		]
	}
}
```

Możesz teraz używać natywnych modułów ES z AVA:

```js
// sum.js
export default function sum(a, b) {
	return a + b;
};
```

```js
// test.js
const test = require('ava');
const sum = require('./sum');

test('2 + 2 = 4', t => {
	t.is(sum(2, 2), 4);
});
```
