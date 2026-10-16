---
title: "Ejercicio 4: agentes personalizados"
authors:
  - geektrainer
lastUpdated: 2026-06-30
---

## ¿Qué son los agentes personalizados?

Los [agentes personalizados][custom-agents-concept] de GitHub Copilot permiten crear asistentes de IA especializados para determinadas tareas o ámbitos del flujo de desarrollo. Al definirlos mediante archivos Markdown en la carpeta `.github/agents` del repositorio, puedes proporcionar a Copilot instrucciones específicas, buenas prácticas, patrones de programación y conocimientos del ámbito para que realice determinados tipos de trabajo con más eficacia. Los equipos pueden codificar su experiencia en agentes reutilizables: por ejemplo, uno de accesibilidad que aplique las directrices de [WCAG][wcag], uno de seguridad que siga prácticas de programación segura o uno de pruebas que mantenga patrones coherentes.

Los agentes personalizados se definen mediante archivos Markdown en la carpeta `.github/agents` del proyecto o de forma global en `~/.copilot/agents`. Cada archivo tiene metadatos YAML con al menos `name` y `description`, seguidos de un prompt Markdown que define el comportamiento, los conocimientos y las instrucciones del agente.

### Comparación entre agentes personalizados y habilidades de agente

Los agentes personalizados y las [habilidades de agente][agent-skills-concept] tienen algunos puntos en común. Ambos se definen principalmente mediante archivos Markdown y explican a la IA cómo realizar tareas. La forma más sencilla de distinguirlos es esta: un **agente personalizado** es quien realiza el trabajo; las **habilidades** son las herramientas.

Los agentes personalizados tienen su propia ventana de contexto y coordinan habilidades (e incluso otros agentes) mientras trabajan. En este laboratorio, el agente personalizado de accesibilidad revisa y actualiza el sitio de acuerdo con las directrices de accesibilidad. Para ello, podría usar habilidades como una que administre pruebas o el flujo de trabajo de una solicitud de incorporación de cambios.

> [!NOTE]
> No existe una única forma correcta de crear un agente personalizado. Como con cualquier herramienta de IA, prueba y repite el proceso hasta encontrar lo que mejor se adapte al entorno y a la situación.

[custom-agents-concept]: https://docs.github.com/copilot/concepts/agents/cloud-agent/about-custom-agents
[agent-skills-concept]: https://docs.github.com/copilot/concepts/agents/about-agent-skills
[wcag]: https://www.w3.org/WAI/standards-guidelines/wcag/

## Escenario

Tailspin Toys se compromete a que todo el mundo pueda usar su plataforma de financiación colectiva, con independencia de sus capacidades visuales o preferencias. Algunos comentarios recientes indican que a ciertos usuarios les cuesta leer el tema oscuro actual porque hay poco contraste entre el texto y el fondo. Para resolver este problema de accesibilidad, el equipo de diseño ha pedido implementar un modo de alto contraste que puedan activar y desactivar los usuarios.

Como la accesibilidad es fundamental, quieres implementar esta funcionalidad lo antes posible. Para ello, usarás un agente personalizado.

En este ejercicio:

- revisarás un agente personalizado de accesibilidad.
- usarás el agente de accesibilidad en Copilot Chat para implementar el modo de alto contraste.

## Revisar el agente personalizado de accesibilidad

Ya se ha creado un agente personalizado de accesibilidad. Revisa su contenido para entender cómo orientará a Copilot.

Vuelve a tu Codespace, abre un terminal y cámbiate a una rama nueva basada en `main` para el trabajo de accesibilidad. Mantendrás separada la PR de filtrado del ejercicio 3:

```bash
git checkout main
git pull
git checkout -b accessibility-vscode
```

1. Abre `.github/agents/accessibility.md`.
2. Fíjate en los metadatos YAML con los campos `name` y `description`.

> [!CAUTION]
> Los campos `name` y `description` de los metadatos son obligatorios para los agentes personalizados.

