___
**Nota del traductor**

Esta es la traducción del archivo [maintaining.md](https://github.com/avajs/ava/blob/master/maintaining.md). Aquí hay un [enlace](https://github.com/avajs/ava/compare/691fdb6f4d6b3ae6b39be449a8577353978866b8...master#diff-af20adbc8ab4842b04d1f5c7df6f563a) a las diferencias con la rama master de AVA (Si al hacer clic en el enlace no se encuentran modificaciones en el archivo `maintaining.md`, será por que la traducción está actualizada).
___
# Mantenimiento [![Dependency Status](https://david-dm.org/avajs/ava.svg)](https://david-dm.org/avajs/ava) [![devDependency Status](https://david-dm.org/avajs/ava/dev-status.svg)](https://david-dm.org/avajs/ava#info=devDependencies)

## Conducta

**Ser amable con todo el mundo.**
Lea y siga el [código de conducta](code-of-conduct.md).


## Pruebas

 - `npm test`: Corrige errores de código y ejecuta todo el conjunto de tests con cobertura.
 - `npm run test-win`: Ejecuta los tests en Windows.
 - `npm run coverage`: Genera un informe de la cobertura del último test generado (abre una ventana del navegador).
 - `tap test/fork.js --bail`:  Ejecuta un archivo de tests específico y se detiene en el primer error (útil cuando busquemos errores).


## Proceso de Lanzamiento

- Enderezar dependencias.
- Asegurar que [Travis CI](https://travis-ci.org/avajs/ava) y [AppVeyor](https://ci.appveyor.com/project/avajs/ava/branch/master) son verdes.
- Publicar una nueva versión utilizando [`np`](https://github.com/sindresorhus/np) con un número de versión según [semver](http://semver.org).
- Escribir una [release note](https://github.com/avajs/ava/releases/new) siguiendo el estilo de las notas de la versión anterior.


## Pull requests

- Las nuevas características deben venir con tests y documentación.
- Asegúrese de que sigue las [directrices de contribución](contributing.md).
- Al menos un miembro del equipo deben `LGTM` un pull request antes de que se fusione.
- Haz squash de tus commits cuando fusiones. *[Example](https://github.com/avajs/ava/commit/0675d3444da6958b54c7e5eada91034e516bc97c)*

## Perfilado

Debe instalar primero
 [`iron-node`](https://github.com/s-a/iron-node) and / or [`devtool`](https://github.com/Jam3/devtool) globally:

```
$ npm install --global iron-node devtool
```

En la raíz de un proyecto con AVA, ejecute:

```
$ iron-node node_modules/ava/profile.js <test-file>
```

O:

```
$ devtool node_modules/ava/profile.js <test-file>
```
Una vez ha cargado la ventana de herramientas de Desarrollo, active los perfiles de memoria o CPU y luego ejecute <kbd>Cmd</kbd><kbd>R</kbd> para ejecutar los tests.

En cuanto terminen los tests detenga la grabación y revise los resultados del generador de perfiles. El flamegraph puede visualizarse seleccionando 'Chart' en el desplegable de la pestaña de 'Perfiles' (otros puntos de vista son 'Tree (arriba abajo)' y 'Heavy (ascendente)').

También puede desproteger la página de configuración en herramientas de desarrollo y permitir una o más opciones en la sección de perfiles.

##### Recursos de utilidad

 - [Una introducción a Node.js depuración con 'devtool'](http://mattdesl.svbtle.com/debugging-nodejs-in-chrome-devtools).
 - [Un video introductorio a las Chrome DevTools CPU y Memory profiling](https://www.youtube.com/watch?v=KKwmdTByxLk).

 ## Evaluación comparativa

Primero Recopilamos datos de referencia para una branch/commit:

```
$ node bench/run
```

Una vez que han recogido datos de dos/tres branches/commits:

```
$ node bench/compare
```

*Por ejemplo podría recopilar datos de referencia del árbol de trabajo y el último commit.*

![](https://cloud.githubusercontent.com/assets/4082216/12700805/bf18f730-c7bf-11e5-8a4f-fec0993c053f.png)

Ahora puede lanzar un subconjunto de la suite:

```
$ node bench/run.js concurrent/sync.js serial/sync.js -- concurrent/sync.js -- serial/sync.js
```

Note el separador '--'. Lo anterior sería lo mismo que tres de los siguientes comandos de benchmarking.

```
$ ava concurrent/sync.js serial/sync.js
$ ava concurrent/sync.js
$ ava serial/sync.js
```

También si esta haciendo una evaluación comparativa de una suite que puede fallar, debe añadir el flag `--should-fail` en este grupo:

```
$ node bench/run.js concurrent/sync.js -- --should-fail other/failures.js
```

Lo anterior pone de referencia dos comandos, pero espera que el segundo falle.

## Inducción de nuevos miembros

- Agregar el usuario al 'readme.md' y 'package.json'.
- Add the user as a collaborator to all AVA related repos and npm packages.
- Agregar el usuario como colaborador a todos los repositorios relacionado a AVA y paquetes npm.
- Share the Twitter account login info and that he user is free to tweet/retweet relevant stuff.
- Compartir la información de inicio de sesión de cuenta de Twitter y así el usuario será libre de twittear y retwittear cosas relevantes. 
