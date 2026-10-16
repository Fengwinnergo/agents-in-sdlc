---
title: "Ejercicio 3: agentes personalizados"
authors:
  - geektrainer
lastUpdated: 2026-06-30
---

## ¿Qué son los agentes personalizados?

Los [agentes personalizados][custom-agents-concept] de GitHub Copilot permiten crear asistentes de IA especializados para determinadas tareas o ámbitos del flujo de desarrollo. Al definir los agentes mediante archivos Markdown en la carpeta `.github/agents` del repositorio, puedes proporcionar instrucciones específicas, buenas prácticas, patrones de programación y conocimientos del ámbito para que Copilot realice determinados tipos de trabajo con más eficacia. Los equipos pueden codificar su experiencia en agentes reutilizables: por ejemplo, un agente de accesibilidad que aplique las directrices de [WCAG][wcag], uno de seguridad que siga prácticas de programación segura o uno de pruebas que mantenga patrones coherentes.

Los agentes personalizados se definen mediante archivos Markdown en la carpeta `.github/agents` del proyecto o de forma global en `~/.copilot/agents`. Cada archivo tiene metadatos YAML que incluyen como mínimo `name` y `description`, seguidos de un prompt Markdown que define el comportamiento, los conocimientos y las instrucciones del agente.

### Comparación entre agentes personalizados y habilidades de agente

Los agentes personalizados y las [habilidades de agente][agent-skills-concept] tienen algunos puntos en común. Ambos se definen principalmente con archivos Markdown y explican a la IA cómo realizar tareas. La forma más sencilla de distinguirlos es esta: un **agente personalizado** es quien realiza el trabajo; las **habilidades** son las herramientas.

Los agentes personalizados tienen su propia ventana de contexto y se encargan de coordinar habilidades (e incluso otros agentes) durante el trabajo. En este laboratorio, el agente personalizado de accesibilidad revisa y actualiza el sitio de acuerdo con las directrices de accesibilidad. Para ello, podría usar habilidades como una que ejecute y administre pruebas o gestione el flujo de trabajo de una solicitud de incorporación de cambios.

> [!NOTE]
> No existe una única forma correcta de crear un agente personalizado. Como con cualquier herramienta de IA, prueba y repite el proceso hasta encontrar lo que mejor se adapte a tu entorno y tus situaciones.

[custom-agents-concept]: https://docs.github.com/copilot/concepts/agents/cloud-agent/about-custom-agents
[agent-skills-concept]: https://docs.github.com/copilot/concepts/agents/about-agent-skills
[wcag]: https://www.w3.org/WAI/standards-guidelines/wcag/

Con los agentes personalizados, aprenderás a:

- definir agentes personalizados.
- asignar una tarea a un agente personalizado.

## Escenario

Tailspin Toys se compromete a que todo el mundo pueda usar su plataforma de financiación colectiva, con independencia de sus capacidades visuales o preferencias. Algunos comentarios recientes indican que a ciertos usuarios les cuesta leer el tema oscuro actual porque hay poco contraste entre el texto y el fondo. Para resolver este problema de accesibilidad, el equipo de diseño ha pedido implementar un modo de alto contraste que puedan activar y desactivar los usuarios.

Como la accesibilidad es fundamental, quieres implementar esta funcionalidad lo antes posible. Para ello, usarás un agente personalizado.

## Revisar el agente personalizado de accesibilidad

Ya se ha creado un agente personalizado de accesibilidad. Revisa su contenido para entender cómo orientará a Copilot.

Vuelve a tu Codespace y revisa el archivo del agente personalizado:

1. Abre `.github/agents/accessibility.md`.
2. Fíjate en los metadatos YAML con los campos `name` y `description`.

> [!CAUTION]
> Los campos `name` y `description` de los metadatos son obligatorios para los agentes personalizados.

3. Lee las secciones siguientes, que describen:
   - las responsabilidades principales al generar código para un sitio web accesible.
   - las buenas prácticas de accesibilidad.
   - ejemplos de HTML, CSS y JavaScript.
   - errores y problemas habituales.

## Crear y asignar una incidencia

El centro de agentes es el lugar principal para trabajar con agentes en tu entorno. Puedes asignar tareas al agente en la nube, supervisarlas y proporcionar orientación adicional. Para empezar, asigna a Copilot la tarea de crear el modo de alto contraste.

1. Ve a tu repositorio.
2. Selecciona la pestaña **Issues**.
3. Selecciona **New issue** para abrir el cuadro de diálogo de nueva incidencia.
4. Selecciona **Blank issue** para crear una incidencia nueva.
5. En **Title**, escribe `Add high contrast mode to website`.
6. En **Description**, escribe:

   ```plaintext
   We need a high contrast mode for the site. There should be a toggle for high contrast which the user can set. It should store the setting in local storage on the browser.
   ```

7. Selecciona **Create** para crear la incidencia.
8. En el lado derecho, selecciona **Assign to Copilot** para abrir el cuadro de diálogo de asignación.
9. Selecciona **Accessibility agent** en la lista de agentes personalizados.

    ![Asignación al agente en la nube con el agente personalizado de accesibilidad resaltado](../../../_images/ex5-select-custom-agent.png)

10. Selecciona **Assign**.
11. Copilot empezará a trabajar en la tarea en segundo plano.

## Resumen y próximos pasos

En este ejercicio has explorado los [agentes personalizados][custom-agents] de GitHub Copilot: asistentes de IA especializados en tareas y ámbitos determinados. Los agentes personalizados permiten codificar la experiencia y los estándares del equipo en componentes reutilizables que guían el trabajo de Copilot.

Has aprendido a:

- definir agentes personalizados.
- asignar una tarea a un agente personalizado.

Mientras Copilot implementa el modo de alto contraste, puedes [supervisar y dirigir la sesión del agente][next-lesson] desde el centro de agentes.

## Recursos

- [Acerca de los agentes personalizados][custom-agents]
- [Preparar la organización para usar agentes personalizados][org-custom-agents]
- [Preparar la empresa para usar agentes personalizados][enterprise-custom-agents]

---

| [← Lección anterior: agente de GitHub Copilot en la nube][previous-lesson] | [Lección siguiente: supervisar y administrar agentes →][next-lesson] |
|:--|--:|

[previous-lesson]: ../2-cloud-agent/
[next-lesson]: ../4-managing-agents/
[custom-agents]: https://docs.github.com/copilot/concepts/agents/cloud-agent/about-custom-agents
[org-custom-agents]: https://docs.github.com/copilot/how-tos/administer-copilot/manage-for-organization/prepare-for-custom-agents
[enterprise-custom-agents]: https://docs.github.com/copilot/how-tos/administer-copilot/manage-for-enterprise/manage-agents/prepare-for-custom-agents
