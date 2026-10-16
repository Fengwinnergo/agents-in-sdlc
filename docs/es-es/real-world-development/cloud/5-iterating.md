---
title: "Ejercicio 5: iterar sobre el trabajo de GitHub Copilot"
authors:
  - geektrainer
lastUpdated: 2026-06-30
---

| [← Lección anterior: administrar agentes][previous-lesson] |
|:--|

## Revisar el trabajo

A lo largo de este laboratorio, has colaborado con GitHub Copilot en varias tareas para mejorar la experiencia de usuario y añadir funcionalidad. Le pediste que documentara el código, creara una función de juegos relacionados para que el equipo de diseño pudiera iterar sobre ella e implementara funciones de accesibilidad, incluidos modos de alto contraste y claro. Ahora revisarás los cambios y, si hace falta, darás comentarios a Copilot para mejorar el resultado.

### Escenario

Como hemos señalado varias veces, los fundamentos del diseño de software y DevOps no cambian al incorporar IA generativa. Siempre hay que revisar el código generado y seguir el proceso habitual de DevOps. Revisarás las propuestas de GitHub Copilot para documentar el código, crear la función de juegos relacionados y añadir funciones de accesibilidad antes de habilitar las revisiones para el resto del equipo.

## Seguridad y agente de GitHub Copilot en la nube

Como el agente de Copilot en la nube realiza tareas de forma asíncrona y sin supervisión, hay ciertas restricciones de seguridad para mantener un entorno seguro:

