---
title: "Ejercicio 1: instrucciones personalizadas (VS Code)"
authors:
  - geektrainer
lastUpdated: 2026-06-30
---

[← Lección anterior: requisitos previos][previous-lesson] · [Lección siguiente: modo de agente →][next-lesson]

El contexto es fundamental al trabajar con IA generativa. Si una tarea debe hacerse de una forma concreta o Copilot necesita información de contexto, asegúrate de que pueda acceder a ella. Hay varias formas de compartir contexto específico con Copilot. Una de las principales son los [archivos de instrucciones][instruction-files], que permiten orientar la generación de código.

En este ejercicio:

- explorarás cómo llegan a Copilot el contexto específico del proyecto, las directrices de programación y los estándares de documentación mediante las instrucciones personalizadas del repositorio y los archivos de instrucciones específicos de rutas.
- generarás la primera parte de los datos para el filtrado (una función auxiliar de acceso a datos de editoriales) con las instrucciones *actuales*.
- añadirás un estándar general al archivo `.github/copilot-instructions.md`.
- volverás a enviar el mismo prompt y observarás cómo el código generado adopta el nuevo estándar.
- confirmarás la actualización de las instrucciones y la parte de filtrado en `main` para que Copilot pueda usar las nuevas directrices en el siguiente ejercicio.

> [!CAUTION]
> El código generado puede no cumplir alguno de los estándares que hayas definido. Copilot es probabilístico. El objetivo de este ejercicio es observar la *tendencia* del comportamiento después de actualizar las instrucciones, no obtener un resultado idéntico carácter por carácter.

## Archivos de instrucciones

### Escenario

Tailspin Toys sigue una serie de directrices y requisitos para las prácticas de desarrollo:

- La capa de datos siempre debe tener pruebas unitarias.
- La interfaz debe usar un tema oscuro y tener un aspecto moderno.
- El código debe incluir documentación en forma de comentarios TSDoc.
- Al principio de cada archivo debe haber un bloque de comentarios que describa su función.

Usarás archivos de instrucciones para asegurarte de que Copilot disponga de la información necesaria para realizar las tareas de acuerdo con estas prácticas.

### Instrucciones personalizadas

Las instrucciones personalizadas proporcionan contexto y preferencias a Copilot para que pueda entender mejor tu estilo de programación y tus requisitos. Puedes indicar las convenciones, bibliotecas e incluso el tipo de comentarios que quieres incluir. Puedes crear instrucciones para todo el repositorio o para tipos de archivo concretos.

Hay dos tipos de archivos de instrucciones:

- `.github/copilot-instructions.md`, un único archivo que se envía a Copilot en **todas** las solicitudes del repositorio. Debe contener información general del proyecto relevante para la mayoría de las solicitudes: por ejemplo, la pila tecnológica, una descripción de lo que se está creando, buenas prácticas y orientación global.
- Los archivos `.github/instructions/*.instructions.md`, que proporcionan directrices para lenguajes, tipos de archivo o tareas concretos, como TypeScript, componentes de interfaz o pruebas unitarias.

> [!NOTE]
> En el IDE, los archivos de instrucciones solo se usan para generar código en Copilot Chat, no para completar código ni para sugerencias de siguiente edición.
>
> Copilot Chat, Copilot CLI y el agente de Copilot en la nube usan las instrucciones del repositorio y los archivos `*.instructions.md` que tengan metadatos `applyTo`.
>
> Copilot también admite otros formatos de instrucciones, como AGENTS.md y CLAUDE.md; consulta la [referencia sobre compatibilidad con instrucciones personalizadas][custom-instructions-support].

### Buenas prácticas para administrar archivos de instrucciones

Hablar en detalle sobre la creación de archivos de instrucciones queda fuera del alcance del taller. Los ejemplos del proyecto muestran un enfoque representativo. En general:

- Mantén el archivo `copilot-instructions.md` centrado en la orientación general del proyecto: qué se está creando, su estructura y los estándares globales de programación.
- Usa archivos `*.instructions.md` para directrices específicas de tipos de archivo o tareas, como pruebas unitarias, componentes Astro o la capa de datos.
- Usa lenguaje natural y claro. Explica la orientación y proporciona ejemplos del código que debe seguirse y del que debe evitarse.

