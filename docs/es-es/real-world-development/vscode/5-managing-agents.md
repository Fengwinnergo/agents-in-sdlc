---
title: "Ejercicio 5: supervisar y administrar agentes"
authors:
  - geektrainer
lastUpdated: 2026-06-30
---

| [← Lección anterior: agentes personalizados][previous-lesson] |
|:--|

Cuando trabajas con GitHub Copilot en el modo de agente, este explora la base de código, propone cambios, edita archivos y ejecuta comandos de forma autónoma. Como ese trabajo se realiza en tu equipo y en tiempo real, Copilot Chat en VS Code muestra cada llamada a una herramienta y cada edición mientras se producen. Puedes revisar las diferencias en línea, aceptar o rechazar cambios concretos y dirigir la conversación con prompts de seguimiento, sin salir del editor.

En este ejercicio:

- supervisarás en tiempo real el trabajo del agente de accesibilidad.
- revisarás y aceptarás los cambios propuestos en los archivos.
- dirigirás al agente durante la sesión para que añada un control para alternar al modo claro junto al de alto contraste.

## Escenario

El agente personalizado de accesibilidad que usaste en el [ejercicio 4][previous-lesson] está implementando el modo de alto contraste. Mientras trabaja, el equipo de diseño solicita además un control para cambiar al modo claro, para los usuarios a quienes les parezca demasiado oscuro el tema predeterminado. En lugar de empezar otra conversación, dirigirás la sesión existente para ampliar el trabajo.

## Supervisar el progreso de Copilot

La vista Copilot Chat muestra cada paso del agente: los archivos que explora, los comandos que ejecuta y los cambios que propone. Cada edición aparece en el chat con las diferencias y los controles **Keep** y **Undo**.

1. Vuelve a la vista Copilot Chat desde el ejercicio 4.
2. Mientras el agente trabaja, desplázate por el chat para ver sus llamadas a herramientas. Aparecerán lecturas y ediciones de archivos, así como comandos de terminal que solicite ejecutar.
3. Cuando el agente proponga un cambio, las diferencias aparecerán en línea:
   - selecciona **Keep** para aceptar el cambio.
   - selecciona **Undo** para revertirlo y permitir que el agente vuelva a intentarlo.
4. También puedes abrir el indicador **Files changed** en la parte superior del chat para consultar todos los archivos que haya modificado el agente durante la sesión.

> [!NOTE]
> Si una llamada a una herramienta requiere permiso, por ejemplo para ejecutar un comando de terminal, el agente se detendrá y mostrará el aviso **Allow**. Aprueba solo los comandos que consideres seguros.

5. Espera a que el agente termine de implementar el modo de alto contraste. Debería mostrar un mensaje final con un resumen de los cambios realizados.

## Pedir al agente que añada el modo claro

Ahora que ya está implementado el modo de alto contraste, ampliarás la misma conversación para añadir un botón de modo claro. Como sigues en la misma sesión, el agente conserva el contexto del trabajo anterior, incluidos los componentes, el enfoque de estilos y el patrón de almacenamiento local.

1. En el campo de Copilot Chat, envía este prompt de seguimiento:

   ```plaintext
   Nice work! Now add a light-mode toggle alongside the high-contrast toggle. Light mode should follow the same pattern — persist via local storage and be accessible from the same area of the UI. The user should be able to enable high-contrast and light mode independently.
   ```

2. El agente retomará el trabajo, explorará los cambios anteriores y propondrá las modificaciones necesarias para el modo claro.
3. Revisa cada cambio nuevo a medida que aparezca, como hiciste con el modo de alto contraste. Usa **Keep** o **Undo** para cada diferencia.
4. Si el agente sigue un enfoque que no quieres —por ejemplo, si intenta reorganizar los controles de un modo que no te convence—, puedes detenerlo con el botón **Stop** junto al campo de chat y enviarle un mensaje aclaratorio.

> [!TIP]
> Dirigir una sesión en curso es más rápido que empezar de nuevo. Usa prompts de seguimiento para pedir ajustes («simplifica el CSS; usa variables CSS en lugar de duplicar selectores»), corregir el rumbo («no añadas un tercer botón; integra el modo claro en el panel de configuración existente») o ampliar el alcance («añade también métodos abreviados de teclado para ambos controles»).

## Revisar los cambios

Antes de confirmar el trabajo, revisa rápidamente todos los archivos que haya modificado el agente.

1. Abre la vista **Source Control** de VS Code.
2. Revisa la lista completa de archivos modificados. Deberían aparecer cambios en los componentes Astro, los estilos y las pruebas relacionadas.
3. Abre algunos archivos y revisa las diferencias. Comprueba que se hayan aplicado los patrones de accesibilidad del agente personalizado: atributos ARIA, navegación con teclado, HTML semántico y persistencia mediante almacenamiento local.
4. Cuando estés satisfecho, prepara y confirma los cambios desde el panel Source Control. Los publicarás en la [lección siguiente][next-lesson].

## Resumen y próximos pasos

Has usado el modo de agente de Copilot Chat para supervisar y dirigir una sesión en curso: has revisado cada edición, aceptado o rechazado cambios y enviado prompts de seguimiento para ampliar el trabajo sin perder contexto.

En esta lección:

- has supervisado el trabajo de un agente en tiempo real.
- has aceptado y rechazado cambios individuales en archivos.
- has dirigido una sesión en curso con prompts de seguimiento.

En la [lección siguiente][next-lesson], publicarás el trabajo de accesibilidad en una solicitud de incorporación de cambios y revisarás los cambios del agente personalizado local que se hayan generado durante el laboratorio.

## Recursos

- [Copilot Chat en VS Code][copilot-chat-vscode]
- [Usar el modo de agente][agent-mode]

---

| [← Lección anterior: agentes personalizados][previous-lesson] | [Lección siguiente: iterar sobre el trabajo de Copilot →][next-lesson] |
|:--|--:|

[previous-lesson]: ../4-custom-agents/
[next-lesson]: ../6-iterating/
[copilot-chat-vscode]: https://code.visualstudio.com/docs/copilot/chat/copilot-chat
[agent-mode]: https://code.visualstudio.com/docs/copilot/chat/chat-agent-mode
