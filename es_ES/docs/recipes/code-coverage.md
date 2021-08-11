___
**Nota del traductor**

Esta es la traducción del archivo [code-coverage.md](https://github.com/avajs/ava/blob/main/docs/recipes/code-coverage.md). Aquí hay un [enlace](https://github.com/avajs/ava/compare/4111f9483f2ff6a158b603735a712eee3ab074c4...main#diff-b3aa0c81a407f54f636a1cf5a619a4a6) a las diferencias con la rama master de AVA (Si al hacer clic en el enlace no se encuentran modificaciones en el archivo `code-coverage.md`, será por que la traducción está actualizada).
___
# Cobertura de código

Traducciones: [Español](https://github.com/avajs/ava-docs/blob/main/es_ES/docs/recipes/code-coverage.md), [Français](https://github.com/avajs/ava-docs/blob/main/fr_FR/docs/recipes/code-coverage.md), [Italiano](https://github.com/avajs/ava-docs/blob/main/it_IT/recipes/code-coverage.md), [日本語](https://github.com/avajs/ava-docs/blob/main/ja_JP/docs/recipes/code-coverage.md), [Portugués](https://github.com/avajs/ava-docs/blob/main/pt_BR/docs/recipes/code-coverage.md)

Como AVA [genera los archivos de test][isolated-env] no se puede usar ['istanbul'] para la cobertura de código, en cambio, se puede lograr esto con ['nyc'] que es básicamente ['istanbul'] con apoyo de sub-process.

## Configurar

Primero instalar NYC:

```
$ npm install nyc --save-dev
```

Luego añadir los directorios de '.nyc_output' y 'coverage' al archivo '.gitignore'.

`.gitignore`:

```
node_modules
coverage
.nyc_output
```

## Cobertura a ES5

Usando NYC para proveer cobertura de código en ES5 es simple. Anteponer a su script de test con 'nyc':

```json
{
	"scripts": {
		"test": "nyc ava"
	}
}
```

Eso es todo!

Si desea crear informes HTML de cobertura o cargar datos de cobertura en Coveralls, debe saltar a estas secciones a continuación.

## Cobertura ES2015

Usar Babel para transpilar nuestro código de producción es un poco más complicado. Aquí lo mostramos en varios pasos.

### Configurar Babel

Primero, necesitamos la configuración de Babel. Lo siguiente es solo un ejemplo, necesitarás modificarlo según tus necesidades.

`package.json`:
```json
{
	"babel": {
		"presets": ["es2015"],
		"plugins": ["transform-runtime"],
		"ignore": "test.js",
		"env": {
			"development": {
				"sourceMaps": "inline"
			}
		}
	}
}
```

Hay dos cosas importantes a tener en cuenta en el ejemplo anterior.

  1. Ignorar los archivos de test porque AVA ya maneja la transpilación de los tests por usted.

  2. Especificar 'inline' en sourceMaps para desarrollo. Esto es importante para generar correctamente la cobertura. Utilizando la sección 'env' de la configuración de Babel nos permite desactivar la opción de sourceMaps para construcciones de producción.


### Crear script de compilación

Ya que es poco probable que quiera la opción 'inline' en sourceMaps en el código de la producción, debe especificar una variable de entorno alternativo en los scripts de compilación:

`package.json`

```json
{
  "scripts": {
    "build": "BABEL_ENV=production babel --out-dir=dist index.js"
  }
}
```

> ATENCIÓN: `BABEL_ENV=production` no funciona en Windows, debe usar la palabra clave `set` (`set BABEL_ENV=production`). Para cruzar construcciones de plataforma, eche un vistazo a [`cross-env`].

Tenga en cuenta que el script anterior realmente tiene muy poco que ver con AVA y es sólo una demostración de cómo utilizar la configuración de 'env' de Babel para manipular su configuración, por lo que es compatible con AVA.

### Usar el 'require hook' de Babel

Para usar el 'require hook' de Babel, añada `babel-core/register` a la sección `require` de tu configuración de AVA en `package.json`.

```json
{
	"ava": {
		"require": ["babel-core/register"]
	}
}
```

*Nota*: También puede establecer el require hook desde la consola de comandos: `ava --require=babel-core/register`. No obstante, configurarlo en `package.json` le ahorrará el tener que escribir dicho comando varias veces.

### Juntándolo todo

Si combinamos todos los pasos anteriores, su 'package.json' debería quedar de la siguiente manera:

```json
{
	"scripts": {
		"test": "nyc ava",
		"build": "BABEL_ENV=production babel --out-dir=dist index.js"
	},
	"babel": {
		"presets": ["es2015"],
		"plugins": ["transform-runtime"],
		"ignore": "test.js",
		"env": {
			"development": {
				"sourceMaps": "inline"
			}
		}
	},
	"ava": {
		"require": ["babel-core/register"]
	}
}
```


## Reportes en HTML

NYC crea un archivo de cobertura 'json' para cada proceso bifurcado en el directorio '.nyc_ouput'.

Para combinarlos en un reporte HTML entendible, haga lo siguiente:

```
$ ./node_modules/.bin/nyc report --reporter=html
```

O use un script de npm para ahorrarse el escribrlo:

```json
{
	"scripts": {
		"report": "nyc report --reporter=html"
	}
}
```

Esto generará un archivo HTML en el directorio de 'cobertura'.


## Alojar la cobertura

### Travis CI & Coveralls

Primero, debe logarse en [coveralls.io] y activar su repositorio.

Una vez hecho esto, añada [`coveralls`] como una dependencia de desarrollo de npm:

```
$ npm install coveralls --save-dev
```

Entonces añada lo siguiente a su archivo de `.travis.yml`:

```yaml
after_success:
    - './node_modules/.bin/nyc report --reporter=text-lcov | ./node_modules/.bin/coveralls'
```

Su informe de cobertura aparecerá en coveralls poco después de que Travis termine.

[`babel`]:      https://github.com/babel/babel
[coveralls.io]: https://coveralls.io
[`coveralls`]:  https://github.com/nickmerwin/node-coveralls
[`cross-env`]:  https://github.com/kentcdodds/cross-env
[isolated-env]: https://github.com/avajs/ava-docs/blob/main/es_ES/readme.md#entorno-aislado
[`istanbul`]:   https://github.com/gotwarlost/istanbul
[`nyc`]:        https://github.com/bcoe/nyc
