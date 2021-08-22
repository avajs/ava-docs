# Testowanie snapshotów

Tłumaczenia: [Français](https://github.com/avajs/ava-docs/blob/main/fr_FR/docs/04-snapshot-testing.md)

AVA wspiera testowanie snapshotów (migawek), [zgodnie z wprowadzeniem od Jest](https://facebook.github.io/jest/docs/snapshot-testing.html), poprzez interfejs [Asercji](./03-assertions.md). Możesz wykonać snapshot dowolnej wartości, a także elementów React:

```js
// Your component
const HelloWorld = () => <h1>Hello World...!</h1>;

export default HelloWorld;
```

```js
// Your test
const test = require('ava');
const render = require('react-test-renderer');
const HelloWorld = require('.');

test('HelloWorld component', t => {
	const tree = render.create(<HelloWorld/>).toJSON();
	t.snapshot(tree);
});
```

[Wypróbuj w tym przykładowym projekcie.](https://github.com/avajs/ava-snapshot-example)

Snapshoty są przechowywane obok plików testowych. Jeśli twoje testy są w `test` lub folderze `tests`, snapshoty będą przechowywane w folderze `snapshots`. Jeśli twoje testy są w folderze `__tests__`, wtedy będą przechowywane w folderze `__snapshots__`.

Powiedz, że masz `~/project/test/main.js` który zawiera snapshoty asercji. AVA utworzy dwa pliki:

* `~/project/test/snapshots/main.js.snap`
* `~/project/test/snapshots/main.js.md`

Pierwszy plik zawiera rzeczywistą migawkę i jest wymagany do przyszłych porównań. Drugi plik zawiera twój *raport migawki*. Jest regenerowany po zaktualizowaniu migawek. Jeśli przekażesz go do kontroli źródła, możesz skorzystać z diff, aby zobaczyć zmiany w migawce.

AVA pokaże, dlaczego asercja migawki nie powiodła się:

<img src="../media/snapshot-testing.png" width="1048">

Następnie możesz sprawdzić swój kod. Jeśli zmiana była zamierzona, możesz użyć `--update-snapshots` (lub `-u`) flagi, aby zaktualizować migawki:

```console
$ ava --update-snapshots
```

Możesz określić stałą lokalizację przechowywania plików migawek w AVA [konfiguracja `package.json`](./06-configuration.md):

**`package.json`:**

```json
{
	"ava": {
		"snapshotDir": "custom-directory"
	}
}
```

Pliki migawek zostaną zapisane w strukturze katalogów, która odzwierciedla strukturę plików testowych.

Jeśli używasz AVA dla wstępnie skompilowanych plików testowych, AVA spróbuje użyć map źródłowych, aby ustalić lokalizację oryginalnych plików. Migawki będą przechowywane obok tych plików, zgodnie z tymi samymi regułami, jak gdyby AVA wykonał bezpośrednio oryginalne pliki. Jest to świetne, jeśli piszesz testy w TypeScript (zobacz naszą [formułę TypeScript](./recipes/typescript.md)).