- Copilot solo tiene acceso de lectura al repositorio y acceso de escritura **únicamente** a la rama que usa para el código.
- El agente se ejecuta en GitHub Actions, que crea un entorno efímero e independiente para su trabajo.
- Una persona debe aprobar la ejecución de los flujos de trabajo de GitHub Actions.
- [El acceso a recursos externos está limitado de forma predeterminada](https://docs.github.com/copilot/how-tos/copilot-on-github/customize-copilot/customize-cloud-agent/customize-the-agent-firewall), incluidos los servidores MCP.

## Revisar la documentación generada

Empezaremos por la primera solicitud de incorporación de cambios (PR) que generó el agente de GitHub Copilot en la nube: la que añade documentación al código. Lo harás mediante la interfaz habitual de PR de GitHub.com.

> [!NOTE]
> Es posible que aparezca una advertencia de que el firewall ha bloqueado GitHub Copilot. **Es lo esperado**: Copilot tiene acceso limitado a recursos externos de forma predeterminada, incluidas las llamadas a servidores MCP externos. Si quieres, puedes [personalizar o deshabilitar el firewall del agente en la nube][agent-firewall].

1. Vuelve a tu repositorio en github.com.
2. Selecciona **Pull Requests** para abrir la lista de solicitudes de incorporación de cambios.
3. Abre la PR titulada, por ejemplo, **Add missing documentation** o un título más descriptivo.

> [!NOTE]
> Si Copilot aún está trabajando, la PR incluirá la etiqueta **[WIP]**. Espera a que termine. Puede tardar varios minutos; mientras tanto, puedes tomarte un descanso o repasar lo aprendido.

4. Cuando la PR esté lista, selecciona la pestaña **Files changed** y revisa los cambios.

    ![Pestaña Files changed](../../../_images/shared-pr-files-changed.png)

5. Explora el código actualizado, que incluye comentarios TSDoc y otra documentación. Los cambios exactos variarán.

    Al revisar los cambios, fíjate en los comentarios TSDoc, las convenciones de TypeScript y los encabezados de comentarios. Estos se deben a los archivos de instrucciones personalizadas que revisaste al principio del ejercicio 2.

6. Cuando hayas terminado de revisar las actualizaciones, vuelve a la pestaña **Conversation** y desplázate hacia abajo.
7. Deberías ver un indicador de que algunos flujos de trabajo esperan aprobación.
8. Si hay flujos de trabajo pendientes, selecciona **Approve and run workflows**.

   ![Aprobar y ejecutar flujos de trabajo](../../../_images/shared-approve-workflows.png)

9. Los flujos de trabajo deberían aparecer en cola en la sección de comprobaciones de la PR. Si todo va bien, las comprobaciones del proyecto para la aplicación Astro deberían finalizar correctamente. Puede tardar varios minutos.

## Pedir cambios a GitHub Copilot

La colaboración con Copilot en una solicitud de incorporación de cambios no es unidireccional. Puedes mencionar a Copilot en comentarios de la PR o en comentarios insertados en el código, igual que harías con otro miembro del equipo. Copilot verá esos comentarios e iniciará otra sesión para resolverlos. Como los resultados son probabilísticos, no hay un texto único que debas usar. Por ejemplo, puedes pedir a Copilot que:

- añada a la parte superior de cada archivo de código un encabezado de comentario que describa brevemente su función.
- añada comentarios TSDoc a los archivos TypeScript y Astro.
- cree un archivo README que describa la estructura de la aplicación Astro.

1. Añade un comentario para pedir un cambio en la documentación generada y menciona a **@copilot**, como harías con cualquier otra persona. Usa una de las ideas anteriores u otra sugerencia relacionada con la documentación que quieras incorporar a la base de código.
2. Selecciona **View Session** para observar el trabajo de Copilot. Verás que inicia una sesión nueva para aplicar los cambios.
3. Puedes seleccionar **Back to pull request** para volver a la PR.

    ![Volver a la solicitud de incorporación de cambios](../../../_images/ex7-back-to-pr.png)

4. Cuando Copilot haya completado los cambios, aparecerá un commit nuevo en la PR.
5. Selecciona la pestaña **Files changed** para revisar las modificaciones.

Puedes seguir iterando hasta que estés satisfecho con el resultado. Cuando lo estés, puedes marcar la PR en borrador como lista para revisión y combinarla en la rama principal.

![Marcar la PR como lista para revisión](../../../_images/ex7-ready-for-review.png)

## Revisar la función de juegos relacionados

Vuelve a la PR que generó Copilot para resolver la incidencia sobre los juegos relacionados en la página de detalles.

1. Vuelve a tu repositorio en GitHub.com.
2. Selecciona la pestaña **Pull Requests**.
3. Selecciona la PR titulada, por ejemplo, **Show related games on the game details page** o un título más descriptivo.
4. Selecciona la pestaña **Files changed** para revisar el código generado.
5. Cuando hayas revisado las actualizaciones, vuelve a la pestaña **Conversation** y desplázate hacia abajo.
6. Deberías ver un indicador de que algunos flujos de trabajo esperan aprobación.
7. Si hay flujos de trabajo pendientes, selecciona **Approve and run workflows**.

   ![Aprobar y ejecutar flujos de trabajo](../../../_images/shared-approve-workflows.png)

8. Los flujos de trabajo deberían aparecer en cola en la sección de comprobaciones de la PR. Si todo va bien, las comprobaciones del proyecto para la aplicación Astro deberían finalizar correctamente. Puede tardar varios minutos.
9. **Opcional:** puedes cambiar a esta rama en tu Codespace para probar manualmente la función de juegos relacionados. Ve a tu Codespace, abre el terminal y ejecuta estos comandos. Sustituye `<branch-name>` por el nombre de la rama que creó Copilot, como `copilot/fix-8`:

   ```bash
   git fetch origin
   git checkout <branch-name>
   ```

Copilot ha creado la función de juegos relacionados. Como antes, puedes iterar y pedirle cambios; por ejemplo, puedes solicitar que ajuste cuántos juegos relacionados se muestran o que añada encabezados y comentarios TSDoc (recuerda que la incidencia se asignó **antes** de actualizar las instrucciones personalizadas). Para ello, añade un comentario en la pestaña **Conversation**; Copilot lo verá e iniciará otra sesión.

## Revisar las funciones de accesibilidad

Por último, revisaremos las funciones de accesibilidad implementadas con el agente personalizado de accesibilidad. Esta PR debe incluir tanto el modo de alto contraste asignado en el ejercicio 3 como el modo claro solicitado desde el centro de agentes en el ejercicio 4.

1. Vuelve a tu repositorio en GitHub.com.
2. Selecciona la pestaña **Pull Requests**.
3. Selecciona la PR titulada, por ejemplo, **Add high contrast mode to website** o un título más descriptivo.

> [!NOTE]
> Si Copilot aún está trabajando, la PR incluirá la etiqueta **[WIP]**. Espera a que termine; puede tardar varios minutos.

4. Selecciona la pestaña **Files changed** para revisar el código que ha generado.
5. Revisa la implementación y presta atención a:
   - los componentes de interfaz que permiten alternar entre modos.
   - el uso del almacenamiento local para conservar las preferencias del usuario.
   - los cambios de CSS o estilos para los modos de alto contraste y claro.
   - los atributos de accesibilidad (etiquetas ARIA, navegación con teclado, etc.).
   - el código JavaScript/TypeScript que controla el cambio de modo.

6. Cuando hayas revisado las actualizaciones, vuelve a la pestaña **Conversation** y desplázate hacia abajo.
7. Deberías ver un indicador de que algunos flujos de trabajo esperan aprobación.
8. Si hay flujos de trabajo pendientes, selecciona **Approve and run workflows**.

   ![Aprobar y ejecutar flujos de trabajo](../../../_images/shared-approve-workflows.png)

9. Los flujos de trabajo deberían aparecer en cola en la sección de comprobaciones de la PR. Si todo va bien, las comprobaciones del proyecto para la aplicación Astro deberían finalizar correctamente. Puede tardar varios minutos.
10. **Opcional:** puedes cambiar a esta rama en tu Codespace para probar manualmente las funciones de accesibilidad. Ve a tu Codespace, abre el terminal y ejecuta estos comandos. Sustituye `<branch-name>` por el nombre de la rama que creó Copilot:

    ```bash
    git fetch origin
    git checkout <branch-name>
    ```

    Después, inicia la aplicación y prueba los modos de alto contraste y claro en el navegador. Comprueba que funcionen y que la selección se conserve después de recargar la página.

El agente personalizado de accesibilidad ha ayudado a Copilot a implementar estas funciones de acuerdo con buenas prácticas de accesibilidad. Si encuentras algún problema de accesibilidad o una posible mejora, puedes mencionarle a **@copilot** en un comentario para pedir una actualización, igual que hiciste con las otras PR.

## Ejercicio opcional: seguir delegando

El agente en la nube resulta más útil cuando puedes asignarle tareas reales del backlog y ocuparte mientras tanto de otros asuntos. Para practicar, crea varias incidencias en el repositorio y asígnaselas a Copilot. Algunas ideas:

- crear un formulario de interés para patrocinadores en la página de detalles de los juegos.
- implementar la paginación de la lista de juegos.
- añadir validación de entradas y gestión de errores a los asistentes de acceso a datos.

## Resumen

Has completado el recorrido del agente en la nube. En estas lecciones:

- **Has inspeccionado los archivos de instrucciones personalizadas** incluidos en el repositorio para observar su efecto en el trabajo del agente.
- **Has asignado incidencias al agente de Copilot en la nube** y has observado cómo configura el entorno, planifica y trabaja de forma asíncrona.
- **Has creado y usado un agente personalizado** de accesibilidad para añadir modos de alto contraste y claro.
- **Has usado la página de agentes de Copilot como centro de control** para supervisar y dirigir la sesión de accesibilidad mientras estaba en curso.
- **Has revisado e iterado sobre las solicitudes de incorporación de cambios del agente**, mencionando a `@copilot` para pedir cambios y aprobando los flujos de trabajo.

## Revisión y próximos pasos

Has completado el recorrido del agente en la nube. Los otros recorridos complementan lo que has practicado:

- 🖥️ **[Recorrido de VS Code](../../vscode/)**: explora el modo de agente de Copilot Chat y la integración de MCP directamente desde el IDE.
- 💻 **[Recorrido de CLI](../../cli/)**: entrega dos cambios revisados desde el terminal con los modos Plan y Autopilot, habilidades de agente, un agente de QA personalizado, Playwright MCP, Agent Merge y un puente opcional `/delegate` al agente en la nube.

En tu propio repositorio, prueba estas ideas:

- asigna al agente en la nube una tarea de refactorización o una incidencia para ampliar la cobertura de pruebas.
- crea un agente personalizado para otro ámbito.
- configura `copilot-setup-steps.yml` para otra pila tecnológica.

## Recursos

- [GitHub Copilot][github-copilot]
- [Acerca de los agentes de Copilot][copilot-agents]
- [Asignar incidencias de GitHub a Copilot][assign-issue]
- [Buenas prácticas para el flujo de trabajo de configuración del agente de Copilot en la nube][cloud-agent-best-practices]
- [Configurar el firewall del agente de Copilot en la nube][agent-firewall]

---

| [← Lección anterior: administrar agentes][previous-lesson] |
|:--|

[github-copilot]: https://github.com/features/copilot
[cloud-agent-overview]: https://docs.github.com/copilot/concepts/agents/cloud-agent/about-cloud-agent
[assign-issue]: https://docs.github.com/copilot/how-tos/use-copilot-agents/cloud-agent/start-copilot-sessions
[setup-workflow]: https://docs.github.com/copilot/how-tos/copilot-on-github/customize-copilot/customize-cloud-agent/customize-the-agent-environment
[copilot-agents]: https://docs.github.com/copilot/concepts/agents/cloud-agent/about-cloud-agent
[cloud-agent-best-practices]: https://docs.github.com/copilot/how-tos/copilot-on-github/customize-copilot/customize-cloud-agent/customize-the-agent-environment
[agent-firewall]: https://docs.github.com/copilot/how-tos/copilot-on-github/customize-copilot/customize-cloud-agent/customize-the-agent-firewall

[previous-lesson]: ../4-managing-agents/
