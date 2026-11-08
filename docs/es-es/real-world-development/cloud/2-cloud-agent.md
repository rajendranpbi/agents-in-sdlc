---
title: "Ejercicio 2 - Agente de GitHub Copilot en la nube"
authors:
  - geektrainer
lastUpdated: 2026-06-30
---

[← Ejercicio anterior: Instrucciones personalizadas][previous-lesson]

Pocas organizaciones, si es que hay alguna, están libres de deuda técnica. Puede tratarse de problemas de seguridad sin resolver, código heredado que necesita actualizarse o solicitudes de funcionalidad que llevan tiempo en el backlog por falta de recursos. El agente de GitHub Copilot en la nube puede actualizar código y añadir funcionalidades de forma autónoma. Cuando termina, crea una solicitud de incorporación de cambios (PR) en borrador para que un desarrollador la revise. Así puedes delegar tareas tediosas, acelerar el desarrollo y dedicarte a tareas de mayor alcance.

Con el agente de GitHub Copilot en la nube, explorarás cómo:

- personalizar el entorno de generación de código;
- asegurarte de que las operaciones se realizan de forma segura;
- crear incidencias con un alcance claro;
- asignar incidencias a Copilot.

## Escenarios

Tailspin Toys tiene deuda técnica que quiere resolver. Los contratistas que crearon la primera versión del sitio dejaron la documentación en mal estado: comprobarás que falta por completo. Como primer paso, el equipo quiere añadir comentarios de documentación de TypeScript (TSDoc) a todas las funciones exportadas de la aplicación.

Además, el equipo de diseño quiere mejorar la exploración de juegos. Quiere mostrar juegos relacionados —otros títulos de la misma categoría— en la página de detalles de cada juego para que los visitantes puedan seguir explorando. No necesita una implementación refinada; basta con una versión que permita probar la experiencia. Aunque esto bloquea el trabajo actual, hay incidencias de mayor prioridad.

Son ejemplos de tareas que pueden posponerse fácilmente y que se pueden asignar al agente de GitHub Copilot en la nube. El agente puede trabajar en segundo plano mientras el desarrollador se ocupa de otras tareas y, después, revisar el resultado.

## Presentación del agente de GitHub Copilot en la nube

