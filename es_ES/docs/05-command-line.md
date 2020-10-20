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

Cuando usas `npm test`, puedes pasar argumentos posicionales directamente` npm test test2.js`, pero los banderas deben pasarse como `npm test - --verbose`.

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

Esto es lo que sucede cuando ejecuta AVA con un patrón de coincidencia de `*oo*` y las siguientes pruebas:

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

## Running tests at specific line numbers

AVA lets you run tests exclusively by referring to their line numbers. Target a single line, a range of lines or both. You can select any line number of a test.

The format is a comma-separated list of `[X|Y-Z]` where `X`, `Y` and `Z` are integers between `1` and the last line number of the file.

This feature is only available from the command line. It won't work if you use tools like `ts-node/register` or `@babel/register`, and it does not currently work with `@ava/babel` and `@ava/typescript`.

### Running a single test

To only run a particular test in a file, append the line number of the test to the path or pattern passed to AVA.

Given the following test file:

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

Running `npx ava test.js:2` for would run the `unicorn` test. In fact you could use any line number between `1` and `3`.

### Running multiple tests

To run multiple tests, either target them one by one or select a range of line numbers. As line numbers are given per file, you can run multiple files with different line numbers for each file. If the same file is provided multiple times, line numbers are merged and only run once.

### Examples

Single line numbers:

```console
npx ava test.js:2,9
```

Range:

```console
npx ava test.js:4-7
```

Mix of single line number and range:

```console
npx ava test.js:4,9-12
```

Different files:

```console
npx ava test.js:3 test2.js:4,7-9
```

When running a file with and without line numbers, line numbers take precedence.

## Resetting AVA's cache

AVA may cache certain files, especially when you use our [`@ava/babel`](https://github.com/avajs/babel) provider. If it seems like your latest changes aren't being picked up by AVA you can reset the cache by running:

```console
npx ava reset-cache
```

This deletes all files in the `node_modules/.cache/ava` directory.

## Reporters

By default AVA uses a minimal reporter:

<img src="../media/mini-reporter.gif" width="460">

Use the `--verbose` flag to enable the verbose reporter. This is always used in CI environments unless the [TAP reporter](#tap-reporter) is enabled.

<img src="../media/verbose-reporter.png" width="294">

### TAP reporter

AVA supports the TAP format and thus is compatible with [any TAP reporter](https://github.com/sindresorhus/awesome-tap#reporters). Use the `--tap` flag to enable TAP output.

```console
$ npx ava --tap | npx tap-nyan
```

<img src="../media/tap-reporter.png" width="420">

Please note that the TAP reporter is unavailable when using [watch mode](./recipes/watch-mode.md).

## Node arguments

The `--node-arguments` argument may be used to specify additional arguments for launching worker processes. These are combined with the `nodeArguments` configuration and any arguments passed to the `node` binary when starting AVA.

**Only pass trusted values.**

Specify the arguments as a single string:

```console
npx ava --node-arguments="--throw-deprecation --zero-fill-buffers"
```

**Only pass trusted values.**
