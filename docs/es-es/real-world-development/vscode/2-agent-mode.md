---
title: "Ejercicio 2: añadir funcionalidad con el modo de agente de Copilot"
authors:
  - geektrainer
lastUpdated: 2026-06-30
---

Incluso las actualizaciones más sencillas de una aplicación suelen requerir cambios en varios archivos y tareas como ejecutar pruebas. Como desarrollador, normalmente tienes que localizar los archivos, hacer los cambios, ejecutar las pruebas, depurar los problemas, averiguar qué archivo falta y volver a actualizarlo. Y así sucesivamente.

Para eso sirve el modo de agente de Copilot.

El modo de agente de Copilot trabaja con más autonomía en el IDE. Se comporta de forma parecida a un desarrollador: empieza por explorar la estructura del proyecto, realiza los cambios necesarios, ejecuta tareas como las pruebas y corrige automáticamente los errores que detecta. Veamos cómo usar el modo de agente para añadir funcionalidad al sitio.

> [!NOTE]
> Aunque los nombres se parecen, el modo de agente y el agente en la nube están diseñados para experiencias distintas. El modo de agente trabaja en el IDE y permite iterar y recibir comentarios rápidamente. El agente en la nube funciona de forma asíncrona, como un miembro del equipo, y se comunica contigo mediante incidencias y solicitudes de incorporación de cambios.

En este ejercicio aprenderás a:

- explorar el proyecto con el modo de agente, identificar archivos relacionados y coordinar cambios.
- implementar funcionalidades en la interfaz y la capa de datos con el modo de agente de GitHub Copilot.
- revisar los cambios y las pruebas que genere el modo de agente antes de incorporarlos a la base de código.

## Escenario

El catálogo de juegos crece y quieres permitir que los usuarios filtren por categoría y editorial. En el ejercicio anterior ya añadiste una función auxiliar de editoriales. Ahora completarás con el modo de agente el resto del trabajo de la capa de datos, la interfaz y las pruebas.

## Ejecutar el sitio web Tailspin Toys

Antes de hacer cambios, explora el sitio web de Tailspin Toys para conocer sus funciones actuales.

El sitio es una plataforma de financiación colectiva de juegos de mesa con un tema de desarrollo. Permite mostrar juegos y sus detalles. Es una única aplicación Astro que genera páginas HTML estáticas durante la compilación. Las páginas consultan directamente una base de datos SQLite local en sus metadatos mediante Drizzle ORM; no hay una API de backend independiente ni un marco de interfaz en el cliente. Los asistentes reutilizables de acceso a datos están en `src/lib/`, y la interactividad se implementa con pequeños elementos `<script>` de Astro que usan API DOM estándar.

### Iniciar el sitio web

La tarea `npm run dev` inicia el único servidor de desarrollo de Astro.

1. Vuelve a tu Codespace. Después del ejercicio anterior, deberías estar en `main`.
2. Abre un terminal nuevo en el Codespace con <kbd>Control</kbd>+<kbd>`</kbd> (Mac) o <kbd>Ctrl</kbd>+<kbd>`</kbd> (Windows/Linux).
3. Actualiza `main` y crea una rama para el trabajo de filtrado:

   ```bash
   git checkout main
   git pull
   git checkout -b filtering-vscode
   ```

4. Inicia el sitio web:

   ```bash
   npm run dev
   ```

   Cuando el servidor de desarrollo de Astro esté listo, aparecerá un mensaje parecido a este:

   ```bash
   🚀 Tailspin Toys is ready!
      Astro server: http://localhost:4321
      Press Ctrl-C to stop.
   ```

> [!NOTE]
> Si aparece un cuadro de diálogo que te pide abrir `http://localhost:4321` en el navegador, ciérralo con el botón **x**.

5. Mantén pulsada la tecla <kbd>Command</kbd> (Mac) o <kbd>Ctrl</kbd> (Windows/Linux) y selecciona la dirección del servidor Astro `http://localhost:4321` en el terminal para abrir el sitio web.

> [!NOTE]
> En un Codespace, al seleccionar el enlace localhost del terminal se te redirige automáticamente a `https://<your-codespace-name>-4321.app.github.dev/`. Es un túnel privado al Codespace, donde se ejecuta el servidor web.

### Explorar el sitio web

