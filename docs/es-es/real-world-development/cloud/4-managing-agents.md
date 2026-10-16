---
title: "Ejercicio 4: supervisar y administrar agentes"
authors:
  - geektrainer
lastUpdated: 2026-06-30
---

En los dos ejercicios anteriores, has asignado tres tareas distintas al agente de Copilot en la nube para mejorar la experiencia de usuario y añadir funcionalidad. Aunque el agente trabaja de forma autónoma y asíncrona, sigue siendo importante supervisar esas tareas.

Hay varias herramientas para administrar las tareas asignadas al agente en la nube, como la [página de agentes][agents-page] de GitHub.com. Desde este centro de control puedes consultar todas las tareas de agentes que tienen solicitudes de incorporación de cambios abiertas, explorar las operaciones realizadas e incluso dirigir una sesión activa.

En esta lección:

- explorarás la página de agentes para supervisar las tareas del agente en la nube.
- dirigirás una sesión activa para solicitar funcionalidad adicional.

## Escenario

Después de asignar al agente la creación de un modo de alto contraste, el equipo decide que también sería buen momento para añadir un modo claro. Como ya se están modificando los estilos del sitio y añadiendo la posibilidad de alternar entre modos, parece lógico incluir esta función. Dirigirás el trabajo del agente para que añada tanto el modo claro como el de alto contraste.

## Revisar las tareas del agente de GitHub Copilot en la nube

Consulta el estado actual de todas las tareas asignadas al agente en la nube.

1. Ve a la [página de agentes de Copilot](https://github.com/copilot/agents).
2. Revisa la lista de tareas del panel principal y del panel lateral. Deberían aparecer las tareas que has asignado a Copilot, entre ellas:
   - actualizar la documentación de la base de código.
   - generar API para modificar los juegos.
   - añadir un modo de alto contraste al sitio web.
3. Selecciona una tarea en curso y revisa el trabajo realizado por Copilot. Puede incluir:
   - obtener el código del repositorio.
   - crear el entorno de trabajo de Copilot.
   - configurar servidores MCP.
   - ejecutar distintos pasos para completar la tarea asignada.

> [!NOTE]
> Los pasos exactos que aparezcan dependerán del estado del trabajo de Copilot y del enfoque que haya seguido.

4. Observa también el panel de solicitudes de incorporación de cambios (PR) que aparece a la derecha. Te permite consultar la PR y los archivos modificados para supervisar mejor el trabajo.

## Dirigir el agente en la nube

Ahora que ya has visto las tareas activas, pide a Copilot que añada la posibilidad de alternar al modo claro mientras trabaja en el modo de alto contraste.

1. Selecciona la sesión que corresponda a la creación del modo de alto contraste. El título exacto variará según el nombre que use Copilot y el estado actual del trabajo.

    ![Sesión de accesibilidad en el centro de agentes](../../../_images/ex6-accessibility-session.png)

2. Observa la sesión durante unos minutos, hasta que indique que ha terminado de configurarse y ha empezado a trabajar. Verás mensajes parecidos a los ejemplos siguientes.
3. En el cuadro de diálogo **Steer active session while Copilot is working**, escribe este prompt:

   ```plaintext
   While we are working on a high contrast mode, let's also add a light mode. There should be a switch for this mode as well where users can select their desired display mode.
   ```

    ![Tarea del agente en la nube en la página de agentes, con el cuadro de diálogo para dirigir la sesión resaltado](../../../_images/ex6-steer-cloud-agent-task.png)

4. Pulsa <kbd>Enter</kbd> para enviar el prompt.
5. Observa cómo Copilot reconoce el prompt y lo incorpora al flujo de trabajo.

## Dejar que Copilot trabaje

Como antes, Copilot se pondrá a trabajar en la tarea actualizada. Incorporará la nueva solicitud al flujo una vez que termine el paso en curso.

El trabajo tardará varios minutos. Mientras tanto, puedes detenerte a repasar todo lo que has aprendido y explorado.

## Resumen y próximos pasos

En este ejercicio has explorado la página de agentes de Copilot, el centro para supervisar y orientar las tareas del agente de GitHub Copilot en la nube. Desde aquí puedes seguir las tareas activas y terminadas, revisar el trabajo y cambiar la dirección de las sesiones en curso.

Has aprendido a:

- explorar el centro de agentes y la página de agentes para supervisar las tareas del agente en la nube.
- redirigir una sesión activa para solicitar funcionalidad adicional.

Mientras Copilot termina el trabajo de accesibilidad, puedes [iterar sobre las solicitudes de incorporación de cambios que ha creado][next-lesson].

## Recursos

- [Página de agentes de Copilot][agents-page]
- [Acerca de los agentes personalizados][custom-agents]

---

| [← Lección anterior: agentes personalizados][previous-lesson] | [Lección siguiente: iterar sobre el trabajo de Copilot →][next-lesson] |
|:--|--:|

[previous-lesson]: ../3-custom-agents/
[next-lesson]: ../5-iterating/
[agents-page]: https://github.blog/changelog/2025-07-03-agents-page-for-copilot-coding-agent-in-public-preview
[custom-agents]: https://docs.github.com/copilot/concepts/agents/cloud-agent/about-custom-agents
