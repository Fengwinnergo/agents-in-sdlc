---
title: "Lección 9: comandos de barra de GitHub Copilot CLI"
description: "Explora los comandos de barra para administrar el contexto, elegir modelos, compartir sesiones y delegar trabajo opcionalmente al agente en la nube."
authors:
  - geektrainer
lastUpdated: 2026-09-18
---

Como cualquier buena herramienta de CLI, GitHub Copilot CLI incluye numerosos comandos de barra para interactuar con ella. Estos comandos ofrecen funciones avanzadas, información interna y opciones de configuración adicionales. Ya has usado comandos como `/diff`, `/mcp`, `/skills`, `/agent` y `/pr`. Ahora explorarás otros comandos útiles.

En esta lección:

- usarás `/context` y `/compact` para explorar cómo administra Copilot el contexto de la conversación.
- usarás `/model` para explorar los modelos disponibles.
- aprenderás cómo exportar o compartir una sesión con `/share`.
- explorarás comandos opcionales para trabajar en paralelo, usar árboles de trabajo y delegar tareas al agente en la nube.

## Escenario

Has completado el flujo de trabajo principal de la CLI. Ahora explorarás otras funciones: administración del contexto, cambio de modelos, uso compartido de sesiones y delegación opcional al [agente de GitHub Copilot en la nube][about-cloud-agent].

## Explorar el contexto de Copilot CLI

Al trabajar en tareas más grandes o complejas, es posible que alcances el límite de contexto del modelo. Copilot CLI comprime la conversación automáticamente cuando hace falta, pero también puedes inspeccionar o comprimir el contexto manualmente con comandos de barra.

1. Vuelve a tu Codespace e inicia Copilot CLI desde la raíz del repositorio si aún no está abierto.
2. Escribe:

   ```plaintext
   /context
   ```

3. Observa el modelo, el uso actual de tokens y la distribución del contexto entre instrucciones del sistema, herramientas, mensajes y espacio libre.
4. Comprime la conversación:

   ```plaintext
   /compact
   ```

5. Vuelve a escribir `/context` y compara el resultado. Si la conversación ya era breve, quizá apenas notes cambios.

> [!NOTE]
> Copilot CLI comprime el contexto automáticamente cuando se llena la ventana. Usa `/compact` si quieres elegir el momento. Usa `/clear` o `/new` para cambiar a una tarea que no esté relacionada y empezar una conversación nueva.

## Elegir un modelo

Cada modelo tiene sus puntos fuertes y cada desarrollador sus preferencias. Copilot CLI permite mostrar los modelos disponibles y seleccionar uno.

1. Escribe:

   ```plaintext
   /model
   ```

2. Explora los modelos disponibles y la información de uso.
3. Mantén el modelo actual, selecciona otro o pulsa <kbd>Esc</kbd> para cerrar la lista.

## Compartir una sesión

Compartir el trabajo y lo aprendido ayuda a todo el equipo a mejorar el uso de las herramientas de IA. El comando `/share` permite exportar una sesión a un archivo Markdown o HTML, crear un enlace para compartirla o publicar un gist de GitHub.

1. Escribe `/help` y revisa las opciones de `/share` en la versión instalada.
2. Si quieres compartir esta sesión, selecciona el destino que mejor se adapte a tus necesidades, por ejemplo `/share file` para exportar un archivo Markdown local.
3. Revisa el contenido exportado antes de enviárselo a alguien o publicarlo. Las exportaciones de sesiones pueden incluir prompts, respuestas y detalles del proyecto.

Publicar un enlace o gist es opcional. No publiques contenido del repositorio o de la conversación que el equipo no quiera compartir.

## Opcional: ampliar el trabajo o delegarlo

Ya has completado el taller principal. Copilot CLI también ofrece comandos para tareas más amplias:

- `/fleet` puede repartir subtareas independientes entre subagentes y ejecutarlas en paralelo.
- `/worktree` puede crear un árbol de trabajo de Git aislado para otra tarea.
- `/delegate` puede enviar una tarea al agente de GitHub Copilot en la nube, que trabaja de forma asíncrona y puede abrir una solicitud de incorporación de cambios.

Estos comandos son opcionales porque pueden crear árboles de trabajo adicionales o trabajo remoto. Antes de probarlos, inicia una tarea nueva y bien delimitada; después, revisa el resultado mediante tu proceso habitual. Si quieres profundizar en el trabajo asíncrono de agentes, continúa con el [taller sobre el agente en la nube][cloud-workshop].

## Resumen y próximos pasos

Los comandos de barra de Copilot CLI te permiten configurarlo, compartir sesiones y ver qué ocurre en segundo plano. En esta lección:

- has usado `/context` y `/compact` para explorar cómo administra Copilot el contexto.
- has usado `/model` para explorar los modelos disponibles.
- has aprendido cómo exportar o compartir una sesión con `/share`.
- has explorado comandos opcionales para trabajar en paralelo, usar árboles de trabajo y delegar tareas al agente en la nube.

Copilot CLI dispone de muchos más comandos de barra. Para cerrar el recorrido, [repasarás lo aprendido][next-lesson] y verás cómo seguir aprendiendo.

## Recursos

- [Referencia de comandos de Copilot CLI][cli-reference]
- [Administración del contexto en Copilot CLI][context-management]
- [Acerca del agente de GitHub Copilot en la nube][about-cloud-agent]

[previous-lesson]: ../8-create-pull-request/
[next-lesson]: ../10-review/
[cli-reference]: https://docs.github.com/copilot/reference/copilot-cli-reference/cli-command-reference
[context-management]: https://docs.github.com/copilot/concepts/agents/copilot-cli/context-management
[about-cloud-agent]: https://docs.github.com/copilot/concepts/agents/cloud-agent/about-cloud-agent
[cloud-workshop]: ../../cloud/
