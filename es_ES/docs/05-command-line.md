# CLI

```console
ava [<pattern>...]
ava debug [<pattern>...]
ava reset-cache

Comandos:
  ava [<pattern>...]        Ejecuta los tests                                  [default]
  ava debug [<pattern>...]  Activa el Node.js inspector and ejecuta un unico archivo de pruebas
  ava reset-cache           Reinicia la cache de compilacion de AVA y termina su ejecucion

Posicionales:
  pattern  patrones globales para seleccionar que archivos de prueba ejecutar. Dejalo en blanco si quieres ejecutar todos los archivos. 
    Agrega dos punto : y especifica numeros de linea de pruebas especificas a ejecutar                            [string]

Options:
  --version               Muestra el numero de version                          [boolean]
  --color                 Forza que la salida use colores                           [boolean]
  --config                Especifica un archivo JavaScript desde el cual se leera la configuracion, 
                            en lugar de usar los archivos package.json o ava.config.*
  --help                  Muestra la ayuda                                    [boolean]
  --concurrency, -c       Maximo numero de archivos de prueba ejecutandose al mismo tiempo
                          (default: CPU cores)                          [number]
  --fail-fast             Detener luego de que la primera prueba falle                [boolean]
  --match, -m             Solo ejecutar pruebas que hagan match on el titulo (pueden ser repetidas)
                                                                        [string]
  --node-arguments        Argumentos adicionales para ejecutar los worker processes de Node.js 
                            (especificar como una unica cadena)        [string]
  --serial, -s            Ejecutar pruebas en modo serial                           [boolean]
  --tap, -t               generar salida en modo TAP                          [boolean]
  --timeout, -T           Asigna un timeout global (milisegundos o formato legible por humanos,
                          ej. 10s, 2m)                                 [string]
  --update-snapshots, -u  Actualiza snapshots                             [boolean]
  --verbose, -v           Habilita output en modo verbose                        [boolean]
  --watch, -w             Re-ejecutar pruebas cuando los archivos cambian               [boolean]

Ejemplos:
  ava
  ava test.js
  ava test.js:4,7-9
```

*Ten en cuenta que la CLI utilizará tu instalación local de AVA cuando esté disponible, incluso cuando se ejecute globalmente.*

AVA busca archivos de prueba utilizando los siguientes patrones:

* `test.js`
* `src/test.js`
* `source/test.js`
* `**/test-*.js`
* `**/*.spec.js`
* `**/*.test.js`
* `**/test/**/*.js`
* `**/tests/**/*.js`
* `**/__tests__/**/*.js`

Los archivos dentro de `node_modules` son *siempre* ignorados. También lo son los archivos que comienzan con `_` o estan dentro de directorios que comienzan con un solo `_`. Además, los archivos que coinciden con estos patrones se ignoran de forma predeterminada, a menos que se configuren patrones diferentes:

* `**/__tests__/**/__helper__/**/*`
* `**/__tests__/**/__helpers__/**/*`
* `**/__tests__/**/__fixture__/**/*`
* `**/__tests__/**/__fixtures__/**/*`
* `**/test/**/helper/**/*`
* `**/test/**/helpers/**/*`
* `**/test/**/fixture/**/*`
* `**/test/**/fixtures/**/*`
* `**/tests/**/helper/**/*`
* `**/tests/**/helpers/**/*`
* `**/tests/**/fixture/**/*`
* `**/tests/**/fixtures/**/*`

Cuando usas `npm test`, puedes pasar argumentos posicionales directamente` npm test test2.js`, pero las banderas deben pasarse como `npm test - --verbose`.

## Ejecución de pruebas con títulos que tienen coincidencias