No hay una única forma de crear archivos de instrucciones, igual que no hay una única forma de usar la IA. Con la práctica descubrirás qué funciona mejor para cada proyecto y situación.

> [!TIP]
> Todos los proyectos que usan GitHub Copilot deberían tener un conjunto sólido de archivos de instrucciones. Al explorar los archivos de este proyecto, verás instrucciones para muchas tareas, como [actualizaciones de la interfaz][ui-instructions] y [Astro][astro-instructions].
>
> Copilot también puede ayudarte a generar archivos de instrucciones. Cada entorno ofrece distintas opciones, como **Configure Chat → Generate Agent Instructions** en VS Code o `/init` en Copilot CLI. Las lecciones del entorno que estés usando indicarán cuándo resulta pertinente.
>
> Si buscas plantillas o un punto de partida, explora [awesome-copilot][awesome-copilot], un repositorio con archivos de instrucciones, agentes personalizados y otros recursos.

[ui-instructions]: https://github.com/github-samples/tailspin-toys/blob/main/.github/instructions/ui.instructions.md
[astro-instructions]: https://github.com/github-samples/tailspin-toys/blob/main/.github/instructions/astro.instructions.md
[awesome-copilot]: https://github.com/github/awesome-copilot
[custom-instructions-support]: https://docs.github.com/copilot/reference/custom-instructions-support

## Explorar los archivos de instrucciones personalizadas del proyecto

Dedica un momento a leer los archivos de instrucciones incluidos en el repositorio: hay un `copilot-instructions.md` principal y varios archivos `*.instructions.md` para distintas tareas. Ábrelos en el editor o en la interfaz web de GitHub.

1. Abre `.github/copilot-instructions.md`.
2. Explora el archivo y fíjate en la descripción breve del proyecto y en secciones como **Agent notes**, **Code standards**, **Scripts** y **Repository Structure**. En **Code standards**, observa las directrices anidadas de **GitHub Actions Workflows**. Se aplican a todas tus interacciones con Copilot.
3. Abre la carpeta `.github/instructions` y explórala. Hay instrucciones para archivos Astro, la capa de datos de Drizzle, las pruebas y otros ámbitos.
4. Abre `.github/instructions/unit-tests.instructions.md`. Observa el campo `applyTo` de la parte superior, que define un patrón glob relativo a la raíz del repositorio y determina a qué archivos se aplican las instrucciones. Por ejemplo, coincide con cualquier archivo TypeScript de pruebas que siga el patrón `**/*.test.ts`.
5. Fíjate en las instrucciones específicas para crear pruebas unitarias.
6. Por último, abre `.github/instructions/drizzle.instructions.md` y desplázate hasta el final. Observa los enlaces a otros archivos de instrucciones, como `unit-tests.instructions.md`, y a archivos existentes del proyecto. Esto permite dividir instrucciones complejas en archivos más pequeños y reutilizables y enlazar ejemplos. Las rutas son relativas al archivo de instrucciones, no a la raíz del repositorio.

> [!NOTE]
> La sección **Code formatting requirements** de `copilot-instructions.md` documenta los estándares de programación, pero todavía no exige documentación en el código. En los pasos siguientes añadirás reglas para los comentarios TSDoc y el encabezado de comentario de cada archivo.

## Crear una rama para los cambios

Sigue las buenas prácticas y crea una rama para los cambios.

1. Vuelve al Codespace que usaste en el ejercicio anterior.
2. Abre un terminal nuevo con <kbd>Control</kbd>+<kbd>`</kbd> (Mac) o <kbd>Ctrl</kbd>+<kbd>`</kbd> (Windows/Linux).
3. Crea una rama nueva y cámbiate a ella:

   ```bash
   git checkout -b custom-instructions
   ```

## Usar Copilot Chat *antes* de actualizar las instrucciones

