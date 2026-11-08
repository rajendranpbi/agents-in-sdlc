---
title: "Ejercicio 2 - Añadir funcionalidades con el modo de agente de Copilot"
authors:
  - geektrainer
lastUpdated: 2026-06-30
---

Incluso los cambios más sencillos de una aplicación suelen requerir modificar varios archivos y realizar tareas como ejecutar pruebas. Como desarrollador, normalmente tienes que localizar todos los archivos necesarios, hacer los cambios, ejecutar las pruebas, depurar, averiguar qué archivo te has dejado y volver a modificarlo. Y la lista continúa.

El modo de agente de Copilot puede ayudarte con estas tareas.

El modo de agente de Copilot está diseñado para trabajar de forma más autónoma en el entorno de desarrollo integrado (IDE). Se comporta de manera parecida a un desarrollador: primero explora la estructura del proyecto, realiza los cambios necesarios, ejecuta tareas como las pruebas y corrige automáticamente los errores que detecta. Vamos a ver cómo usar el modo de agente para añadir funcionalidades al sitio.

> [!NOTE]
> Aunque los nombres se parecen, el modo de agente y el agente en la nube ofrecen experiencias distintas. El modo de agente trabaja en el IDE y permite ciclos de respuesta rápidos e interacción directa. El agente en la nube funciona de forma asíncrona, como un compañero del equipo, e interactúa contigo mediante incidencias y solicitudes de incorporación de cambios.

En este ejercicio, aprenderás a:

- usar el modo de agente de Copilot para explorar el proyecto, identificar archivos pertinentes y coordinar cambios;
- implementar funcionalidades en la interfaz y la capa de datos;
- revisar los cambios y las pruebas que genera el modo de agente antes de fusionarlos en el código.

## Escenario

A medida que aumenta la lista de juegos, quieres permitir que los usuarios filtren por categoría y editorial. En el ejercicio anterior ya añadiste un helper de editoriales. Ahora completarás el trabajo restante en la capa de datos, la interfaz y las pruebas con el modo de agente de Copilot.

## Ejecutar el sitio web de Tailspin Toys

Antes de hacer cambios, explora el sitio web de Tailspin Toys para conocer su funcionamiento actual.

El sitio web es una plataforma de financiación colectiva de juegos de mesa con una temática para desarrolladores. Permite mostrar juegos y consultar sus detalles. El sitio es una única aplicación Astro que genera páginas en lenguaje de marcado de hipertexto (HTML) estáticas durante la compilación. Las páginas consultan directamente una base de datos SQLite local mediante Drizzle, un mapeador objeto-relacional (ORM), en los metadatos iniciales; no hay una interfaz de programación de aplicaciones (API) de backend independiente ni un framework de interfaz del lado del cliente. Los helpers de acceso a datos reutilizables están en `src/lib/`, y la interactividad se añade mediante un pequeño elemento `<script>` de Astro con ámbito local que usa las API del modelo de objetos del documento (DOM) estándar.

### Iniciar el sitio web

El comando `npm run dev` inicia el único servidor de desarrollo de Astro. Para ejecutarlo en GitHub Codespaces:

1. Vuelve a tu Codespace. Después del ejercicio anterior, deberías estar en `main`.
2. Abre un terminal en el Codespace con <kbd>Control</kbd>+<kbd>\`</kbd> (Mac) o <kbd>Ctrl</kbd>+<kbd>\`</kbd> (Windows/Linux).
3. Crea una rama para el trabajo de filtrado:

   ```bash
   git checkout main
   git pull
   git checkout -b filtering-vscode
   ```

4. Inicia el sitio web:

   ```bash
   npm run dev
   ```

   Cuando el servidor de desarrollo de Astro esté listo, debería aparecer un mensaje parecido a este:

   ```bash
   🚀 Tailspin Toys is ready!
      Astro server: http://localhost:4321
      Press Ctrl-C to stop.
   ```

> [!NOTE]
> Si aparece un cuadro de diálogo para abrir un navegador en `http://localhost:4321`, ciérralo seleccionando **x**.

5. Para abrir el sitio web, mantén pulsada la tecla <kbd>Command</kbd> (Mac) o <kbd>Ctrl</kbd> (Windows/Linux) y selecciona la dirección del servidor Astro `http://localhost:4321` en el terminal.

