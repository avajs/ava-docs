___
**Nota del traductor**

Esta es la traducción del archivo [maintaining.md](https://github.com/sindresorhus/ava/blob/master/maintaining.md). Aquí hay un [enlace](https://github.com/sindresorhus/ava/compare/0316047c8df66202e308ce095eb2f335f4aac1c1...master#diff-af20adbc8ab4842b04d1f5c7df6f563a) a las diferencias con la rama master de AVA (Si al hacer clic en el enlace no se encuentran modificaciones en el archivo `maintaining.md`, será por que la traducción está actualizada).
___
# Mantenimiento [![Dependency Status](https://david-dm.org/sindresorhus/ava.svg)](https://david-dm.org/sindresorhus/ava) [![devDependency Status](https://david-dm.org/sindresorhus/ava/dev-status.svg)](https://david-dm.org/sindresorhus/ava#info=devDependencies)


## Pruebas

 - `npm test`: Corrige errores de código y ejecuta todo el conjunto de tests con cobertura.
 - `npm run test-win`: Ejecuta los tests en Windows.
 - `npm run coverage`: Genera un informe de la cobertura del último test generado (abre una ventana del navegador).
 - `tap test/fork.js --bail`:  Ejecuta un archivo de tests específico y se detiene en el primer error (útil cuando busquemos errores).


## Proceso de Lanzamiento

- Enderezar dependencias.
- Asegurar que [Travis CI](https://travis-ci.org/sindresorhus/ava) y [AppVeyor](https://ci.appveyor.com/project/sindresorhus/ava/branch/master) son verdes.
- Publicar una nueva versión utilizando [`np`](https://github.com/sindresorhus/np) con un número de versión según [semver](http://semver.org).
- Escribir una [release note](https://github.com/sindresorhus/ava/releases/new) siguiendo el estilo de las notas de la versión anterior.


## Pull requests

- Las nuevas características deben venir con tests y documentación.
- Asegúrese de que sigue las [directrices de contribución](contributing.md).
- Al menos dos miembros del equipo deben `LGTM` un pull request antes de que se fusione.
- Haz squash de tus commits cuando fusiones.

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
