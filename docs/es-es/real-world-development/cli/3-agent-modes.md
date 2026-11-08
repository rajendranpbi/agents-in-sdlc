---
title: "Lección 3 - Modos de agente: Plan y Autopilot"
description: "Usa Plan para acordar un enfoque, Autopilot para crear el filtrado a partir de una incidencia e Interactive para revisar el resultado."
authors:
  - geektrainer
lastUpdated: 2026-09-18
---

Empezamos por añadir una pequeña funcionalidad al proyecto, pero los cambios más amplios requieren un proceso más sólido. GitHub Copilot CLI está diseñado para adaptarse al flujo de trabajo de una organización y ayudar a desarrollar las funcionalidades adecuadas de la forma apropiada. Esta es la primera de varias lecciones en las que seguirás un proceso habitual de desarrollo con agentes: partirás de una incidencia para crear una funcionalidad, comprobarás que el código es válido y que la funcionalidad se comporta como esperas, y finalmente la fusionarás en el proyecto.

> [!NOTE]
> A medida que avances en el flujo de trabajo de la funcionalidad, usarás la misma conversación y rama. Normalmente se usarían distintas ramas o PR para los diferentes tipos de archivos, pero en este taller agruparemos el trabajo para centrarnos en los conceptos principales.

En esta lección, vas a:

- iniciar una conversación nueva de Copilot a partir de una incidencia de GitHub;
- definir los requisitos en el modo Plan;
- implementar la funcionalidad con el modo Autopilot;
- revisar el código;
- comprobar manualmente la funcionalidad en un navegador mediante un puerto reenviado.

Mientras sigues trabajando en esta funcionalidad, actualizarás las instrucciones del repositorio, personalizarás la habilidad existente `quality-checks`, añadirás validación con MCP, crearás un agente de QA y abrirás la PR de la funcionalidad.

## Escenario

El catálogo de Tailspin Toys está creciendo y los visitantes necesitan filtrar los juegos por categoría y editorial. La incidencia del backlog describe la funcionalidad, pero es necesario acordar algunos detalles, como la combinación de categorías, antes de escribir código. Usarás el modo Plan para resolver esas decisiones y, después, autorizarás una implementación acotada con Autopilot.

## Contexto

La incorporación de agentes de programación con IA al flujo de desarrollo no cambia los fundamentos. De hecho, los hace aún más importantes. La mayoría de los desarrolladores siguen un proceso parecido a este:

1. Partir de una incidencia registrada que describa el trabajo.
2. Planificar lo que se debe crear.
3. Desarrollar y revisar el código.
4. Ejecutar las pruebas para validar el código.
5. Validar manualmente la nueva funcionalidad.
6. Crear una solicitud de incorporación de cambios (PR).
7. Cuando se haya revisado el código y se hayan superado los procesos de integración continua, fusionarlo en el proyecto.

> [!NOTE]
> Los detalles concretos varían según el equipo y la organización, pero la mayoría de los procesos siguen una estructura parecida a la anterior.

Al seguir este enfoque habitual, te aseguras de que el código generado por IA cumple los requisitos y pasa por el mismo proceso de revisión que el código escrito manualmente.

## Modos de conversación

El **modo de conversación** controla el nivel de autonomía del agente. Pulsa <kbd>Shift</kbd>+<kbd>Tab</kbd> para cambiar de modo:

- **Interactive**: trabajas con el agente. Este propone cambios y espera tu respuesta antes de continuar.
- **Plan**: el agente prepara primero un plan y no puede editar archivos del proyecto.
- **Autopilot**: el agente trabaja de forma autónoma: escribe código, ejecuta pruebas e itera hasta considerar que la tarea está terminada.

Empieza en el modo Plan, revisa el plan y, después, usa Autopilot para implementarlo.

## Partir de la incidencia

Antes de empezar a trabajar en el filtrado, vuelve a tu Codespace y comprueba que el repositorio y el terminal estén preparados.

1. Vuelve al Codespace. Si está detenido, reinícialo antes de continuar.
2. Confirma que se ha fusionado la PR de las valoraciones con estrellas.
3. Si el terminal no está abierto, pulsa <kbd>Control</kbd>+<kbd>\`</kbd> (Mac) o <kbd>Ctrl</kbd>+<kbd>\`</kbd> (Windows/Linux).
4. Actualiza `main` y crea una rama para el filtrado:

   ```bash
   git checkout main
   git pull --ff-only
   git checkout -b game-filters-cli
   ```

5. Inicia Copilot CLI:

   ```bash
   copilot --yolo
   ```

6. Pulsa <kbd>Tab</kbd> dos veces para abrir la pestaña **Issues**.
7. Pulsa <kbd>A</kbd> para mostrar todas las incidencias.
8. Usa las teclas de dirección para resaltar la incidencia titulada **Allow users to filter games by category and publisher**.
9. Pulsa <kbd>C</kbd> para añadir la incidencia al prompt y volver a la pestaña **Session**.

Observa que el prompt ahora empieza por `#7` (o por un número parecido). El carácter `#` permite añadir al contexto una incidencia o una solicitud de incorporación de cambios (PR) de GitHub.