3. Lee las secciones siguientes, que describen:
   - las responsabilidades principales al generar código para un sitio accesible.
   - las buenas prácticas de accesibilidad.
   - ejemplos de HTML, CSS y JavaScript.
   - errores habituales.

## Usar el agente personalizado en Copilot Chat

VS Code muestra los agentes personalizados definidos en `.github/agents` en el menú desplegable de agentes, en la parte inferior de la vista Copilot Chat. Puedes seleccionar un agente personalizado para limitar la sesión de chat a sus instrucciones y herramientas.

> [!TIP]
> **Abrir Copilot Chat**
>
> Antes de empezar los ejercicios siguientes, vuelve a tu Codespace, abre el panel de Copilot Chat y selecciona **New Chat** para iniciar una conversación limpia. El modo y el modelo pueden variar según el ejercicio; cada paso lo indicará cuando corresponda.

1. Si aún no está seleccionado, elige **Agent** en el menú de agentes de la vista Chat.

   ![Selector de agente en la vista Chat](../../../_images/shared-chat-mode-selector.png)

2. Selecciona el menú de agentes de la parte inferior de la vista Chat. Muestra el agente activo, que de forma predeterminada es **default**.
3. Selecciona **Accessibility agent** en la lista de agentes disponibles.
4. Envía este prompt al agente de accesibilidad:

   ```plaintext
   Add a high-contrast mode to the site. There should be a toggle for high contrast which the user can set, and the setting should persist across page reloads using local storage on the browser.
   ```

5. Copilot Chat explorará la base de código, propondrá cambios y editará los archivos del proyecto. Cada edición aparecerá en el chat con la ruta del archivo y las diferencias que puedes revisar.
6. A medida que se apliquen los cambios, se actualizará el indicador **Files changed** para mostrar los archivos modificados por el agente.

> [!NOTE]
> El proceso tardará varios minutos. Copilot modificará realmente el repositorio y, a medida que avance, editará archivos existentes, como los componentes Astro, el CSS y las pruebas relacionadas.

En el próximo ejercicio revisarás y dirigirás el trabajo en curso.

## Resumen y próximos pasos

En esta lección has explorado los [agentes personalizados][custom-agents] de GitHub Copilot, asistentes de IA especializados en determinadas tareas y ámbitos. Los agentes personalizados permiten codificar la experiencia y los estándares del equipo en componentes reutilizables que guían el trabajo de Copilot.

Has aprendido:

- cómo se definen los agentes personalizados.
- cómo usar un agente personalizado en el modo de agente de Copilot Chat.

A continuación, [supervisarás y dirigirás el trabajo del agente][next-lesson]: revisarás los cambios a medida que se apliquen y añadirás un botón para cambiar al modo claro en la misma sesión.

## Recursos

- [Acerca de los agentes personalizados][custom-agents]
- [Crear agentes personalizados en el IDE][creating-custom-agents-ide]
- [Agentes personalizados en VS Code][custom-agents-vscode]
- [Configuración de agentes personalizados][custom-agents-config]
- [Agentes personalizados de awesome-copilot][awesome-copilot-agents]

---

| [← Lección anterior: probar la funcionalidad con el servidor MCP de Playwright][previous-lesson] | [Lección siguiente: supervisar y administrar agentes →][next-lesson] |
|:--|--:|

[previous-lesson]: ../3-mcp/
[next-lesson]: ../5-managing-agents/
[custom-agents]: https://docs.github.com/copilot/concepts/agents/cloud-agent/about-custom-agents
[creating-custom-agents-ide]: https://docs.github.com/copilot/how-tos/use-copilot-agents/cloud-agent/create-custom-agents-in-your-ide
[custom-agents-vscode]: https://code.visualstudio.com/docs/copilot/customization/custom-agents
[custom-agents-config]: https://docs.github.com/copilot/reference/custom-agents-configuration
[awesome-copilot-agents]: https://github.com/github/awesome-copilot/tree/main/agents