> [!NOTE]
> En Codespaces, al seleccionar un enlace `localhost` en el terminal, se te redirigirá automáticamente a `https://<your-codespace-name>-4321.app.github.dev/`. Es un túnel privado a tu Codespace, que ahora aloja el servidor web.

### Explorar el sitio web

Cuando el sitio esté en ejecución, explora sus funciones principales:

- **Home Page**: muestra una lista de juegos de mesa con títulos, imágenes y descripciones.
- **Game Details Page**: al seleccionar un juego, se abre una página con más información, como el título, la descripción, la editorial y la categoría.

## Explorar el backlog con Copilot

> [!TIP]
> **Abrir Copilot Chat**
>
> Antes de empezar, vuelve a tu Codespace, abre el panel Copilot Chat y selecciona **New Chat** para iniciar una conversación nueva. El modo y el modelo varían según el ejercicio; cada paso lo indicará cuando sea pertinente.

La primera versión del sitio funciona, pero queremos añadir capacidades. Para empezar, revisa el backlog. Al crear el proyecto a partir de la plantilla, se generó automáticamente un backlog de incidencias. Pide a GitHub Copilot que te muestre esas tareas.

1. En el menú desplegable de agentes de la vista Chat, selecciona **Agent**. El agente planifica e implementa cambios de forma autónoma en varios archivos, ejecuta comandos de terminal y llama a herramientas.

   ![Captura del selector de agentes en la vista Chat](../../../_images/shared-chat-mode-selector.png)

2. En la lista de modelos disponibles, selecciona **Claude Sonnet 4.5**.

> [!CAUTION]
> Los autores del taller no recomiendan un modelo concreto. Durante su desarrollo se usó Claude Sonnet 4.5, por lo que se incluye en las instrucciones para intentar obtener sugerencias relativamente coherentes y ofrecer una experiencia fluida. Como los modelos de lenguaje grande (LLM) son probabilísticos, las sugerencias pueden diferir de los ejemplos del taller. Es normal.

> [!NOTE]
> Debido a la naturaleza probabilística de los LLM, Copilot puede usar un comando MCP diferente, pero debería poder completar la tarea.

3. Envía a Copilot este prompt para consultar las incidencias del backlog:

   ```plaintext
   Please show me the backlog of items from my GitHub repository. Help me prioritize them based on those which will be most useful to the user.
   ```

4. Selecciona **Continue** para ejecutar el comando que enumera las incidencias.
5. Revisa la lista de incidencias generada.

Observa que Copilot también ha priorizado las tareas según las que considera más útiles para el usuario.

## Revisar los archivos de instrucciones