## Planificar la funcionalidad de filtrado

La planificación permite definir el enfoque para implementar una funcionalidad o realizar una tarea antes de delegarla a Copilot. Siempre es recomendable dedicar tiempo a planificar las tareas complejas. Cambia al modo Plan y pide a Copilot que prepare el plan.

1. Pulsa <kbd>Shift</kbd>+<kbd>Tab</kbd> para cambiar al modo Plan. Confirma que el indicador debajo del prompt muestra **Plan**.
2. Después de la referencia a la incidencia que añadiste en el paso anterior, escribe el prompt siguiente:

   ```plaintext
   Create a plan for implementing this feature.
   ```

Copilot empezará a preparar el plan. Primero explorará el proyecto y, después, determinará el mejor enfoque.

3. Durante el proceso, Copilot puede hacerte preguntas sobre el funcionamiento del filtrado. Respóndelas según tus preferencias. No hay respuestas incorrectas.
4. Cuando termine el plan, pulsa <kbd>Control</kbd>+<kbd>E</kbd> (Mac) o <kbd>Ctrl</kbd>+<kbd>E</kbd> (Windows/Linux) para expandirlo.
5. Desplázate por el plan para revisarlo.
6. Pide a Copilot que revise cualquier parte del plan que no se ajuste a tus decisiones.

## Aprobar Autopilot

Ya has redactado y revisado el plan. Ahora toca implementarlo. Deja que Copilot trabaje con el modo Autopilot.

Autopilot permite que Copilot itere sobre el problema hasta que considere que lo ha resuelto.

1. Selecciona **Accept plan and build on autopilot (recommended)** o la opción equivalente de tu versión.
2. Confirma que el indicador debajo del prompt muestra **Autopilot**.
3. Observa cómo Copilot sigue el plan acordado, genera código y ejecuta pruebas.

> [!NOTE]
> Al aprobar el plan, la implementación puede empezar inmediatamente, así que revísalo antes. Si Copilot informa de dependencias ausentes o de un conflicto de puerto, resuelve el problema de configuración antes de dar por terminadas las comprobaciones.

## Revisar y comprobar la implementación

Revisa el código generado antes de fusionarlo, igual que harías con cualquier otro cambio. También iniciaremos el sitio para comprobar que todo funciona.

1. Pulsa <kbd>Shift</kbd>+<kbd>Tab</kbd> para cambiar al modo Interactive. Confirma que el indicador ya no muestra **Plan** ni **Autopilot**.
2. Escribe `/diff` e inspecciona la implementación del filtrado y las pruebas.
3. Compara el resultado con la incidencia y las decisiones que tomaste durante la planificación.
4. Cuando hayas revisado el código, pulsa <kbd>Esc</kbd> para salir de la vista de diferencias.
5. Revisa los resultados de las comprobaciones del proyecto y pide a Copilot que resuelva cualquier error.

## Explorar la nueva funcionalidad

El código parece correcto, pero ¿se ejecuta? Inicia la aplicación como antes y abre el sitio a través del puerto reenviado por Codespaces.

1. Pide a Copilot que inicie la aplicación:

   ```plaintext
   Start the app so I can try the filtering feature in my browser. Tell me the URL and leave the server running.
   ```

2. Cuando Codespaces indique que el puerto `4321` está disponible, selecciona **Open in Browser**.
3. Prueba el filtrado por categoría, el filtrado por editorial y las combinaciones que acordaste en el plan.
4. Confirma que las opciones para restablecer los filtros y el comportamiento cuando no hay resultados coinciden con la incidencia y tus decisiones.
5. Vuelve al Codespace y pide a Copilot que detenga el servidor de desarrollo que inició.

## Resumen y siguientes pasos

Has usado distintos modos de conversación para crear y revisar una funcionalidad. En esta lección, has:

- iniciado una conversación nueva de Copilot a partir de una incidencia de GitHub;
- definido los requisitos en el modo Plan;
- implementado la funcionalidad con el modo Autopilot;
- revisado el código;
- comprobado manualmente la funcionalidad en un navegador mediante un puerto reenviado.

A continuación, profundizarás en cómo se genera el código y cómo conseguir que siga las prácticas documentadas mediante [instrucciones personalizadas][next-lesson].

## Recursos

- [Autopilot en GitHub Copilot CLI][autopilot]
- [Referencia de comandos de Copilot CLI][cli-reference]

[autopilot]: https://docs.github.com/copilot/concepts/agents/copilot-cli/autopilot
[cli-reference]: https://docs.github.com/copilot/reference/copilot-cli-reference/cli-command-reference
[previous-lesson]: ../2-add-star-rating/
[next-lesson]: ../4-custom-instructions/
