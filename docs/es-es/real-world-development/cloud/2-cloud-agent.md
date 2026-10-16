---
title: "Ejercicio 2: agente de GitHub Copilot en la nube"
authors:
  - geektrainer
lastUpdated: 2026-06-30
---

| [← Lección anterior: instrucciones personalizadas][previous-lesson] |
|:--|

Es probable que casi todas las organizaciones tengan problemas de deuda técnica. Puede tratarse de problemas de seguridad pendientes, código heredado que hay que actualizar o solicitudes de funcionalidades que llevan tiempo en el backlog porque no había tiempo para implementarlas. El agente de GitHub Copilot en la nube está diseñado para realizar tareas como actualizar código y añadir funcionalidades de forma autónoma. Cuando termina, crea una solicitud de incorporación de cambios (PR) en borrador para que un desarrollador la revise. Así puedes delegar las tareas tediosas, acelerar el desarrollo y centrarte en el trabajo de mayor alcance.

Con el agente de Copilot en la nube, explorarás:

- cómo personalizar el entorno de generación de código.
- cómo asegurarte de que las operaciones se realicen de forma segura.
- la importancia de definir incidencias con un alcance claro.
- cómo asignar incidencias a Copilot.

## Escenarios

Tailspin Toys tiene deuda técnica que quiere resolver. Los contratistas que crearon la primera versión del sitio dejaron la documentación en un estado poco satisfactorio: verás que no existe. Como primer paso, quieren añadir comentarios TSDoc a todas las funciones exportadas de la aplicación.

Además, el equipo de diseño quiere facilitar el descubrimiento de juegos. Quiere que la página de detalles de cada juego muestre títulos relacionados, es decir, otros juegos de la misma categoría, para que los usuarios puedan seguir explorando. De momento no hace falta una implementación pulida; solo quieren poder usarla para realizar pruebas de aceptación de la experiencia de usuario. Aunque esta tarea es un bloqueo, hay otras incidencias de mayor prioridad.

Estas tareas se pueden posponer fácilmente y son buenas candidatas para asignarlas al agente de Copilot en la nube. Este puede trabajar en ellas de forma asíncrona, mientras el desarrollador se ocupa de otros asuntos y vuelve después a revisar el trabajo.

## Presentación del agente de GitHub Copilot en la nube

