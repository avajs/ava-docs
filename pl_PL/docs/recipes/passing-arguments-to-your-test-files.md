# Przekazywanie argumentów do plików testowych

Tłumaczenie: [Français](https://github.com/avajs/ava-docs/blob/main/fr_FR/docs/recipes/passing-arguments-to-your-test-files.md)

Możesz przekazać argumenty wiersza poleceń do plików testowych. Użyj argumentu `--` terminatora aby oddzielić argumenty AVA od własnych:

```js
// test.js
const test = require('ava');

test('argv', t => {
	t.deepEqual(process.argv.slice(2), ['--hello', 'world']);
});
```

```console
$ npx ava -- --hello world
```

Potrzebujesz dwóch `--` argumentów terminatorów jeśli wywołujesz AVA za pomocą skryptu `npm test`:

```json
{
	"scripts": {
		"test": "ava"
	}
}
```

```console
$ npm test -- -- --hello world
```
