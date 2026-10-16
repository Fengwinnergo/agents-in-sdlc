---
title: "Ejercicio 1: instrucciones personalizadas (agente en la nube)"
authors:
  - geektrainer
lastUpdated: 2026-06-30
---

[← Lección anterior: requisitos previos][previous-lesson] · [Lección siguiente: agente de GitHub Copilot en la nube →][next-lesson]

El contexto es fundamental al trabajar con IA generativa. Si una tarea debe hacerse de una forma concreta o Copilot necesita información de contexto, asegúrate de que pueda acceder a ella. Los [archivos de instrucciones][instruction-files] aportan esa orientación para que Copilot entienda no solo *qué* quieres, sino también *cómo* debe hacerlo.

En este ejercicio:

- explorarás cómo llegan a Copilot el contexto específico del proyecto, las directrices de programación y los estándares de documentación mediante las instrucciones personalizadas del repositorio y los archivos de instrucciones específicos de rutas.
- añadirás un estándar general al archivo `.github/copilot-instructions.md`.

> [!NOTE]
> A diferencia de los recorridos de VS Code y CLI, aquí no usarás un prompt para comparar un resultado *anterior* y *posterior*. El agente de Copilot en la nube trabaja de forma asíncrona en incidencias de GitHub, así que es más difícil mostrar su efecto en tiempo real. Lo verás más adelante en este recorrido, cuando revises las PR que genere.

## Archivos de instrucciones

### Escenario

Tailspin Toys tiene un conjunto de directrices y requisitos para las prácticas de desarrollo:

- La capa de datos siempre debe tener pruebas unitarias.
- La interfaz debe usar un tema oscuro y tener un aspecto moderno.
- El código debe incluir documentación en forma de comentarios TSDoc.
- Al principio de cada archivo debe haber un bloque de comentarios que describa su función.

Los archivos de instrucciones permitirán que Copilot disponga de la información necesaria para realizar las tareas según estas prácticas.

### Instrucciones personalizadas

Las instrucciones personalizadas proporcionan contexto y preferencias a Copilot para que pueda entender mejor tu estilo de programación y tus requisitos. Puedes indicar convenciones, bibliotecas e incluso el tipo de comentarios que quieres incluir. Puedes crear instrucciones para todo el repositorio o para tipos de archivo concretos.

Hay dos tipos de archivos de instrucciones:

- `.github/copilot-instructions.md`, un único archivo que se envía a Copilot en **todas** las solicitudes del repositorio. Debe contener información del proyecto relevante para la mayoría de solicitudes de chat o CLI, como la pila tecnológica, una descripción general de lo que se está creando y las buenas prácticas.
- Los archivos `.github/instructions/*.instructions.md`, que se pueden crear para tareas o tipos de archivo concretos, como TypeScript, componentes de interfaz o pruebas unitarias.

> [!NOTE]
> En el IDE, los archivos de instrucciones solo se usan para generar código en Copilot Chat, no para completar código ni para sugerencias de siguiente edición.
>
> Copilot Chat, Copilot CLI y el agente de Copilot en la nube usan las instrucciones del repositorio y los archivos `*.instructions.md` que tengan metadatos `applyTo`.
>
> Copilot también admite otros formatos de instrucciones, como AGENTS.md y CLAUDE.md; consulta la [referencia sobre compatibilidad con instrucciones personalizadas][custom-instructions-support].

### Buenas prácticas para administrar archivos de instrucciones

Hablar en detalle sobre la creación de archivos de instrucciones queda fuera del alcance del taller. Los ejemplos del proyecto muestran un enfoque representativo. En general:

- Mantén el archivo `copilot-instructions.md` centrado en la orientación general del proyecto: qué se está creando, la estructura y los estándares globales.
- Usa archivos `*.instructions.md` para instrucciones específicas de tipos de archivo o tareas, como las pruebas unitarias, componentes Astro o la capa de datos.
- Usa un lenguaje natural y claro. Explica la orientación y proporciona ejemplos del código esperado y del que debe evitarse.

No existe una única forma correcta de crear archivos de instrucciones, igual que no existe una única forma de usar la IA. Con la práctica descubrirás qué funciona mejor en cada proyecto y situación.

> [!TIP]
> Todos los proyectos que usan GitHub Copilot deberían tener un conjunto sólido de archivos de instrucciones. Al explorar los archivos del proyecto, verás instrucciones para muchas tareas, como las [actualizaciones de la interfaz][ui-instructions] y [Astro][astro-instructions].
>
> Copilot también puede ayudarte a generar archivos de instrucciones. Cada entorno ofrece distintas opciones, como **Configure Chat → Generate Agent Instructions** en VS Code o `/init` en Copilot CLI. Las lecciones del entorno que estés usando indicarán cuándo resulta pertinente.
>
> Si buscas plantillas o un punto de partida, explora [awesome-copilot][awesome-copilot], un repositorio con archivos de instrucciones, agentes personalizados y otros recursos.

[ui-instructions]: https://github.com/github-samples/tailspin-toys/blob/main/.github/instructions/ui.instructions.md
[astro-instructions]: https://github.com/github-samples/tailspin-toys/blob/main/.github/instructions/astro.instructions.md
[awesome-copilot]: https://github.com/github/awesome-copilot
[custom-instructions-support]: https://docs.github.com/copilot/reference/custom-instructions-support

## Explorar los archivos de instrucciones personalizadas del proyecto

Dedica un momento a leer los archivos de instrucciones incluidos en este repositorio: hay un `copilot-instructions.md` principal y varios archivos `*.instructions.md` para distintas tareas. Ábrelos en el editor o en la interfaz web de GitHub.