> [!TIP]
> **Abrir Copilot Chat**
>
> Antes de empezar los ejercicios siguientes, vuelve a tu Codespace, abre el panel de Copilot Chat y selecciona **New Chat** para iniciar una conversación limpia. El modo y el modelo pueden variar según el ejercicio; cada paso lo indicará cuando corresponda.

Para observar el efecto de las instrucciones personalizadas, primero envía un prompt con el archivo de instrucciones actual. Más adelante lo actualizarás y volverás a enviar el mismo prompt para ver la diferencia.

1. Cierra las pestañas abiertas de ejercicios anteriores para que Copilot solo tenga el contexto que necesitas.
2. Abre `src/lib/publishers.ts` para que Copilot sepa dónde debe guardar el asistente.
3. En el selector de agentes de la vista Chat, selecciona **Agent** para que Copilot pueda aplicar cambios en los archivos.

   ![Selector de agente en la vista Chat](../../../_images/shared-chat-mode-selector.png)

4. Envía este prompt:

   ```plaintext
   Create or update src/lib/publishers.ts with a data-access helper that returns a list of all publishers with the name and id for each. Apply the file changes.
   ```

5. Copilot explora el proyecto y aplica cambios, normalmente tanto al archivo del asistente como a sus pruebas.
6. Observa que la función auxiliar propuesta es una función tipada que recibe un cliente `db` como primer argumento y devuelve una matriz tipada de editoriales. Este comportamiento se debe a las convenciones de la capa de datos de `.github/instructions/drizzle.instructions.md`, que se aplica a `src/lib/*.ts`.
7. Observa que al código propuesto **le faltan** los comentarios TSDoc y un encabezado de comentario de archivo.

> [!CAUTION]
> Como Copilot es probabilístico, es posible que añada comentarios incluso sin que se lo pidas. No pasa nada: el objetivo es observar la mejora de la *coherencia* tras actualizar las instrucciones.

## Añadir un estándar al repositorio

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

## Volver a enviar el prompt y observar el cambio

1. Vuelve a Copilot Chat y selecciona **New Chat** para limpiar el contexto.
2. Vuelve a seleccionar `src/lib/publishers.ts` para que Copilot se centre en el archivo adecuado.
3. Envía **el mismo prompt** que antes:

   ```plaintext
   Create or update src/lib/publishers.ts with a data-access helper that returns a list of all publishers with the name and id for each. Apply the file changes.
   ```

4. Observa que el archivo propuesto empieza con un bloque de comentarios parecido a este:

   ```typescript
   /**
    * Asistentes de acceso a datos de editoriales para la plataforma de financiación colectiva de Tailspin Toys.
    * Proporciona funciones para recuperar información de editoriales de la base de datos.
    */
   ```

5. Observa que la función propuesta ahora incluye un comentario TSDoc parecido a este:

   ```typescript
   /**
    * Devuelve una lista de todas las editoriales con su identificador y nombre.
    *
    * @param db - Cliente de base de datos Drizzle.
    * @returns Promesa que se resuelve en una matriz de objetos que representan editoriales.
    */
   ```

Has orientado a Copilot para que siga un estándar del proyecto y lo aplique a código real sobre el que trabajarás en el siguiente ejercicio.

## Confirmar los cambios de instrucciones y enviar la rama

Los archivos de instrucciones, como cualquier otro elemento del repositorio, se administran mediante el control de código fuente. Confirma los cambios y envía la rama al repositorio.

1. Abre otro terminal con <kbd>Control</kbd>+<kbd>`</kbd> (Mac) o <kbd>Ctrl</kbd>+<kbd>`</kbd> (Windows/Linux).
2. Confirma que estén presentes la actualización de instrucciones y los cambios del asistente:

   ```bash
   git status
   ```

3. Prepara y confirma la actualización de instrucciones y todos los archivos que haya modificado Copilot para crear el asistente:

   ```bash
   git add .github/copilot-instructions.md src/lib/publishers.ts
   # Si git status muestra actualizaciones adicionales (por ejemplo, pruebas), añade también esos archivos.
   git commit -m "Add doc comment standards and publishers helper foundation"
   ```

4. Envía la rama al repositorio:

   ```bash
   git push -u origin custom-instructions
   ```