El [agente de GitHub Copilot en la nube](https://docs.github.com/copilot/concepts/agents/cloud-agent/about-cloud-agent#overview-of-copilot-cloud-agent-formerly-copilot-coding-agent) puede realizar tareas en segundo plano de forma parecida a un desarrollador. Al igual que con un desarrollador, puedes asignarle tareas de varias formas, por ejemplo, [asignándole una incidencia de GitHub](https://docs.github.com/copilot/how-tos/use-copilot-agents/cloud-agent/start-copilot-sessions). Una vez asignada, Copilot crea una solicitud de incorporación de cambios en borrador para seguir el progreso, configura un entorno y empieza a trabajar. Puedes consultar la sesión mientras trabaja o cuando termine. Cuando la solución esté lista para que la revises, te mencionará en la PR.

## Importancia de delimitar bien las instrucciones

GitHub Copilot no hace magia: no elimina la necesidad de pensar. Incluso las tareas que parecen sencillas esconden complejidad; por eso, [define con cuidado el alcance de las tareas del agente de Copilot en la nube][cloud-agent-best-practices]. Trátalo como a un compañero de programación con IA: trabaja por etapas, aprende, experimenta y adapta el enfoque. Los fundamentos del desarrollo de software no cambian por incorporar IA generativa.

## Instrucciones personalizadas de este repositorio

En los ejercicios anteriores has visto cómo las instrucciones personalizadas guían a Copilot. Si ya has completado el ejercicio práctico de instrucciones personalizadas, habrás visto `.github/copilot-instructions.md` en acción. Si no, es buen momento para echarle un vistazo. Antes de asignar trabajo al agente en la nube, lee los archivos de instrucciones incluidos en el repositorio sin modificarlos y fíjate en cómo influyen en su trabajo.

Abre estos archivos en la interfaz web de GitHub del repositorio o en un Codespace que ya esté en ejecución:

- `.github/copilot-instructions.md`: revisa la sección **Code standards**, en especial las directrices de TypeScript y los comentarios TSDoc.
- `.github/instructions/unit-tests.instructions.md`: observa el campo `applyTo` de los metadatos; limita la aplicación de estas instrucciones a los archivos `**/*.test.ts`.

Cuando asignes la incidencia `Code lacks documentation` al agente en la nube en la sección siguiente, fíjate en los comentarios TSDoc, las convenciones de TypeScript y los encabezados de comentarios que aparezcan en la PR. Estas directrices provienen de los archivos de instrucciones. Volveremos a destacarlas cuando revisemos las PR en un ejercicio posterior.

## Configurar el entorno de desarrollo del agente en la nube

La creación de código suele requerir un entorno específico y la ejecución de scripts de configuración para dejarlo todo listo. Lo mismo sucede al asignar tareas a Copilot, que trabaja de forma parecida a un ingeniero de software.

El agente en la nube usa [GitHub Actions][github-actions] para crear su entorno. Puedes personalizarlo mediante un [flujo de trabajo de configuración especial][setup-workflow], definido en `.github/workflows/copilot-setup-steps.yml`, que se ejecuta antes de empezar el trabajo. Así puedes instalar las herramientas y dependencias necesarias. Para simplificar el laboratorio y facilitar el aprendizaje, este flujo de trabajo ya está configurado. Permite que Copilot acceda a Node.js, las dependencias del proyecto, los binarios del navegador para las pruebas de extremo a extremo y la base de datos SQLite migrada y con datos iniciales de la aplicación Astro:

```yaml
name: "Copilot Setup Steps"

# Permite probar los pasos de configuración desde la pestaña "Actions" del repositorio
on: workflow_dispatch

env:
  ASTRO_TELEMETRY_DISABLED: "1"

jobs:
  copilot-setup-steps:
    runs-on: ubuntu-latest
    # Establece los permisos mínimos necesarios para *tus pasos*. Copilot tendrá su propio token para sus operaciones.
    permissions:
      # Si quieres clonar el repositorio como parte de la configuración, por ejemplo para instalar dependencias, necesitarás el permiso `contents: read`. Si no lo clonas en los pasos de configuración, Copilot lo hará automáticamente cuando estos terminen.
      contents: read
    steps:
      - name: Checkout code
        uses: actions/checkout@v5

      # Configuración del frontend y la aplicación: Node.js (ahora toda la aplicación usa Astro + Drizzle/libSQL)
      - name: Set up Node.js
        uses: actions/setup-node@v6
        with:
          node-version: "lts/*"
          cache: "npm"
          cache-dependency-path: "./package-lock.json"

      - name: Install JavaScript dependencies
        run: npm ci

      - name: Install Playwright browsers
        run: npx playwright install --with-deps chromium

      # Migra y carga datos iniciales en SQLite local para que la compilación y las pruebas dispongan de datos.
      - name: Set up the database
        run: npm run db:setup
```

El archivo se parece a cualquier otro flujo de trabajo de GitHub, pero tiene varios detalles importantes:

- Incluye un único trabajo llamado `copilot-setup-steps`. Se ejecuta en GitHub Actions antes de que Copilot empiece a trabajar en la PR.
- El desencadenador `workflow_dispatch` permite ejecutar manualmente el flujo de trabajo desde la pestaña **Actions** del repositorio. Resulta útil para probar que funcione correctamente sin tener que esperar a que Copilot lo ejecute.

## Añadir documentación

Aunque todo el mundo entiende la importancia de la documentación, muchos proyectos tienen información desactualizada o carecen de ella. Esta deuda técnica ralentiza el trabajo y dificulta el mantenimiento de la base de código y la incorporación de nuevos desarrolladores. Copilot destaca en la generación de documentación, así que esta incidencia es ideal para asignársela al agente en la nube. Trabajará en segundo plano para generar la documentación necesaria; en un ejercicio posterior revisarás los cambios.

1. En una nueva pestaña del navegador, ve a tu repositorio en github.com.
2. Selecciona la pestaña **Issues**.
3. Selecciona **New issue** para abrir el cuadro de diálogo de nueva incidencia.
4. Selecciona **Blank issue** para crear la incidencia.
5. En **Title**, escribe `Code lacks documentation`.
6. En **Description**, escribe:

    ```plaintext
    Our organization has a requirement that functions and methods include TSDoc doc comments where helpful. Unfortunately, recent updates haven't followed this standard. We need to update the existing code to ensure doc comments are included where they clarify behavior.
    ```

7. Selecciona **Create** para crear la incidencia.
8. En el lado derecho, selecciona **Assign to Copilot** para abrir el cuadro de diálogo de asignación.

    ![Asignar Copilot a una incidencia](../../../_images/shared-assign-copilot.png)

9. Selecciona **Assign**.
10. Selecciona la pestaña **Pull Requests**.
11. Abre la PR que acaba de generar Copilot. Tendrá un título parecido a `[WIP]: Code lacks documentation`. Si no aparece ninguna PR nueva en la lista, espera un minuto y actualiza la ventana del navegador.
12. Al cabo de unos minutos, Copilot debería crear una lista de tareas.

> [!NOTE]
> La lista de tareas puede tardar varios minutos en aparecer en la PR. Copilot está preparando su entorno (mediante el flujo de trabajo descrito anteriormente), analizando el proyecto y determinando el mejor enfoque para resolver la tarea.

13. Revisa la lista y las tareas que se propone completar.
14. Desplázate por la cronología de la PR. Debería aparecer una actualización que indique que Copilot ha empezado a trabajar en la incidencia.
15. Selecciona el botón **View session**.

    ![Vista de la sesión de Copilot](../../../_images/ex4-view-session.png)

> [!CAUTION]
> Es posible que tengas que actualizar la ventana para ver el indicador actualizado.

16. Observa la sesión en directo y cómo Copilot resuelve el problema. Verás cómo explora el código, analiza el estado del proyecto, se detiene a decidir cuál es el plan adecuado y crea código.

El proceso tardará varios minutos. Una de las principales ventajas del agente de Copilot en la nube es que trabaja de forma asíncrona y te permite ocuparte de otras tareas. Aprovecharemos esta función: asignaremos otra tarea al agente y, mientras trabaja, añadiremos código para incorporar funcionalidades a la aplicación.

## Añadir una sección de juegos relacionados a la página de detalles

Una de las principales ventajas del agente de Copilot en la nube es que permite dividir el trabajo: tú puedes centrarte en una tarea mientras él se ocupa de otra. Aunque añadir una sección de juegos relacionados para el equipo de diseño no lleve mucho tiempo, ese tiempo se podría dedicar a otras tareas. Vamos a asignárselo al agente.

1. Vuelve a tu repositorio en github.com.
2. Selecciona la pestaña **Issues**.
3. Selecciona **New issue** para abrir el cuadro de diálogo de nueva incidencia.
4. Selecciona **Blank issue** para usar la plantilla en blanco.
5. En **Title**, escribe `Show related games on the game details page`.
6. En **Description**, escribe:

    ```markdown
    We want to help people discover more games by showing related games on each game's details page. The design team wants to explore the UX and do some acceptance testing. Our requirements are:

    - Add a data-access helper in `src/lib/` that returns other games in the same category as a given game, excluding that game
    - Show a "Related games" section on the game details page that uses the helper
    - Handle the case where a game has no related games
    - There should be unit tests created for the new helper
    - Before creating the PR, ensure all tests pass
    ```

7. Selecciona **Create** para crear la incidencia.
8. En el lado derecho, selecciona **Assign to Copilot** para abrir el cuadro de diálogo de asignación.

    ![Asignar Copilot a una incidencia](../../../_images/shared-assign-copilot.png)

9. Selecciona **Assign**.

Al poco tiempo aparecerá un conjunto de ojos 👀 en el primer comentario de la incidencia para indicar que Copilot ya está trabajando.

![Copilot usa el emoji de ojos para indicar que está trabajando en la incidencia](../../../_images/ex4-issue-eyes-emoji.png)

Copilot está trabajando en tu segunda solicitud. El agente en la nube trabaja de forma parecida a un ingeniero de software; no hace falta supervisarlo activamente, pero sí revisar el trabajo cuando termine. Ahora crearás y usarás agentes personalizados.

## Resumen y próximos pasos

En este ejercicio has explorado el [agente de GitHub Copilot en la nube][copilot-agents]. Puedes asignarle incidencias para que trabaje de forma asíncrona. Copilot puede resolver deuda técnica, crear funcionalidades o ayudar a migrar código de un marco de trabajo a otro.

Has aprendido a:

- personalizar el entorno de generación de código.
- asegurarte de que las operaciones se realicen de forma segura.
- definir incidencias con un alcance claro.
- asignar incidencias a Copilot.

Mientras el agente trabaja en segundo plano, puedes crear y usar agentes personalizados. El [agente de Copilot en la nube también puede usar servidores MCP][cloud-agent-mcp] y dispone de instrucciones personalizadas, que exploraste en módulos anteriores.

## Recursos

- [Acerca del agente de Copilot][copilot-agents]
- [Asignar incidencias de GitHub a Copilot][assign-issue]
- [Buenas prácticas para el flujo de trabajo de configuración del agente de Copilot en la nube][cloud-agent-best-practices]

| [Lección siguiente: agentes personalizados →][next-lesson] |
|--:|

[previous-lesson]: ../1-custom-instructions/
[next-lesson]: ../3-custom-agents/
[cloud-agent-mcp]: https://docs.github.com/copilot/how-tos/copilot-on-github/customize-copilot/customize-cloud-agent/extend-cloud-agent-with-mcp
[assign-issue]: https://docs.github.com/copilot/how-tos/use-copilot-agents/cloud-agent/start-copilot-sessions
[setup-workflow]: https://docs.github.com/copilot/how-tos/copilot-on-github/customize-copilot/customize-cloud-agent/customize-the-agent-environment
[copilot-agents]: https://docs.github.com/copilot/concepts/agents/cloud-agent/about-cloud-agent
[cloud-agent-best-practices]: https://docs.github.com/copilot/how-tos/copilot-on-github/customize-copilot/customize-cloud-agent/customize-the-agent-environment
[github-actions]: https://docs.github.com/actions
