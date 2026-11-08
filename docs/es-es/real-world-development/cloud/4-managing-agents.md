---
title: "Ejercicio 4 - Supervisar y gestionar agentes"
authors:
  - geektrainer
lastUpdated: 2026-06-30
---

En los dos ejercicios anteriores has pedido al agente de GitHub Copilot en la nube que realice tres tareas distintas para mejorar la experiencia y añadir funcionalidades. Aunque el agente trabaja de forma asíncrona y autónoma, es importante supervisar estas tareas.

Hay varias herramientas para gestionar las tareas que se asignan al agente en la nube, incluida [la página de agentes][agents-page] de GitHub.com. Desde este panel puedes ver todas las tareas de los agentes que tienen solicitudes de incorporación de cambios abiertas, consultar las operaciones realizadas y guiar las sesiones en curso.

En esta lección, vas a:

- explorar la página de agentes para supervisar las tareas del agente en la nube;
- guiar una sesión en curso para solicitar una funcionalidad adicional.

## Escenario

Después de asignar al agente la tarea de crear un modo de alto contraste, el equipo decide añadir también un modo claro. Como el trabajo ya incluye cambios de estilo y una opción para alternar modos, parece lógico incorporar esta funcionalidad. Guiarás al agente para que añada el modo claro además del modo de alto contraste.

## Revisar las tareas del agente de GitHub Copilot en la nube

Consulta el estado actual de todas las tareas asignadas al agente de GitHub Copilot en la nube.

1. Ve a la [página de agentes de Copilot](https://github.com/copilot/agents).
2. Observa la lista de tareas en el panel principal y en el panel de la izquierda. Deberías ver las tareas que has asignado a Copilot, entre ellas:
   - actualizar la documentación del código;
   - generar interfaces de programación de aplicaciones (API) para modificar juegos;
   - añadir un modo de alto contraste al sitio web.
3. Selecciona una de las tareas en curso y revisa las operaciones realizadas por Copilot. Pueden incluir:
   - obtener el código del repositorio;
   - crear el entorno de trabajo de Copilot;
   - configurar servidores MCP;
   - realizar distintas tareas para completar el trabajo asignado.

> [!NOTE]
> Los pasos exactos que aparecen dependen del estado del trabajo de Copilot y del enfoque que haya seguido.

4. Fíjate también en el panel de la solicitud de incorporación de cambios (PR) de la derecha. Desde allí puedes consultar la PR y los archivos modificados para seguir el progreso.

## Guiar al agente en la nube

Ya has visto las tareas activas. Ahora pedirás a Copilot que añada el modo claro mientras trabaja en el modo de alto contraste.

1. Selecciona la sesión relacionada con el modo de alto contraste. El título exacto dependerá del nombre que haya elegido Copilot y del estado del trabajo.

   ![Sesión de accesibilidad en Mission Control](../../../_images/ex6-accessibility-session.png)

2. Observa la sesión durante unos minutos, hasta que indique que ha terminado la configuración y ha empezado a trabajar. Verás mensajes parecidos a los siguientes.
3. En el cuadro de diálogo **Steer active session while Copilot is working**, escribe el prompt siguiente:

   ```plaintext
   While we are working on a high contrast mode, let's also add a light mode. There should be a switch for this mode as well where users can select their desired display mode.
   ```

   ![Captura de la tarea del agente en la nube en la página de agentes, con el cuadro de diálogo para guiar la sesión activa resaltado](../../../_images/ex6-steer-cloud-agent-task.png)

4. Pulsa <kbd>Enter</kbd> para enviar el prompt.
5. Observa cómo Copilot confirma que lo ha recibido y lo incorpora al trabajo.

## Dejar que Copilot trabaje

Copilot continuará con la tarea actualizada. Incorporará la nueva solicitud cuando termine el paso que estaba realizando en el momento en que enviaste el mensaje.

El proceso tardará varios minutos. Puedes aprovechar el tiempo para repasar lo que has aprendido y explorado hasta ahora.

## Resumen y siguientes pasos

Has explorado la página de agentes de Copilot, el centro de supervisión y orientación de las tareas del agente de GitHub Copilot en la nube. Puedes realizar un seguimiento de las tareas activas y completadas, revisar el trabajo y redirigir las sesiones en curso para ajustar el alcance o proporcionar más indicaciones.

Has explorado estos aspectos:

- usar Mission Control y la página de agentes para supervisar las tareas del agente en la nube;
- redirigir una sesión en curso para pedir una funcionalidad adicional.

Mientras Copilot termina de trabajar en las funciones de accesibilidad, puedes [iterar sobre las solicitudes de incorporación de cambios que ha creado][next-lesson].

## Recursos

- [Página de agentes de Copilot][agents-page]
- [Acerca de los agentes personalizados][custom-agents]

---

| [← Ejercicio anterior: Agentes personalizados][previous-lesson] | [Siguiente ejercicio: Iterar sobre el trabajo de Copilot →][next-lesson] |
|:--|--:|

[previous-lesson]: ../3-custom-agents/
[next-lesson]: ../5-iterating/
[agents-page]: https://github.blog/changelog/2025-07-03-agents-page-for-copilot-coding-agent-in-public-preview
[custom-agents]: https://docs.github.com/copilot/concepts/agents/cloud-agent/about-custom-agents