## Crear y combinar una solicitud de incorporación de cambios

Una vez enviada la rama, crearás una solicitud de incorporación de cambios y la vincularás a la incidencia sobre los estándares de documentación del backlog. Copilot puede buscar la incidencia, crear una PR que la cierre y combinarla.

> [!NOTE]
> Este es el primer ejercicio en el que Copilot actúa en GitHub en tu nombre. La plantilla del proyecto ya configura las herramientas de GitHub, así que no hace falta preparar nada. La primera vez que Copilot use una herramienta, es posible que VS Code te pida iniciar sesión en GitHub. Sigue las indicaciones para permitirlo. En un ejercicio posterior aprenderás cómo funciona esta conexión mediante Model Context Protocol.

1. Abre Copilot Chat en el Codespace.
2. Pulsa <kbd>Control</kbd>+<kbd>Command</kbd>+<kbd>I</kbd> (Mac) o <kbd>Ctrl</kbd>+<kbd>Alt</kbd>+<kbd>I</kbd> (Windows/Linux) para abrir la vista Copilot Chat y comprueba que **Agent** esté seleccionado en el selector de agentes.
3. Pide a Copilot que busque la incidencia sobre la actualización de los archivos de instrucciones y que cree una PR desde la rama actual. Indícale que describa tanto la actualización de instrucciones como la base de la función auxiliar de editoriales:

   ```plaintext
   Find the issue related to updating the instructions file. Create a new PR from the current branch, highlight that the PR closes that issue, and include that we also added the publishers helper foundation for the upcoming filtering work.
   ```

4. Copilot buscará la incidencia y creará la PR.
5. Si Copilot te pide permiso para realizar acciones de GitHub, revisa el comando y selecciona **Allow** cuando corresponda.
6. Después de crear la PR, pide a Copilot que la combine y que vuelva a `main` local:

   ```plaintext
   Merge the PR into main. Then return to main locally, and pull the latest code so we are up to date.
   ```

7. Si Copilot te pide permiso para realizar acciones de GitHub y ejecutar comandos de shell, revisa el comando y selecciona **Allow** cuando corresponda.

¡Ya has creado y combinado una solicitud de incorporación de cambios con GitHub Copilot!

## Resumen y próximos pasos

Has explorado cómo obtiene contexto Copilot de los archivos de instrucciones y has usado Copilot Chat en VS Code para:

- enviar un prompt de generación de código y observar el resultado con las instrucciones *existentes*.
- añadir un estándar general a `.github/copilot-instructions.md`.
- volver a enviar el mismo prompt y observar cómo el código propuesto adopta el nuevo estándar.
- confirmar la actualización de instrucciones y la base del asistente en `main` para que el siguiente ejercicio pueda continuar el trabajo.

A continuación, pondrás las instrucciones en práctica en el [modo de agente][next-lesson] mientras Copilot añade una funcionalidad a distintas partes de la base de código.

## Recursos

- [Archivos de instrucciones para personalizar GitHub Copilot][instruction-files]
- [Buenas prácticas para crear instrucciones personalizadas][instructions-best-practices]
- [Cinco consejos para escribir mejores instrucciones personalizadas][copilot-instructions-five-tips]
- [Instrucciones personalizadas personales para GitHub Copilot][personal-instructions]
- [Awesome Copilot: colección de archivos de instrucciones][awesome-copilot]

[previous-lesson]: ../0-prerequisites/
[next-lesson]: ../2-agent-mode/
[instruction-files]: https://code.visualstudio.com/docs/copilot/copilot-customization
[instructions-best-practices]: https://docs.github.com/enterprise-cloud@latest/copilot/using-github-copilot/coding-agent/best-practices-for-using-copilot-to-work-on-tasks#adding-custom-instructions-to-your-repository
[personal-instructions]: https://docs.github.com/copilot/customizing-copilot/adding-personal-custom-instructions-for-github-copilot
[copilot-instructions-five-tips]: https://github.blog/ai-and-ml/github-copilot/5-tips-for-writing-better-custom-instructions-for-copilot/
