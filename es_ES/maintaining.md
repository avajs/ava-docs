___
**Nota del traductor**

Esta es la traducción del archivo [maintaining.md](https://github.com/sindresorhus/ava/blob/master/maintaining.md). Aquí hay un [enlace](https://github.com/sindresorhus/ava/compare/0f0cbe9b0d5a71609a8f5a7afcb412f2bf41ad91...master#diff-af20adbc8ab4842b04d1f5c7df6f563a) a las diferencias con la rama master de AVA (Si al hacer clic en el enlace no se encuentran modificaciones en el archivo `maintaining.md`, será por que la traducción está actualizada).
___
# Mantenimiento


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
