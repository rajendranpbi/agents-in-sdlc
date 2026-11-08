---
title: "Lección 10 - Resumen y siguientes pasos"
description: "Repasa el flujo de trabajo de Copilot CLI, las dos solicitudes de incorporación de cambios, las personalizaciones reutilizables y otros recursos."
authors:
  - geektrainer
lastUpdated: 2026-09-29
---

Has usado GitHub Copilot CLI a lo largo de un flujo de trabajo continuo en Tailspin Toys. Has:

- preparado un Codespace, instalado Copilot CLI, explorado el proyecto y localizado la incidencia de filtrado preparada;
- añadido valoraciones con estrellas, revisado el resultado en un navegador mediante un puerto reenviado y fusionado manualmente tu primera solicitud de incorporación de cambios (PR);
- partido de la incidencia de filtrado, definido el enfoque en el modo Plan, desarrollado la funcionalidad en el modo Autopilot y revisado el resultado en el modo Interactive;
- guiado al agente con instrucciones personalizadas, personalizado la habilidad existente `quality-checks` y utilizado esa habilidad para ejecutar pruebas unitarias, lint y comprobaciones de tipos;
- añadido el servidor Model Context Protocol (MCP) de Playwright y usado el servidor para explorar el filtrado en un navegador real;
- creado y seleccionado un agente personalizado de garantía de calidad (QA) para evaluar los requisitos, la cobertura, los resultados de la habilidad y las pruebas del navegador;
- revisado el cambio completo de filtrado y autorizado Agent Merge para la PR correspondiente;
- explorado comandos de barra para el contexto, los modelos, el uso compartido y la delegación opcional al agente en la nube.

## Lo que has publicado

El taller incluye dos hitos de PR, cada uno en su propia rama creada a partir de `main` actualizado:

1. **Valoraciones con estrellas:** muestra en las tarjetas de juegos el valor existente de `starRating` y un estado explícito cuando no hay valoración.
2. **Filtrado y flujo de trabajo de calidad:** implementa el filtrado, actualiza las instrucciones y las aplica a la funcionalidad, personaliza el informe de `quality-checks`, crea un perfil de QA e incluye las pruebas correspondientes.

Desde la planificación del filtrado hasta la apertura de su PR, has usado la misma conversación y rama. Hemos agrupado el trabajo en una PR para simplificar el taller.

## Diferentes tipos de verificación

Has comprobado el código de varias formas: con pruebas automatizadas, inspeccionándolo en el navegador y dejando que Copilot explore el navegador mediante MCP. La habilidad `quality-checks` ha ejecutado las pruebas unitarias, el lint y las comprobaciones de tipos, y ha presentado los resultados en el formato nuevo. QA ha reunido esos resultados con una revisión de los requisitos y la cobertura de pruebas antes de la PR.

Las pruebas nuevas deben cubrir carencias reales; si una ejecución de QA determina que no hace falta añadir pruebas, también puede ser correcto. Antes de autorizar la fusión, revisa el código y las pruebas, y vuelve a recopilar las pruebas afectadas después de hacer cambios.

## Buenas prácticas

El contexto y las herramientas que proporcionas influyen en el trabajo de Copilot. En este taller, has actualizado instrucciones, personalizado una habilidad, creado un perfil de QA y configurado un servidor MCP. Reutiliza estas personalizaciones en otras conversaciones y ajústalas a medida que cambien las necesidades del equipo. Las instrucciones establecen estándares, las habilidades describen tareas repetibles, los agentes personalizados definen funciones especializadas y los servidores MCP conectan herramientas externas. Revisa los cambios y los resultados reales de las herramientas, no solo el resumen del agente.

Elige el **modo y el modelo** adecuados para la tarea. Usa **Plan** para definir el enfoque antes de empezar a desarrollar, **Interactive** para seguir de cerca los cambios específicos y **Autopilot** para las tareas acotadas. Elige un modelo más rápido para las ediciones rutinarias y uno más avanzado para el trabajo complejo.

El contexto sigue siendo tan importante como la infraestructura. Describir con claridad *qué* quieres crear, *por qué* y *cómo* influye de manera significativa en el resultado.

## Más posibilidades

Has completado el flujo de trabajo principal. Estas son algunas funciones más de la CLI que puedes explorar:

- `/review` para pedir al agente de revisión de código que analice los cambios;
- `/rubber-duck` para explicar un problema y obtener otra perspectiva;
- `/fleet` para coordinar tareas independientes en paralelo;
- `/worktree` para aislar una tarea distinta;
- `/delegate` para enviar una tarea al agente de GitHub Copilot en la nube.

## Siguientes pasos

La mejor forma de mejorar con cualquier herramienta es seguir utilizándola. Úsala en código de producción, en proyectos personales o para desarrollar esa pequeña aplicación que llevas años imaginando. Comparte lo que aprendas con el equipo y aprovecha lo que te enseñen los demás. Y, como siempre, explora la documentación.

Si quieres ampliar Tailspin Toys desde el terminal, continúa con la serie opcional [Foundry Backer Concierge][foundry]. Para comparar otros entornos, explora el [taller de VS Code][vscode], el [taller de la aplicación GitHub Copilot][app] o el [taller del agente de Copilot en la nube][cloud].

## Recursos

- [Acerca de GitHub Copilot CLI][about-cli]
- [Referencia de comandos de Copilot CLI][cli-reference]
- [Personalizar Copilot CLI][customize-cli]
- [Gestionar solicitudes de incorporación de cambios con Copilot CLI][manage-prs]

[previous-lesson]: ../9-cli-power-tools/
[foundry]: ../8-foundry-agent/
[vscode]: ../../vscode/
[app]: ../../app/
[cloud]: ../../cloud/
[about-cli]: https://docs.github.com/copilot/concepts/agents/about-copilot-cli
[cli-reference]: https://docs.github.com/copilot/reference/copilot-cli-reference/cli-command-reference
[customize-cli]: https://docs.github.com/copilot/how-tos/copilot-cli/customize-copilot
[manage-prs]: https://docs.github.com/copilot/how-tos/copilot-cli/use-copilot-cli/manage-pull-requests
