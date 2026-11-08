---
title: "Ejercicio 5 - Supervisar y gestionar agentes"
authors:
  - geektrainer
lastUpdated: 2026-06-30
---

| [← Ejercicio anterior: Agentes personalizados][previous-lesson] |
|:--|

Cuando usas el modo de agente de GitHub Copilot, trabaja de forma autónoma: explora el código, propone cambios, edita archivos y ejecuta comandos. Como trabaja en tiempo real en tu equipo, Copilot Chat en VS Code muestra cada llamada a herramientas y cada edición a medida que se realizan. Puedes revisar las diferencias en línea, aceptar o rechazar cambios concretos y orientar la conversación con prompts de seguimiento sin salir del editor.

En este ejercicio, vas a:

- supervisar en tiempo real el trabajo del agente de accesibilidad;
- revisar y aceptar las ediciones propuestas;
- guiar la sesión para añadir un modo claro junto al modo de alto contraste.

## Escenario

El agente personalizado de accesibilidad que usaste en el [ejercicio 4][previous-lesson] está implementando el modo de alto contraste. Mientras trabaja, el equipo de diseño solicita también un modo claro para quienes consideran que el tema predeterminado es demasiado oscuro. En lugar de empezar una conversación nueva, guiarás la sesión actual para ampliar el trabajo.

## Supervisar el progreso de Copilot

La vista Copilot Chat muestra cada paso del agente: los archivos que explora, los comandos que ejecuta y las ediciones que propone. Cada edición aparece en el chat con las diferencias y los controles **Keep** y **Undo**.

1. Vuelve a la vista Copilot Chat del ejercicio 4.
2. Mientras trabaja el agente, desplázate por el chat para ver cada llamada a herramientas. Verás lecturas y ediciones de archivos y los comandos de terminal que solicita ejecutar.
3. Cuando el agente proponga una edición, aparecerán las diferencias en línea:
   - Selecciona **Keep** para aceptar el cambio.
   - Selecciona **Undo** para revertirlo y permitir que el agente vuelva a intentarlo.
4. También puedes abrir el indicador **Files changed** en la parte superior del chat para ver todos los archivos modificados durante la sesión.

> [!NOTE]
> Si una llamada a herramientas solicita permiso, por ejemplo, para ejecutar un comando de terminal, el agente se detendrá y mostrará la opción **Allow**. Aprueba solo los comandos que consideres adecuados.

5. Espera a que el agente termine de implementar el modo de alto contraste. Deberías ver un mensaje final que resuma los cambios.

## Guiar al agente para añadir un modo claro

Ahora que el modo de alto contraste está implementado, ampliarás la misma conversación para añadir un modo claro. Como continuarás en la sesión actual, el agente conservará el contexto del trabajo anterior, incluidos los componentes modificados, el enfoque de estilo y el uso del almacenamiento local.

1. En el campo de Copilot Chat, envía el prompt de seguimiento siguiente:

   ```plaintext
   Nice work! Now add a light-mode toggle alongside the high-contrast toggle. Light mode should follow the same pattern — persist via local storage and be accessible from the same area of the UI. The user should be able to enable high-contrast and light mode independently.
   ```

2. El agente continuará desde el punto en que se quedó, explorará los cambios realizados y propondrá los cambios necesarios para el modo claro.
3. Revisa cada edición nueva a medida que aparezca, igual que hiciste con el modo de alto contraste. Usa **Keep** o **Undo** para cada diferencia.
4. Si el agente sigue un enfoque que no quieres —por ejemplo, intenta reorganizar la interfaz de alternancia de una forma que no te gusta—, deténlo con el botón **Stop** junto al campo de chat y envía un mensaje de seguimiento para aclarar lo que necesitas.

> [!TIP]
> Guiar una sesión en curso es más rápido que empezar de nuevo. Usa prompts de seguimiento para pedir ajustes («simplifica el CSS y usa variables CSS en lugar de duplicar selectores»), corregir el enfoque («no añadas un tercer control; integra el modo claro en el panel de configuración existente») o ampliar el alcance («añade también métodos abreviados de teclado para ambos controles»).

## Revisar los archivos modificados

Antes de confirmar el trabajo, revisa los archivos que ha modificado el agente.

1. Abre la vista **Source Control** en VS Code.
2. Revisa la lista completa de archivos modificados. Deberías ver cambios en los componentes Astro, los estilos y las pruebas relacionadas.
3. Abre algunos archivos modificados y revisa las diferencias. Confirma que se aplican las prácticas de accesibilidad del agente personalizado: atributos de Accessible Rich Internet Applications (ARIA), navegación con teclado, HTML semántico y persistencia mediante almacenamiento local.
4. Cuando estés conforme, prepara y confirma los cambios desde el panel Source Control. Los publicarás en [el siguiente ejercicio][next-lesson].

## Resumen y siguientes pasos

Has usado el modo de agente de Copilot Chat para supervisar y guiar una sesión en curso: has revisado cada edición, aceptado o rechazado cambios y enviado prompts de seguimiento para ampliar el trabajo sin perder el contexto.

En este ejercicio, has explorado:

- cómo supervisar el trabajo de un agente en tiempo real;
- cómo aceptar y rechazar ediciones individuales;
- cómo guiar una sesión en curso con prompts de seguimiento.

En el [siguiente ejercicio][next-lesson], publicarás el trabajo de accesibilidad en una solicitud de incorporación de cambios y revisarás los cambios del agente personalizado local que se han generado durante este taller.

## Recursos

- [Copilot Chat en VS Code][copilot-chat-vscode]
- [Usar el modo de agente][agent-mode]

---

| [← Ejercicio anterior: Agentes personalizados][previous-lesson] | [Siguiente ejercicio: Iterar sobre el trabajo de Copilot →][next-lesson] |
|:--|--:|

[previous-lesson]: ../4-custom-agents/
[next-lesson]: ../6-iterating/
[copilot-chat-vscode]: https://code.visualstudio.com/docs/copilot/chat/copilot-chat
[agent-mode]: https://code.visualstudio.com/docs/copilot/chat/chat-agent-mode
