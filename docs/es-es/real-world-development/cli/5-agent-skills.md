---
title: "Lección 5 - Personalizar y usar una habilidad de comprobaciones de calidad"
description: "Explora la habilidad existente quality-checks, personaliza el formato de su informe y úsala para validar el filtrado."
authors:
  - geektrainer
lastUpdated: 2026-09-29
---

Desarrollar código implica mucho más que escribirlo. Ya has comprobado manualmente que funciona y has utilizado archivos de instrucciones para asegurar que sigue los estándares. ¿Qué ocurre con las pruebas, el lint y el resto de la integración continua (CI)?

Las **habilidades de agente** son adecuadas para estas tareas. Ayudan a Copilot a entender cómo realizar operaciones como estas.

En esta lección, vas a:

- explorar la habilidad existente `quality-checks`;
- personalizar el formato de sus resultados;
- volver a cargar la habilidad y ejecutarla.

## Escenario

Tailspin Toys usa la habilidad `quality-checks` para ejecutar pruebas unitarias, lint y comprobaciones de tipos. El equipo quiere mejorar el informe para que los resultados sean más fáciles de leer.

## Instrucciones, scripts y recursos

Las habilidades de agente agrupan instrucciones reutilizables para tareas, scripts ejecutables y recursos auxiliares que el agente carga cuando los necesita. En esencia, son carpetas que contienen un archivo Markdown llamado `SKILL.md`. El archivo incluye metadatos con un nombre y una descripción para definir la habilidad, una descripción general de sus funciones y directrices sobre cuándo usarla. La carpeta también puede incluir subcarpetas con scripts y otros recursos que la habilidad utiliza.

> [!NOTE]
> Las carpetas y los archivos adicionales son opcionales. La habilidad `quality-checks` de Tailspin Toys solo contiene `SKILL.md` porque utiliza los comandos existentes del proyecto.

Las habilidades pueden estar en la carpeta `.github/skills` de un proyecto para que el equipo las comparta y reutilice, o en la carpeta de habilidades del usuario, `~/.copilot/skills`.

## Explorar la habilidad

Explora la habilidad `quality-checks` que creó el equipo de Tailspin Toys para ejecutar pruebas unitarias, lint y comprobaciones de tipos.

1. Vuelve a tu Codespace. En el editor de Codespaces, abre `.github/skills/quality-checks/SKILL.md`.
2. Lee el `name` y la `description` de la parte superior. La descripción ayuda a Copilot a saber cuándo debe usar la habilidad.
3. Lee las instrucciones y fíjate en cómo guían a Copilot durante las pruebas y el lint.
4. Observa que todavía no hay una sección **Results output formatting**.

## Ejecutar la habilidad antes de cambiarla

Puedes invocar las habilidades directamente desde Copilot CLI o pedirlas en lenguaje natural. Pide a Copilot que ejecute las tres comprobaciones de la habilidad.

1. Vuelve a la conversación de filtrado en modo Interactive.
2. Usa el prompt siguiente:

   ```plaintext
   Run the quality-checks skill for unit tests, lint, and type checks.
   ```

3. Lee el informe final.

## Personalizar el informe

El informe debería indicar qué se ha ejecutado, si se ha superado y qué resultados han devuelto las herramientas. Actualiza la habilidad para que genere ese informe.

1. Vuelve a abrir `.github/skills/quality-checks/SKILL.md`.
2. Añade la sección siguiente al final del archivo:

   ```markdown
   ## Results output formatting

   Upon completion, report each command that ran and whether it passed, failed, or was blocked. Include test counts, durations, errors, warnings, and other metrics only when the tool reports them. Identify the next action for any failure or blocker, and never describe a skipped or incomplete check as passed.
   ```

3. El archivo se guardará automáticamente.

## Ejecutar la habilidad actualizada

Una vez realizados los cambios, comprueba el resultado. Copilot CLI puede volver a cargar las habilidades editadas sin reiniciar la conversación.

1. Escribe:

   ```plaintext
   /skills reload
   ```

2. Usa exactamente el mismo prompt de antes:

   ```plaintext
   Run the quality-checks skill for unit tests, lint, and type checks.
   ```

3. Lee el informe final y compáralo con el primero.

## Resumen y siguientes pasos

Has personalizado y utilizado una habilidad de agente existente. En esta lección, has:

- explorado la habilidad `quality-checks` para pruebas unitarias, lint y comprobaciones de tipos;
- personalizado el formato de sus resultados;
- vuelto a cargar y ejecutado la habilidad.

El cambio acompañará al filtrado en la PR de la funcionalidad. A continuación, permitirás que Copilot interactúe directamente con el sitio [mediante el servidor MCP de Playwright][next-lesson].

## Más ejemplos de habilidades

Estos ejemplos de la comunidad sirven de referencia; no son tareas adicionales:

- [Especificación de Agent Skills][skill-spec]
- [Flujo de trabajo de contribución: `make-repo-contribution`][contribution-example]
- [Documentos de requisitos: `prd`][prd-example]
- [Diagramas y script de exportación integrado: `drawio`][drawio-example]
- [Pruebas en navegador: `webapp-testing`][browser-example]

[previous-lesson]: ../4-custom-instructions/
[next-lesson]: ../6-mcp-playwright/
[skill-spec]: https://agentskills.io/specification
[contribution-example]: https://github.com/github/awesome-copilot/tree/main/skills/make-repo-contribution
[prd-example]: https://github.com/github/awesome-copilot/tree/main/skills/prd
[drawio-example]: https://github.com/github/awesome-copilot/tree/main/skills/drawio
[browser-example]: https://github.com/github/awesome-copilot/tree/main/skills/webapp-testing