La bandera `--match` permite ejecutar solo las pruebas que tienen un título con coincidencias. Esto se logra con patrones simples de comodines (wildcards). Los patrones no distinguen entre mayúsculas y minúsculas. Consulte [`matcher`] (https://github.com/sindresorhus/matcher) para obtener más detalles.

Coincidir con títulos que terminan con `foo`:

```console
npx ava --match='*foo'
```

Coincidir con títulos que empiezan con `foo`:

```console
npx ava --match='foo*'
```

Coincidir con títulos que contienen `foo`:

```console
npx ava --match='*foo*'
```

Coincidir con títulos que son *exactamente* `foo` (aunque sin distinción entre mayúsculas y minúsculas):

```console
npx ava --match='foo'
```

Coincidir con títulos que no contienen `foo`:

```console
npx ava --match='!*foo*'
```

Coincidir con títulos que comienzan con `foo` y terminan con `bar`:

```console
npx ava --match='foo*bar'
```

Coincidir con títulos que comienzan con `foo` o terminan con `bar`:

```console
npx ava --match='foo*' --match='*bar'
```

Ten en cuenta que un patrón de coincidencias tiene prioridad sobre el modificador `.only`. Solo se comparan las pruebas con un título explícito. Las pruebas sin títulos o cuyo título se deriva de la función de implementación se omitirán cuando se usa `--match`.

Esto es lo que sucede cuando se ejecuta AVA con un patrón de coincidencia de `*oo*` y las siguientes pruebas:

```js
test('foo se ejecutara', t => {
	t.pass();
});

test('moo tambien se ejecutara', t => {
	t.pass();
});

test.only('boo tambien se ejecutara pero no exclusivamente', t => {
	t.pass();
});

// No se ejecutará, sin título
test(function (t) {
	t.fail();
});

// No se ejecutará, sin título explícito
test(function foo(t) {
	t.fail();
});
```

## Ejecutar pruebas usando números de línea específicos

AVA permite ejecutar pruebas exclusivamente haciendo referencia a sus números de línea. Apuntando a una sola línea, un rango de líneas o ambas. Puedes seleccionar cualquier número de línea de una prueba.

El formato es una lista separada por comas de "[X | Y-Z]" donde "X", "Y" y "Z" son números enteros entre "1" y el último número de línea del archivo.

Esta función solo está disponible desde la línea de comandos. No funcionará si usa herramientas como `ts-node / register` o` @ babel / register`, y actualmente no funciona con `@ ava / babel` y` @ ava / typescript`.

### Ejecutar una unica prueba

Para ejecutar solo una prueba en particular en un archivo, agregue el número de línea de la prueba a la ruta o patrón pasado a AVA.

Dado el siguiente archivo de prueba:

`test.js`

```js
1: test('unicorn', t => {
2:   t.pass();
3: });
4:
5: test('rainbow', t => {
6:  t.fail();
7: });
```

Ejecutar `npx ava test.js:2` ejecutaría la prueba `unicorn`. De hecho, podría utilizar cualquier número de línea entre "1" y "3".

### Ejecutando múltiples pruebas

Para ejecutar varias pruebas, debes especificarlas una por una o para que seleccione un rango de números de línea. Como los números de línea se dan por archivo, puedes ejecutar varios archivos con diferentes números de línea para cada archivo. Si se proporciona el mismo archivo varias veces, los números de línea se combinan y solo se ejecutan una vez.

### Ejemplos

Numeros de linea unicos

```console
npx ava test.js:2,9
```

Rangos:

```console
npx ava test.js:4-7
```

Mezcla de rangos y número de línea única:

```console
npx ava test.js:4,9-12
```

Diferentes archivos:

```console
npx ava test.js:3 test2.js:4,7-9
```

Cuando se ejecuta un archivo con y sin números de línea, los números de línea tienen prioridad.

## Restableciendo la caché de AVA

AVA suele almacenar en caché ciertos archivos, especialmente cuando usa [`@ava/babel`] (https://github.com/avajs/babel). 
Si parece que los últimos cambios no están siendo recogidos por AVA, puedes restablecer la caché ejecutando:
```console
npx ava reset-cache
```

Esto borra todos los archivos del directorio `node_modules/.cache/ava`.

## Reportes

Por defecto, AVA usa un reporte mínimo:

<img src="../media/mini-reporter.gif" width="460">

Utilice la bandera `--verbose` para habilitar el reporte detallado. 
Esto siempre se usa en entornos de CI a menos que el [TAP del reporte] (#tap-reporter) esté habilitado.

<img src="../media/verbose-reporter.png" width="294">

### TAP reportes

AVA soporta formato TAP, por lo cual es compatible con cualquier [TAP reporter](https://github.com/sindresorhus/awesome-tap#reporters). Usa el flag `--tap` para habilitar una salida en modo TAP.

```console
$ npx ava --tap | npx tap-nyan
```

<img src="../media/tap-reporter.png" width="420">

Por favor, ten en cuenta que TAP reporter no esta disponible cuando usas el modo [watch](./recipes/watch-mode.md).

## Node arguments

El argumento `--node-arguments` puede usarse para especificar argumentos adicionales para iniciar procesos de trabajo. 
Estos se combinan con la configuración de `nodeArguments` y cualquier argumento pasado al binario de`node` al iniciar AVA.

** Solo transfiera valores confiables. **

Especifique los argumentos como una sola cadena:

```console
npx ava --node-arguments="--throw-deprecation --zero-fill-buffers"
```

** Solo transfiera valores confiables. **