Con el sitio web en ejecución, puedes explorar sus funciones principales:

- **Página principal**: muestra una lista de juegos de mesa con títulos, imágenes y descripciones.
- **Página de detalles del juego**: al seleccionar un juego, se abre una página con más información, como el título, la descripción, la editorial y la categoría.

## Explorar el backlog con Copilot

> [!TIP]
> **Abrir Copilot Chat**
>
> Antes de empezar los ejercicios siguientes, vuelve a tu Codespace, abre el panel de Copilot Chat y selecciona **New Chat** para iniciar una conversación limpia. El modo y el modelo varían según el ejercicio; cada paso lo indicará cuando corresponda.

La implementación inicial del sitio web funciona, pero queremos mejorarla con nuevas capacidades. Empezaremos por revisar el backlog. Al configurar el proyecto a partir de la plantilla, se creó automáticamente un backlog de incidencias. Pide a GitHub Copilot que te las muestre.

1. En el selector de agentes de la vista Chat, selecciona **Agent**. El agente planifica e implementa cambios de forma autónoma en varios archivos, ejecuta comandos del terminal e invoca herramientas.

   ![Selector de agente en la vista Chat](../../../_images/shared-chat-mode-selector.png)

2. En la lista de modelos disponibles, selecciona **Claude Sonnet 4.5**.

> [!CAUTION]
> Los autores del taller no recomiendan un modelo específico. Al crear este taller usamos Claude Sonnet 4.5 y lo incluimos en las instrucciones para que las sugerencias de código fueran relativamente coherentes y la experiencia fuera satisfactoria. Sin embargo, los modelos de lenguaje son probabilísticos, así que es normal que las sugerencias difieran de las que se describen en el taller.

> [!NOTE]
> Debido a la naturaleza probabilística de los modelos de lenguaje, Copilot puede usar otro comando MCP, pero debería poder completar la tarea.

3. Pide a Copilot que muestre el backlog de incidencias con este prompt:

   ```plaintext
   Please show me the backlog of items from my GitHub repository. Help me prioritize them based on those which will be most useful to the user.
   ```

4. Selecciona **Continue** para ejecutar el comando que enumera las incidencias.
5. Revisa la lista generada.

Observa que Copilot incluso ha priorizado las incidencias en función de cuáles considera más útiles para el usuario.

## Revisar los archivos de instrucciones