El [agente de GitHub Copilot en la nube](https://docs.github.com/copilot/concepts/agents/cloud-agent/about-cloud-agent#overview-of-copilot-cloud-agent-formerly-copilot-coding-agent) puede realizar tareas en segundo plano, de forma parecida a un desarrollador. Puedes, por ejemplo, [asignar una incidencia de GitHub a Copilot](https://docs.github.com/copilot/how-tos/use-copilot-agents/cloud-agent/start-copilot-sessions). Cuando se la asignas, Copilot crea una solicitud de incorporación de cambios en borrador para hacer un seguimiento, configura un entorno y empieza a trabajar. Puedes consultar la sesión mientras sigue activa o después de que termine. Cuando esté lista para revisar la solución propuesta, Copilot te mencionará en la solicitud de incorporación de cambios.

## La importancia de acotar bien las instrucciones

GitHub Copilot no hace magia: no puedes saltarte el trabajo de análisis. Incluso las tareas aparentemente sencillas pueden ser complejas. Por eso, [define cuidadosamente el alcance de las tareas para el agente en la nube][cloud-agent-best-practices]. Trátalo como a un compañero de programación con IA: trabaja por etapas, aprende, experimenta y adapta el enfoque. Incorporar IA generativa no cambia los fundamentos del desarrollo de software.

## Instrucciones personalizadas de este repositorio

En ejercicios anteriores has visto cómo las instrucciones personalizadas guían a Copilot. Si ya has completado un ejercicio práctico sobre instrucciones personalizadas, habrás visto cómo funciona `.github/copilot-instructions.md`; si no, aprovecha para echarle un vistazo. Antes de asignar trabajo al agente de GitHub Copilot en la nube, revisa los archivos de instrucciones del repositorio en modo de solo lectura para poder reconocer su efecto más adelante.

Abre estos archivos en la interfaz web de GitHub o en un Codespace que ya esté en ejecución:

- `.github/copilot-instructions.md`: revisa la sección **Code standards**, especialmente las expectativas sobre las convenciones de TypeScript y los comentarios de documentación TSDoc.
- `.github/instructions/unit-tests.instructions.md`: fíjate en los metadatos `applyTo`, que limitan la aplicación de estas instrucciones a archivos `**/*.test.ts`.

Cuando asignes al agente en la nube la incidencia `Code lacks documentation` en la sección siguiente, observa si la solicitud de incorporación de cambios resultante incluye comentarios TSDoc, convenciones de TypeScript y encabezados de comentario. Estas directrices proceden de los archivos de instrucciones. Volverás a verlas cuando revises las PR en un ejercicio posterior.

## Configurar el entorno de desarrollo del agente

El desarrollo de código suele requerir un entorno específico y la ejecución de scripts para instalar herramientas y dependencias. Lo mismo se aplica al agente de Copilot, que trabaja de forma parecida a un ingeniero de software.

El agente en la nube usa [GitHub Actions][github-actions] como entorno de trabajo. Puedes personalizarlo mediante un [flujo de trabajo de configuración][setup-workflow] especial, definido en `.github/workflows/copilot-setup-steps.yml`, que se ejecuta antes de que el agente empiece a trabajar. Así puede disponer de las herramientas y dependencias necesarias. Este flujo ya está configurado para el taller y proporciona Node.js, las dependencias del proyecto, los navegadores para las pruebas de extremo a extremo y la base de datos SQLite migrada y precargada de la aplicación Astro:

```yaml
name: "Copilot Setup Steps"

# Allows you to test the setup steps from your repository's "Actions" tab
on: workflow_dispatch

env:
  ASTRO_TELEMETRY_DISABLED: "1"

jobs:
  copilot-setup-steps:
    runs-on: ubuntu-latest
    # Set the permissions to the lowest permissions possible needed for *your steps*. Copilot will be given its own token for its operations.
    permissions:
      # If you want to clone the repository as part of your setup steps, for example to install dependencies, you'll need the `contents: read` permission. If you don't clone the repository in your setup steps, Copilot will do this for you automatically after the steps complete.
      contents: read
    steps:
      - name: Checkout code
        uses: actions/checkout@v5

      # Frontend / app setup - Node.js (the whole app is now Astro + Drizzle/libSQL)
      - name: Set up Node.js
        uses: actions/setup-node@v6
        with:
          node-version: "lts/*"
          cache: "npm"
          cache-dependency-path: "./package-lock.json"

      - name: Install JavaScript dependencies
        run: npm ci

      - name: Install Playwright browsers
        run: npx playwright install --with-deps chromium

      # Migrate + seed the local SQLite database so builds and tests have data.
      - name: Set up the database
        run: npm run db:setup
```

El archivo se parece a cualquier otro flujo de trabajo de GitHub, pero tiene algunos aspectos importantes:

- Incluye un único trabajo llamado `copilot-setup-steps`. Se ejecuta en GitHub Actions antes de que Copilot empiece a trabajar en la solicitud de incorporación de cambios.
- El desencadenador `workflow_dispatch` permite ejecutar manualmente el flujo de trabajo desde la pestaña **Actions** del repositorio. Esto resulta útil para comprobar que funciona correctamente sin esperar a que Copilot lo ejecute.

## Añadir documentación

Aunque todo el mundo reconoce la importancia de la documentación, muchos proyectos tienen información desactualizada o carecen de ella. Esta deuda técnica reduce la productividad y dificulta el mantenimiento del código o la incorporación de nuevos desarrolladores. Copilot puede ayudar a crear documentación, así que esta es una buena incidencia para asignarle al agente de GitHub Copilot en la nube. Trabajará en segundo plano y podrás revisar su trabajo en un ejercicio posterior.

1. En una pestaña nueva del navegador, ve al repositorio en github.com.
2. Selecciona la pestaña **Issues**.
3. Selecciona **New issue** para abrir el cuadro de diálogo de una incidencia nueva.
4. Selecciona **Blank issue**.
5. En **Title**, escribe `Code lacks documentation`.
6. En **Description**, escribe:

    ```plaintext
    Our organization has a requirement that functions and methods include TSDoc doc comments where helpful. Unfortunately, recent updates haven't followed this standard. We need to update the existing code to ensure doc comments are included where they clarify behavior.
    ```

7. Selecciona **Create** para crear la incidencia.
8. A la derecha, selecciona **Assign to Copilot** para abrir el cuadro de diálogo de asignación.

  ![Asignar Copilot a una incidencia](../../../_images/shared-assign-copilot.png)

9. Selecciona **Assign**.
10. Selecciona la pestaña **Pull Requests**.
11. Abre la solicitud de incorporación de cambios que se acaba de generar. Su título será parecido a `[WIP]: Code lacks documentation`. Si aún no aparece en la lista, espera un momento y actualiza la página.
12. Al cabo de unos minutos, Copilot debería haber creado una lista de tareas.

> [!NOTE]
> La lista puede tardar varios minutos en aparecer en la PR. Copilot está preparando el entorno mediante el flujo de trabajo anterior, analizando el proyecto y determinando cómo abordar la tarea.

13. Revisa la lista y las tareas que se van a realizar.
14. Desplázate hacia abajo por la cronología de la PR y comprueba si Copilot ha indicado que ha empezado a trabajar en la incidencia.
15. Selecciona el botón **View session**.

  ![Vista de la sesión de Copilot](../../../_images/ex4-view-session.png)

> [!CAUTION]
> Puede que tengas que actualizar la página para ver el indicador actualizado.

16. Recorre la sesión en directo y observa cómo Copilot explora el código y comprende su estado, se detiene a analizar el enfoque y crea código.

El proceso puede tardar varios minutos. Una de las principales ventajas del agente en la nube es que puede trabajar de forma asíncrona y dejarte tiempo para otras tareas. Aprovecharemos esa característica para asignarle otra tarea y, mientras tanto, escribir código que añada funcionalidades a la aplicación.

## Añadir una sección de juegos relacionados a la página de detalles

Una de las ventajas del agente en la nube es que permite repartir el trabajo. Mientras tú te ocupas de unas tareas, el agente puede trabajar en otras. Vamos a asignarle la solicitud del equipo de diseño para añadir una sección de juegos relacionados.

1. Vuelve al repositorio en github.com.
2. Selecciona la pestaña **Issues**.
3. Selecciona **New issue** para abrir el cuadro de diálogo de una incidencia nueva.
4. Selecciona **Blank issue**.
5. En **Title**, escribe `Show related games on the game details page`.
6. En **Description**, escribe:

    ```markdown
    We want to help people discover more games by showing related games on each game's details page. The design team wants to explore the UX and do some acceptance testing. Our requirements are:

   - Add a data-access helper in `src/lib/` that returns other games in the same category as a given game, excluding that game
   - Show a "Related games" section on the game details page that uses the helper
   - Handle the case where a game has no related games
   - There should be unit tests created for the new helper
   - Before creating the PR, ensure all tests pass
   ```

7. Selecciona **Create** para crear la incidencia.
8. A la derecha, selecciona **Assign to Copilot** para abrir el cuadro de diálogo de asignación.

  ![Asignar Copilot a una incidencia](../../../_images/shared-assign-copilot.png)

9. Selecciona **Assign**.

Poco después, aparecerá 👀 en el primer comentario de la incidencia para indicar que Copilot está trabajando en ella.

![Copilot usa el emoji de ojos para indicar que está trabajando en la incidencia](../../../_images/ex4-issue-eyes-emoji.png)

Copilot ya está trabajando en tu segunda solicitud. El agente de GitHub Copilot en la nube funciona de manera parecida a un ingeniero de software: no hace falta que lo supervises continuamente; puedes revisar el resultado cuando termine. Ahora crearás y utilizarás agentes personalizados.

## Resumen y siguientes pasos

Has explorado el [agente de GitHub Copilot en la nube][copilot-agents]. Puedes asignarle incidencias para que trabaje de forma asíncrona y delegarle tareas de deuda técnica, desarrollo de funcionalidades o migración entre frameworks.

Has explorado estos aspectos:

- personalizar el entorno de generación de código;
- asegurarte de que las operaciones se realizan de forma segura;
- crear incidencias con un alcance claro;
- asignar incidencias a Copilot.

Mientras el agente en la nube trabaja, puedes crear y utilizar agentes personalizados. El [agente en la nube también puede usar servidores de Model Context Protocol (MCP)][cloud-agent-mcp] y las instrucciones personalizadas que has explorado en ejercicios anteriores.

## Recursos

- [Acerca del agente de GitHub Copilot en la nube][copilot-agents]
- [Asignar incidencias de GitHub a Copilot][assign-issue]
- [Buenas prácticas para el flujo de trabajo de configuración del agente en la nube][cloud-agent-best-practices]

| [Siguiente ejercicio: Agentes personalizados →][next-lesson] |
|--:|

[previous-lesson]: ../1-custom-instructions/
[next-lesson]: ../3-custom-agents/
[cloud-agent-mcp]: https://docs.github.com/copilot/how-tos/copilot-on-github/customize-copilot/customize-cloud-agent/extend-cloud-agent-with-mcp
[assign-issue]: https://docs.github.com/copilot/how-tos/use-copilot-agents/cloud-agent/start-copilot-sessions
[setup-workflow]: https://docs.github.com/copilot/how-tos/copilot-on-github/customize-copilot/customize-cloud-agent/customize-the-agent-environment
[copilot-agents]: https://docs.github.com/copilot/concepts/agents/cloud-agent/about-cloud-agent
[cloud-agent-best-practices]: https://docs.github.com/copilot/how-tos/copilot-on-github/customize-copilot/customize-cloud-agent/customize-the-agent-environment
[github-actions]: https://docs.github.com/actions
