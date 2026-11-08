---
title: "Lección 8 - Crear y fusionar la PR de la funcionalidad"
description: "Revisa el filtrado, las instrucciones, la actualización de la habilidad, el perfil de garantía de calidad (QA) y las pruebas; después, crea una solicitud de incorporación de cambios (PR) y usa Agent Merge."
authors:
  - geektrainer
lastUpdated: 2026-09-18
---

La implementación del filtrado, las instrucciones actualizadas, la habilidad, el perfil de garantía de calidad (QA) y las pruebas están en una misma rama. Ahora toca revisarlo todo y abrir una solicitud de incorporación de cambios. Fusionaste tú mismo la solicitud de incorporación de cambios (PR) de las valoraciones con estrellas; esta vez dejarás que **Agent Merge** gestione el proceso.

> [!NOTE]
> Lo habitual sería separar la funcionalidad, las instrucciones, la habilidad y el agente de QA en varias PR. Para simplificar el taller, has mantenido todo el flujo de trabajo de filtrado y calidad en una conversación y una rama, y lo incluirás en esta PR.

En esta lección, vas a:

- aprender qué es Agent Merge y cómo automatiza el ciclo de fusión;
- inspeccionar el cambio completo de la funcionalidad y las pruebas de verificación;
- crear la PR del filtrado;
- habilitar Agent Merge solo después de la revisión y confirmar que se ha fusionado la PR.

## Escenario

Durante el flujo de trabajo de filtrado, has utilizado Copilot para planificar, implementar y verificar una funcionalidad. Ahora Tailspin Toys quiere automatizar las tareas restantes de la PR, pero mantener bajo el control del desarrollador la autorización para fusionarla.

## Presentación de Agent Merge

**Agent Merge** automatiza el trabajo restante para incorporar una solicitud de incorporación de cambios. Cuando lo habilitas, Copilot resuelve los bloqueos de la PR: corrige las comprobaciones de integración continua (CI) que fallan, responde a los comentarios de revisión y vuelve a basar la rama cuando sea necesario. Después, habilita la fusión automática de GitHub si el repositorio lo permite.

Hasta ahora has seleccionado tú **Merge pull request**. Agent Merge puede encargarse de esa tarea. Revisa el trabajo y confirma que está listo antes de habilitar Agent Merge.

## Usar Agent Merge para gestionar la PR

Con todo el código listo, revisarás los cambios, crearás la PR y dejarás que Agent Merge se encargue del resto del proceso.

1. Vuelve a tu Codespace.
2. Escribe `/agent` para abrir el cuadro de diálogo de agentes.
3. Selecciona **Default** en la lista y pulsa <kbd>Enter</kbd>.
4. Crea una PR con el comando `/pr create`.
5. Activa Agent Merge con `/pr agentmerge`.
6. Copilot supervisará el proceso de integración continua de la PR. Cuando todas las comprobaciones se superen, la fusionará.
7. Confirma que Copilot muestra un mensaje parecido a «PR #14 was squash-merged successfully».

> [!IMPORTANT]
> Agent Merge no elude las aprobaciones obligatorias, la protección de ramas, las colas de fusión, la configuración del repositorio ni los permisos que falten. Si se bloquea, revisa el motivo indicado y completa manualmente la fusión después de revisar los cambios, si la configuración del repositorio lo permite.

## Resumen y siguientes pasos

Has automatizado varios aspectos del proceso de desarrollo: la generación de código, las pruebas y la validación del código, y ahora el proceso de la solicitud de incorporación de cambios. En esta lección, has:

- aprendido qué es Agent Merge y cómo automatiza el ciclo de fusión;
- inspeccionado el cambio completo de la funcionalidad y las pruebas de verificación;
- creado la PR del filtrado;
- habilitado Agent Merge solo después de la revisión y confirmado que se ha fusionado la PR.

A continuación, [explorarás otros comandos de barra útiles de Copilot CLI][next-lesson] para gestionar el contexto, los modelos, el uso compartido y la delegación opcional al agente en la nube.

## Recursos

- [Gestionar solicitudes de incorporación de cambios con Copilot CLI][manage-prs]
- [Referencia de comandos de Copilot CLI][cli-reference]

[previous-lesson]: ../7-qa-agent/
[next-lesson]: ../9-cli-power-tools/
[manage-prs]: https://docs.github.com/copilot/how-tos/copilot-cli/use-copilot-cli/manage-pull-requests
[cli-reference]: https://docs.github.com/copilot/reference/copilot-cli-reference/cli-command-reference