Antes de pedir al agente que genere código, revisa el archivo de instrucciones que proporcionará contexto a Copilot. Usarás el archivo de [interfaz de usuario (UI)](https://github.com/github-samples/tailspin-toys/blob/main/.github/instructions/ui.instructions.md), que explica cómo añadir funcionalidades al sitio web.

1. En el Codespace, abre `.github/instructions/ui.instructions.md`.
2. Revisa las directrices generales para añadir funcionalidades, entre ellas:
   - una descripción general de la arquitectura;
   - principios de diseño de componentes, capacidad de prueba y accesibilidad;
   - enlaces a archivos de instrucciones específicos para Astro, Tailwind CSS y Drizzle.

> [!TIP]
> Los archivos de instrucciones pueden incluir referencias a otros archivos de instrucciones y a archivos del proyecto. Las rutas son relativas al archivo de instrucciones. Así puedes reutilizar contenido, dividir conjuntos complejos en partes más manejables y proporcionar ejemplos y plantillas.

## Implementar el filtrado

Para completar el filtrado, habrá que modificar al menos tres partes de la aplicación:

- añadir o ajustar la lógica restante de filtrado en la capa de datos (`src/lib/`);
- añadir o actualizar las pruebas del comportamiento del filtrado;
- actualizar la página de lista de juegos para incluir la interfaz de filtrado.

También hay que ejecutar las pruebas y confirmar que se superan antes de fusionar los cambios. El modo de agente de Copilot puede realizar estas tareas.

1. Continúa en la conversación actual con Copilot o selecciona **New Chat** para iniciar otra.
2. Selecciona **Add Context**, **Instructions** y **ui** para añadir el archivo de instrucciones.

   ![Captura de cómo seleccionar el archivo de instrucciones de interfaz](../../../_images/ex3-select-instructions-file.png)

3. Confirma que **Agent** siga seleccionado en el menú desplegable de agentes de la vista Chat.

   ![Captura del selector de agentes en la vista Chat](../../../_images/shared-chat-mode-selector.png)

4. Confirma que **Claude Sonnet 4.5** siga seleccionado como modelo.
5. Usa el prompt siguiente para pedir a Copilot que implemente la funcionalidad según la incidencia del backlog:

   ```plaintext
   Please update the site to include filtering by publisher and category based on the requirements from the related GitHub issue in the backlog. A publishers helper already exists from the previous exercise, so preserve and refine that work as needed while completing the remaining data-layer, UI, and tests. Ensure all tests are passing before completion. The server is already running, so you do not need to start it up.
   ```

6. Observa cómo Copilot explora el proyecto y localiza los archivos relacionados con la funcionalidad: los helpers de la capa de datos, la interfaz y las pruebas. Después, modificará los archivos y ejecutará las pruebas.

   ![Captura de Copilot explorando los archivos del proyecto](../../../_images/ex3-agent-mode-explores.png)

> [!NOTE]
> Copilot realizará varias tareas, como explorar el proyecto, modificar archivos y ejecutar pruebas. El proceso puede tardar varios minutos, según la complejidad de la tarea y del código. Durante el trabajo, pueden aparecer los controles **Keep** y **Undo** en el editor. Cuando termine, verás uno de estos controles para cada cambio; no hace falta que los selecciones mientras el agente sigue trabajando.

7. Cuando Copilot te lo solicite, selecciona **Continue** para ejecutar las pruebas.

   ![Captura del cuadro de diálogo de Copilot Chat que pide permiso para ejecutar pruebas](../../../_images/ex3-agent-mode-run-tests.png)

8. Puede que haya pausas y que algunas pruebas fallen durante el proceso. No pasa nada: Copilot alterna entre la generación de código y las pruebas hasta completar la tarea sin detectar errores.

   ![Captura de una sesión completada de Copilot Agent Mode](../../../_images/ex3-agent-mode-proposed-changes.png)

9. Revisa el código generado para detectar posibles problemas.

> [!CAUTION]
> Revisa siempre el código generado por Copilot o cualquier otra herramienta de IA.

10. Vuelve al navegador donde se está ejecutando el sitio web y explora la nueva funcionalidad.
11. Cuando hayas confirmado que todo funciona y revisado el código, selecciona **Keep** en la ventana Copilot Chat.

## Resumen y siguientes pasos

¡Enhorabuena! Has usado el modo de agente de GitHub Copilot para añadir funcionalidades al sitio de Tailspin Toys. Has aprendido a:

- usar el modo de agente de GitHub Copilot para implementar funcionalidades en la interfaz y la capa de datos;
- usar el modo de agente para explorar el proyecto, identificar archivos pertinentes y coordinar cambios;
- revisar los cambios y las pruebas generados por el modo de agente antes de fusionarlos en el código.

A continuación, [probarás la funcionalidad con el servidor MCP de Playwright][next-lesson] y abrirás una solicitud de incorporación de cambios.

### Ejercicio de exploración adicional: implementar la paginación

A medida que aumente la lista de juegos, será necesario habilitar la paginación. Aplica lo aprendido en este ejercicio y pide a Copilot que actualice el sitio. Al redactar el prompt:

- sigue las buenas prácticas existentes e incluye los archivos de instrucciones;
- decide cómo quieres implementar la paginación, por ejemplo, si el usuario podrá elegir el tamaño de página o si será fijo;
- proporciona a Copilot las directrices necesarias para que implemente el comportamiento que quieres;
- itera con GitHub Copilot y proporciona más contexto cuando solicites cambios. Es el flujo normal al trabajar con Copilot.

## Recursos

- [Modos ask, edit y agent de Copilot: qué hacen y cuándo se usan][choose-mode]
- [Modo de agente en VS Code][vs-code-agent-mode]

[previous-lesson]: ../1-custom-instructions/
[next-lesson]: ../3-mcp/
[choose-mode]: https://github.blog/ai-and-ml/github-copilot/copilot-ask-edit-and-agent-modes-what-they-do-and-when-to-use-them/
[vs-code-agent-mode]: https://code.visualstudio.com/docs/copilot/chat/chat-agent-mode