Antes de pedir al agente que genere el código, revisa el archivo de instrucciones que proporcionará contexto a Copilot. Usarás el archivo de instrucciones de [interfaz de usuario (UI)](https://github.com/github-samples/tailspin-toys/blob/main/.github/instructions/ui.instructions.md), que explica cómo añadir funcionalidad al sitio.

1. En tu Codespace, abre `.github/instructions/ui.instructions.md`.
2. Revisa las directrices generales para añadir funcionalidad:
   - una descripción general de la arquitectura.
   - principios de diseño de componentes, capacidad de prueba y accesibilidad.
   - enlaces a archivos de instrucciones específicos para distintos tipos de archivo, incluidos:
     - Astro.
     - Tailwind CSS.
     - Drizzle.

> [!TIP]
> Los archivos de instrucciones pueden hacer referencia a otros archivos de instrucciones y a archivos del proyecto. Las rutas son relativas al archivo de instrucciones. Esto permite reutilizar contenido, dividir instrucciones complejas en fragmentos más manejables y proporcionar ejemplos y plantillas.

## Implementar la funcionalidad de filtrado

Para completar el filtrado habrá que realizar al menos tres cambios en la aplicación:

- añadir o perfeccionar la lógica de filtrado restante en la capa de datos (`src/lib/`).
- añadir o actualizar las pruebas del comportamiento de filtrado.
- actualizar la página de lista de juegos para incorporar la interfaz de filtrado.

Además, las pruebas deben ejecutarse y finalizar correctamente antes de incorporar los cambios a la base de código. El modo de agente de Copilot puede encargarse de estas tareas.

1. Continúa en la conversación actual de Copilot o inicia otra seleccionando **New Chat**.
2. Selecciona **Add Context**, **Instructions** y **ui** como archivo de instrucciones.

   ![Ejemplo de selección del archivo de instrucciones de UI](../../../_images/ex3-select-instructions-file.png)

3. Comprueba que **Agent** siga seleccionado en el selector de agentes de la vista Chat.

   ![Selector de agente en la vista Chat](../../../_images/shared-chat-mode-selector.png)

4. Comprueba que siga seleccionado el modelo **Claude Sonnet 4.5**.
5. Pide a Copilot que implemente la funcionalidad según la incidencia relacionada del backlog:

   ```plaintext
   Please update the site to include filtering by publisher and category based on the requirements from the related GitHub issue in the backlog. A publishers helper already exists from the previous exercise, so preserve and refine that work as needed while completing the remaining data-layer, UI, and tests. Ensure all tests are passing before completion. The server is already running, so you do not need to start it up.
   ```

6. Observa cómo Copilot explora el proyecto y localiza los archivos relacionados con la funcionalidad. Debería identificar los asistentes de la capa de datos, la interfaz y las pruebas. Después, modificará los archivos y ejecutará las pruebas.

   ![Copilot explora los archivos del proyecto](../../../_images/ex3-agent-mode-explores.png)

> [!NOTE]
> Copilot ejecutará varias tareas, como explorar el proyecto, modificar archivos y ejecutar pruebas. Según la complejidad de la tarea y de la base de código, puede tardar unos minutos. Durante ese proceso, el editor de código puede mostrar los botones **Keep** y **Undo**. Cuando Copilot termine, tendrás que aceptar o deshacer todos los cambios; no hace falta que selecciones esos botones mientras sigue trabajando.

7. Cuando Copilot te lo pida, selecciona **Continue** para ejecutar las pruebas.

   ![Cuadro de diálogo de Copilot Chat que pide confirmar la ejecución de pruebas](../../../_images/ex3-agent-mode-run-tests.png)

8. Es posible que Copilot se detenga varias veces e incluso que algunas pruebas fallen durante el proceso. No pasa nada: Copilot alternará entre generar código y ejecutar pruebas hasta completar la tarea y dejar de detectar errores.

   ![Sesión de Copilot Agent Mode completada](../../../_images/ex3-agent-mode-proposed-changes.png)

9. Explora el código generado y comprueba si hay posibles problemas.

> [!CAUTION]
> Recuerda que siempre es importante revisar el código generado por Copilot o cualquier otra herramienta de IA.

10. Vuelve al navegador donde se está ejecutando el sitio web y explora la nueva funcionalidad.
11. Cuando hayas comprobado que todo funciona y revisado el código, selecciona **Keep** en la ventana de Copilot Chat.

## Resumen y próximos pasos

¡Enhorabuena! En este ejercicio has usado el modo de agente de GitHub Copilot para añadir funciones al sitio web Tailspin Toys. Has aprendido a:

- implementar funcionalidades en la interfaz y la capa de datos con el modo de agente de GitHub Copilot.
- explorar el proyecto con el modo de agente, identificar archivos relacionados y coordinar cambios.
- revisar los cambios y las pruebas generados antes de incorporarlos a la base de código.

Ahora [probarás la funcionalidad con el servidor MCP de Playwright][next-lesson] y abrirás una solicitud de incorporación de cambios.

### Ejercicio adicional: implementar la paginación

Al crecer la lista de juegos, habrá que habilitar la paginación. Usa lo que has aprendido para pedir a Copilot que implemente la paginación del sitio. Ten en cuenta lo siguiente:

- sigue las buenas prácticas existentes, incluidos los archivos de instrucciones.
- decide cómo quieres implementar la paginación y si quieres que el usuario elija el tamaño de página o usar un tamaño fijo.
- proporciona a Copilot las directrices necesarias para implementar lo que necesitas.
- es posible que tengas que iterar con GitHub Copilot, pedir cambios y proporcionar contexto. Es normal al trabajar con Copilot.

## Recursos

- [Modos ask, edit y agent de Copilot: funciones y usos][choose-mode]
- [Modo de agente en VS Code][vs-code-agent-mode]

[previous-lesson]: ../1-custom-instructions/
[next-lesson]: ../3-mcp/
[choose-mode]: https://github.blog/ai-and-ml/github-copilot/copilot-ask-edit-and-agent-modes-what-they-do-and-when-to-use-them/
[vs-code-agent-mode]: https://code.visualstudio.com/docs/copilot/chat/chat-agent-mode