1. Abre `.github/copilot-instructions.md`.
2. Explora el archivo y fíjate en la breve descripción del proyecto y en secciones como **Agent notes**, **Code standards**, **Scripts** y **Repository Structure**. Se aplican a cualquier interacción con Copilot.
3. Abre la carpeta `.github/instructions` y explórala. Hay instrucciones para archivos Astro, la capa de datos de Drizzle, las pruebas y otros ámbitos.
4. Abre `.github/instructions/unit-tests.instructions.md`. Observa el campo `applyTo` de la parte superior, que define un patrón glob para determinar a qué archivos se aplican las instrucciones. En este caso, coincide con los archivos TypeScript de pruebas, como los que siguen el patrón `**/*.test.ts`.
5. Fíjate en las instrucciones específicas para crear pruebas unitarias.
6. Por último, abre `.github/instructions/drizzle.instructions.md` y desplázate hasta el final. Observa los enlaces a otros archivos de instrucciones, como `unit-tests.instructions.md`, y a archivos existentes del proyecto. Esto permite dividir conjuntos de instrucciones extensos en otros más pequeños y reutilizables, y enlazar ejemplos. Las rutas son relativas al archivo de instrucciones, no a la raíz del repositorio.

> [!NOTE]
> La sección **Code formatting requirements** de `copilot-instructions.md` documenta los estándares de programación, pero todavía no exige documentación en el código. En los pasos siguientes añadirás las reglas para los comentarios TSDoc y el encabezado de comentario de cada archivo.

## Añadir un estándar al repositorio

Ahora añadirás la única regla que editarás en este ejercicio: la documentación debe escribirse en el código mediante comentarios TSDoc y un encabezado de comentario para el archivo. El agente en la nube aplicará esta regla al trabajar en las incidencias que le asignes más adelante.

Antes de editar el archivo, prepara una rama de trabajo. El agente en la nube leerá las instrucciones de la rama de destino de la incidencia, pero de todas formas dejarás los cambios guardados de forma ordenada:

1. En el terminal de Codespaces, crea una rama y cámbiate a ella:

   ```bash
   git checkout -b custom-instructions-cloud
   ```

Como se explicó antes, `.github/copilot-instructions.md` proporciona información general del proyecto a Copilot. Añade estándares de programación para mejorar las sugerencias de código.

1. Vuelve a abrir `.github/copilot-instructions.md`.
2. Localiza la sección **Code formatting requirements**, que debería estar cerca de la línea 27. Observa que describe los estándares de programación, pero aún no exige documentación en el código.
3. Añade estas líneas Markdown justo debajo de los estándares existentes:

   ```markdown
   - Every exported function should have a TSDoc comment describing its purpose, parameters, and return value.
   - Before imports or any code, add a comment block to the file that explains its purpose.
   ```

4. Guarda `copilot-instructions.md`.

> [!TIP]
> Como viste en la lección anterior, puedes crear archivos de instrucciones en el nivel del repositorio (`.github/copilot-instructions.md`) para proporcionar orientación general, o usar archivos `*.instructions.md` para lenguajes, tipos de archivo o tareas concretos. El archivo de instrucciones del repositorio es el lugar adecuado para estándares generales, como la regla de comentarios TSDoc.

## Confirmar, enviar y combinar la actualización de instrucciones

El agente en la nube lee los archivos de instrucciones de la rama a la que se dirige la incidencia. En el siguiente ejercicio, al asignar una incidencia a Copilot, este creará una rama basada en `main`. Por eso, los cambios de instrucciones deben llegar a `main` para que el agente pueda usarlos.

1. Prepara y confirma los cambios:

   ```bash
   git add .github/copilot-instructions.md
   git commit -m "Add doc comment and file-header standards to copilot instructions"
   ```

2. Envía la rama:

   ```bash
   git push -u origin custom-instructions-cloud
   ```

3. En github.com, abre una solicitud de incorporación de cambios desde `custom-instructions-cloud` a `main` y combínala. El agente en la nube leerá estas instrucciones cuando trabaje en las incidencias que le asignes en el próximo ejercicio.

> [!TIP]
> Si prefieres trabajar directamente en `main` durante este taller, puedes omitir los pasos de la rama y confirmar los cambios directamente en `main`. La rama sirve para practicar el flujo de trabajo habitual de un proyecto real.

## Resumen y próximos pasos

Has explorado cómo Copilot obtiene contexto de los archivos de instrucciones de este proyecto y has añadido un estándar general al archivo `.github/copilot-instructions.md`. En las PR que genere el agente en la nube durante el resto del recorrido podrás comprobar si sigue ese estándar.

A continuación, [asignarás tu primera incidencia al agente de Copilot en la nube][next-lesson].

## Recursos

- [Archivos de instrucciones para personalizar GitHub Copilot][instruction-files]
- [Buenas prácticas para crear instrucciones personalizadas][instructions-best-practices]
- [Cinco consejos para escribir mejores instrucciones personalizadas para Copilot][copilot-instructions-five-tips]
- [Awesome Copilot: archivos de instrucciones y otros recursos][awesome-copilot]

[previous-lesson]: ../0-prerequisites/
[next-lesson]: ../2-cloud-agent/
[instruction-files]: https://docs.github.com/copilot/customizing-copilot/about-customizing-github-copilot-chat-responses
[instructions-best-practices]: https://docs.github.com/enterprise-cloud@latest/copilot/using-github-copilot/coding-agent/best-practices-for-using-copilot-to-work-on-tasks#adding-custom-instructions-to-your-repository
[copilot-instructions-five-tips]: https://github.blog/ai-and-ml/github-copilot/5-tips-for-writing-better-custom-instructions-for-copilot/
