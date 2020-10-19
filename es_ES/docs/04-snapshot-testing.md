# Pruebas usando snapshots

AVA permite realizar pruebas usando snapshots, [presentadas por Jest](https://facebook.github.io/jest/docs/snapshot-testing.html), usando su interface de [aserciones](./03-assertions.md). Puedes tomar un snapshot de cualquier valor, así como de los elementos de React:

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

[Prueba este proyecto de ejemplo.](https://github.com/avajs/ava-snapshot-example)

Los snapshots se almacenan junto con tus archivos de prueba. Si tus pruebas estan en una carpeta `test` o `tests` los snapshots seran guardados en una carpeta `snapshots`. Si sus pruebas están en una carpeta `__tests__`, entonces se almacenarán en una carpeta `__snapshots__`.

Digamos que tienes `~/project/test/main.js` que contiene aserciones sobre snapshots. AVA creará dos archivos:

* `~/project/test/snapshots/main.js.snap`
* `~/project/test/snapshots/main.js.md`

El primer archivo contiene el snapshot como tal y es necesario para futuras comparaciones. El segundo archivo contiene un *snapshot report*. Este se regenera cuando actualizan los snapshots. Si lo subes a tu versional de codigo fuente, puedes chequear las diferencias para ver los cambios en tu snapshot.

AVA te mostrará por qué falló la verificacion de tu snapshot:

<img src="../media/snapshot-testing.png" width="1048">

Entonces puedes verificar tu código. Si el cambio fue intencional, puede usar la bandera `--update-snapshots` (o` -u`) para actualizar los snapshots:

```console
$ ava --update-snapshots
```

Puede especificar una ubicación fija para almacenar los snapshot en la [configuración `package.json`] de AVA (./06-configuration.md):

**`package.json`:**

```json
{
	"ava": {
		"snapshotDir": "custom-directory"
	}
}
```

Los archivos de snapshots se guardarán en una estructura de directorio que refleja la de tus archivos de prueba.

Si estás ejecutando AVA contra archivos de prueba precompilados, AVA intentará mapeará el origen para determinar la ubicación de los archivos originales. Los snapshots se almacenarán junto a estos archivos, siguiendo las mismas reglas como si AVA hubiera ejecutado los archivos originales directamente. 
Esto es genial si estás escribiendo pruebas en TypeScript (mira nuestra [receta de TypeScript] (./recipes/typescript.md)).
