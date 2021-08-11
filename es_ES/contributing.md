___
**Nota del traductor**

Esta es la traducción del archivo [contributing.md](https://github.com/avajs/ava/blob/main/.github/CONTRIBUTING.md). 

Aquí hay un [enlace](https://github.com/avajs/ava/compare/4111f9483f2ff6a158b603735a712eee3ab074c4...master#diff-cc4aac3e9be04e0413c9520f223b493c) a las diferencias con la rama master de AVA (Si al hacer clic en el enlace no se encuentran modificaciones en el archivo `contributing.md`, será por que la traducción está actualizada).
___
# Contribuir a AVA

✨ Gracias por contribuir a AVA! ✨

Tenga en cuenta que este proyecto se ha lanzado con un [Código de conducta](code-of-conduct.md) para el contribuyente. Al participar en este proyecto usted acepta acatar sus términos.

Traducciones: [Español](https://github.com/avajs/ava-docs/blob/master/es_ES/contributing.md), [Français](https://github.com/avajs/ava-docs/blob/master/fr_FR/contributing.md), [Italiano](https://github.com/avajs/ava-docs/blob/master/it_IT/contributing.md), [日本語](https://github.com/avajs/ava-docs/blob/master/ja_JP/contributing.md), [Portugués](https://github.com/avajs/ava-docs/blob/master/pt_BR/contributing.md), [Русский](https://github.com/avajs/ava-docs/blob/master/ru_RU/contributing.md), [简体中文](https://github.com/avajs/ava-docs/blob/master/zh_CN/contributing.md)

## ¿Cómo puedo contribuir?

### Mejorar la documentación

Como usuario de AVA, usted es el candidato perfecto para ayudarnos a mejorar nuestra documentación. Correcciones tipográficas, correcciones de errores, mejores explicaciones, más ejemplos, etc. Cuestiones abiertas para las cosas que se podrían mejorar. [Ayudanos a traducir nuestros documentos.](https://github.com/avajs/ava-docs) Cualquier cosa. Incluso las mejoras a este documento.

Usa la etiqueta [`docs`](https://github.com/avajs/ava/labels/docs) para encontrar sugerencias sobre temas en los que nos encantaría tener más documentación.

### Mejorar problemas

Algunos problemas se crean con falta de información, no son reproducibles, o simplemente no válidos. Ayude a hacer que sean más fáciles de resolver. Gastamos un montón de tiempo en manejar problemas que podríamos invertir en corregir errores y añadiendo características.

### Sugerencias y comentarios sobre problemas

Siempre estamos buscando más opiniones sobre los debates en el seguimiento de incidencias. Es una buena oportunidad de influir en la futura dirección de AVA.

La etiqueta [`question`](https://github.com/avajs/ava/labels/question) es un buen punto de partida para encontrar discusiones abiertas.

### Escribiendo código

Puedes usar las etiquetas para descubrir las incidencias con las que puedes colaborar:

* [`babel` issues](https://github.com/avajs/ava/labels/babel) relacionado con nuestra infraestructura Babel
* [`blocked` issues](https://github.com/avajs/ava/labels/blocked) necesitan ayuda para destrancarse
* [`bug` issues](https://github.com/avajs/ava/labels/bug) son errores conocidos que nos gustaría arreglar
* [`enhancement` issues](https://github.com/avajs/ava/labels/enhancement) son funciones que estamos abiertos a incluir
* [`performance` issues](https://github.com/avajs/ava/labels/performance) monitorean ideas sobre cómo mejorar el desempeño de AVA

Las etiquetas de [`help wanted`](https://github.com/avajs/ava/labels/help%20wanted) y [`good for beginner`](https://github.com/avajs/ava/labels/good%20for%20beginner) son especialmente útiles.

Puedes encontrar una incidencia ya asignada, o con la etiqueta [`assigned`](https://github.com/avajs/ava/labels/assigned). Por favor chequear dos veces antes de comenzar a trabajar en esta incidencia porque es probable que ya haya alguien más trabajando en ello.

Nos gustaría corregir primero las inidencias con la etiqueta [`priority`](https://github.com/avajs/ava/labels/priority). Nos encantaría ver progreso en las de [`low-priority`](https://github.com/avajs/ava/labels/low%20priority) también. [`future` issues](https://github.com/avajs/ava/labels/future) son aquéllas a las que nos gustaría llegar, pero no prontamente. Por favor chequear antes de trabajar en estas porque quizás aún no querramos tomarnos el trabajo de brindarle soporte a estas funcionalidades.

### Pasa el rato en nuestro chat

Tenemos un [chat](https://gitter.im/avajs/ava). Visitanos y estate alerta, habla con nosotros y ayuda a otros.

### Notificando un problema

- El seguimiento de incidencias es para incidencias. Use nuestro [chat](https://gitter.im/avajs/ava) o [Stack Overflow](https://stackoverflow.com/questions/tagged/ava) para soporte.
- Buscar en el seguimiento de incidencias antes de abrir un tema.
- Asegúrese de que está utilizando la última versión de AVA.
- Utilice un título claro y descriptivo.
- Incluya tanta información como sea posible: Pasos para reproducir el problema, el mensaje de error, la versión de Node.js, sistema operativo, etc.
- Cuanto más tiempo le dedique usted a un problema, más le dedicaremos nosotros.
- [El mejor reporte de un problema es un test demostrando el fallo.](https://twitter.com/sindresorhus/status/579306280495357953)

### Solicitando un pull request

- Cambios no triviales es a menudo mejor si se discuten en un problema (issue) primero, para que no hagáis trabajo innecesario.
- Para las tareas ambiciosas, usted debe mostrar su trabajo delante de la comunidad para la retroalimentación tan pronto como sea posible. Abra un pull request tan pronto como usted haya hecho lo mínimo necesario para demostrar su idea. En esta primera etapa, no se preocupe de hacer las cosas perfectas, o completas al 100%. Añada un prefijo [WIP] para el título, y describa lo que todavía le falta hacer. Esto permite que los colaboradores sepan que no deben fijarse en los pequeños detalles o señalar las mejoras que usted ya sabe que debe hacer.
- Las nuevas características se deben acompañar con tests y documentación.
- No incluya los cambios no relacionados.
- Corrija errores de código y haga los tests antes de solicitar el pull request mediante la ejecución `$ npm test`.
- Haga el pull request de una [rama con el asunto](https://github.com/dchelimsky/rspec/wiki/Topic-Branches), no master.
- Utilice un título claro y descriptivo para la solicitud de pull request y commits.
- Escriba una descripción convincente de por qué debemos incluir su pull request. Es su trabajo el convencernos. Responder a "por qué" es necesario y proporcionar casos de uso.
- Es posible que se le pida que haga cambios en su pull request. No es necesario solicitar otro pull request. [Únicamente actualice el que tiene.](https://github.com/RichardLitt/knowledge/blob/master/github/amending-a-commit-guide.md)

Nota: mientras realices cambios de código, trata de recordar el mantra de AVA (robado a Python) de tener preferentemente una manera de hacer algo. Por ejemplo, una petición para agregar un alias a una parte de la API ([como aquí](https://github.com/avajs/ava/pull/663)) va a ser probablemente rechazada si no tiene algún otro beneficio substancial.

*¿Buscas realizar tu primera contribución a un proyecto de código abierto? ¡No busques más! AVA posiblemente sea uno de los proyectos y comunidades más acogedores que hay. ¡Revisa el artículo ["Making your first contribution"](https://medium.com/@vadimdemedes/making-your-first-contribution-de6576ddb190), que te guiará para empezar de la forma correcta y hacer que tu contribución sea parte de AVA!*
