---
title: "Lección 9 - Comandos de barra de GitHub Copilot CLI"
description: "Explora los comandos de barra para gestionar el contexto, seleccionar modelos, compartir sesiones y delegar tareas opcionalmente al agente en la nube."
authors:
  - geektrainer
lastUpdated: 2026-09-18
---

Como cualquier herramienta de CLI que se precie, GitHub Copilot CLI incluye muchos comandos de barra para interactuar con ella. Estos comandos ofrecen funciones avanzadas, información interna y opciones de configuración adicionales. Ya has usado comandos como `/diff`, `/mcp`, `/skills`, `/agent` y `/pr`. Ahora explorarás otros comandos útiles.

En esta lección, vas a:

- usar `/context` y `/compact` para explorar cómo gestiona Copilot el contexto de la conversación;
- usar `/model` para explorar los modelos disponibles;
- aprender cómo `/share` puede exportar o compartir una sesión;
- explorar comandos opcionales para trabajar en paralelo, usar árboles de trabajo y delegar tareas al agente en la nube.

## Escenario

Ya has completado el flujo de trabajo principal de la CLI. Ahora explorarás otras funciones: gestionar el contexto, cambiar de modelo, compartir sesiones y delegar tareas, si quieres, al [agente de GitHub Copilot en la nube][about-cloud-agent].

## Explorar el contexto de Copilot CLI

Al trabajar en tareas grandes o complejas, puede que alcances el límite de contexto del modelo. Copilot CLI compacta automáticamente la conversación cuando hace falta, y puedes inspeccionar o compactar el contexto con comandos de barra.

1. Vuelve a tu Codespace e inicia Copilot CLI desde la raíz del repositorio si aún no está abierto.
2. Escribe:

   ```plaintext
   /context
   ```

3. Fíjate en el modelo, el uso actual de tokens y cómo se distribuye el contexto entre las instrucciones del sistema, las herramientas, los mensajes y el espacio disponible.
4. Compacta la conversación:

   ```plaintext
   /compact
   ```

5. Escribe `/context` de nuevo y compara el resultado. Si la conversación ya es pequeña, puede que no haya cambiado mucho.

> [!NOTE]
> Copilot CLI compacta automáticamente el contexto cuando se llena la ventana. Usa `/compact` si quieres elegir el momento. Usa `/clear` o `/new` cuando vayas a pasar a una tarea distinta y quieras empezar una conversación nueva.

## Elegir un modelo

Los distintos modelos tienen puntos fuertes diferentes y cada desarrollador tiene sus preferencias. Copilot CLI permite mostrar y seleccionar el modelo que quieres usar.

1. Escribe:

   ```plaintext
   /model
   ```

2. Explora los modelos disponibles y la información sobre su uso.
3. Mantén el modelo actual, selecciona otro o pulsa <kbd>Esc</kbd> para cerrar la lista.

## Compartir una sesión

Colaborar y compartir lo aprendido ayuda a todo el equipo a mejorar el uso de las herramientas de IA. El comando `/share` puede exportar una sesión a un archivo Markdown o HTML, crear un enlace para compartir o publicar un gist de GitHub.

1. Escribe `/help` y revisa las opciones de `/share` en la versión que tienes instalada.
2. Si quieres compartir la sesión, elige el destino adecuado; por ejemplo, `/share file` para exportar un archivo Markdown local.
3. Revisa el contenido exportado antes de enviarlo o publicarlo. Las exportaciones pueden incluir prompts, respuestas y detalles del proyecto.

Publicar un enlace o un gist es opcional. No publiques contenido del repositorio o de la conversación que el equipo no tenga previsto compartir.

## Opcional: ampliar el trabajo o delegarlo

Ya has completado el taller principal. Copilot CLI también ofrece comandos para tareas más amplias:

- `/fleet` puede dividir tareas independientes entre subagentes y ejecutarlas en paralelo.
- `/worktree` puede crear un árbol de trabajo de Git aislado para otra tarea.
- `/delegate` puede enviar una tarea al agente de GitHub Copilot en la nube, que trabaja de forma asíncrona y puede abrir una solicitud de incorporación de cambios.

Estos comandos son opcionales porque pueden crear árboles de trabajo adicionales o trabajo remoto. Antes de probarlos, inicia una tarea nueva y bien definida, y revisa el resultado con el flujo de trabajo habitual. Si quieres profundizar en el trabajo asíncrono con agentes, continúa con el [taller sobre el agente en la nube][cloud-workshop].

## Resumen y siguientes pasos

Los comandos de barra de Copilot CLI te permiten configurarlo, compartir sesiones y consultar lo que ocurre internamente. En esta lección, has:

- usado `/context` y `/compact` para explorar cómo gestiona Copilot el contexto de la conversación;
- usado `/model` para explorar los modelos disponibles;
- aprendido cómo `/share` puede exportar o compartir una sesión;
- explorado comandos opcionales para trabajar en paralelo, usar árboles de trabajo y delegar tareas al agente en la nube.

Copilot CLI ofrece más comandos de barra y funciones por explorar. Para terminar el recorrido, [repasarás lo aprendido][next-lesson] y descubrirás los siguientes pasos.

## Recursos

- [Referencia de comandos de Copilot CLI][cli-reference]
- [Gestión del contexto en Copilot CLI][context-management]
- [Acerca del agente de GitHub Copilot en la nube][about-cloud-agent]

[previous-lesson]: ../8-create-pull-request/
[next-lesson]: ../10-review/
[cli-reference]: https://docs.github.com/copilot/reference/copilot-cli-reference/cli-command-reference
[context-management]: https://docs.github.com/copilot/concepts/agents/copilot-cli/context-management
[about-cloud-agent]: https://docs.github.com/copilot/concepts/agents/cloud-agent/about-cloud-agent
[cloud-workshop]: ../../cloud/
