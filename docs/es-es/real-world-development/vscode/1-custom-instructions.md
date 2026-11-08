---
title: "Ejercicio 1 - Instrucciones personalizadas (VS Code)"
authors:
  - geektrainer
lastUpdated: 2026-06-30
---

[← Ejercicio anterior: Requisitos previos][previous-lesson] · [Siguiente ejercicio: Modo de agente →][next-lesson]

El contexto es clave cuando se trabaja con IA generativa. Si una tarea debe realizarse de una forma concreta, o hay información de fondo que Copilot debería conocer, conviene asegurarse de que ese contexto esté disponible. Hay varias formas de compartir contexto específico con Copilot. Una de las principales son los [archivos de instrucciones][instruction-files], que proporcionan directrices para la generación de código.

En este ejercicio, vas a:

- explorar cómo el contexto específico del proyecto, las directrices de programación y los estándares de documentación llegan a Copilot mediante las instrucciones personalizadas del repositorio y los archivos de instrucciones con ámbito de ruta;
- generar el primer conjunto de datos para el filtrado (un helper de editoriales) con las instrucciones *actuales*;
- añadir un nuevo estándar general del repositorio a `.github/copilot-instructions.md`;
- volver a ejecutar el mismo prompt y observar cómo el código generado adopta el estándar nuevo;
- confirmar en `main` la actualización de las instrucciones y el primer componente del filtrado para que Copilot pueda utilizar las instrucciones en el ejercicio siguiente.

> [!CAUTION]
> El código generado puede no seguir algunos de los estándares que establezcas. Copilot es probabilístico. El objetivo es observar la *tendencia* de los cambios en su comportamiento después de actualizar las instrucciones, no conseguir una salida idéntica carácter por carácter.

## Archivos de instrucciones

### Escenario

Como cualquier buen equipo de desarrollo, Tailspin Toys tiene directrices y requisitos para sus prácticas de desarrollo. Entre ellos se incluyen:

- La capa de datos siempre necesita pruebas unitarias.
- La interfaz debe estar en modo oscuro y tener un aspecto moderno.
- Debe añadirse documentación al código en forma de comentarios de documentación de TypeScript (TSDoc).
- Debe añadirse un bloque de comentarios al principio de cada archivo para describir su función.

El uso de archivos de instrucciones permitirá que Copilot disponga de la información adecuada para realizar las tareas de acuerdo con estas prácticas.

### Instrucciones personalizadas

Las instrucciones personalizadas proporcionan contexto y preferencias a Copilot para que entienda mejor tu estilo de programación y tus requisitos. Esta función te permite orientar a Copilot para obtener sugerencias y fragmentos de código más pertinentes. Puedes especificar tus convenciones de programación, bibliotecas y los tipos de comentarios que quieres incluir en el código. Puedes crear instrucciones para todo el repositorio o para tipos concretos de archivos y proporcionar contexto específico para cada tarea.

Hay dos tipos de archivos de instrucciones:

- `.github/copilot-instructions.md`, un único archivo que se envía a Copilot en **todas** las solicitudes del repositorio. Debe contener información a nivel de proyecto pertinente para la mayoría de las solicitudes de chat o CLI del repositorio, como la pila tecnológica, una descripción general del proyecto, buenas prácticas y otras directrices globales.
- Los archivos `.github/instructions/*.instructions.md` pueden crearse para tareas o tipos de archivo concretos. Puedes usarlos para proporcionar directrices para lenguajes, como TypeScript o Astro, o para tareas, como crear un componente de interfaz o un conjunto de pruebas unitarias.

> [!NOTE]
> En el IDE, los archivos de instrucciones solo se usan para generar código en Copilot Chat, no para las finalizaciones de código ni para las sugerencias de la siguiente edición.
>
> Copilot Chat, Copilot CLI y el agente de Copilot en la nube usan tanto los archivos de instrucciones del repositorio como los archivos `*.instructions.md` con metadatos `applyTo` al generar código.
>
> Además, Copilot [admite archivos de instrucciones que usan otros estándares][custom-instructions-support], incluidos AGENTS.md y CLAUDE.md.

### Buenas prácticas para gestionar archivos de instrucciones

Una conversación exhaustiva sobre la creación de archivos de instrucciones queda fuera del alcance del taller. Los ejemplos del proyecto muestran un enfoque representativo. A grandes rasgos:

- Mantén las instrucciones de `copilot-instructions.md` centradas en directrices a nivel de proyecto, como una descripción de lo que se está desarrollando, la estructura del proyecto y los estándares generales de programación.
- Usa archivos `*.instructions.md` para proporcionar directrices específicas de tipos de archivo, como pruebas unitarias, componentes Astro y la capa de datos, o para tareas concretas.
- Usa lenguaje natural y mantén claras las directrices. Incluye ejemplos de cómo debería ser el código y de cómo no debería serlo.

No hay una única forma de crear archivos de instrucciones, igual que no hay una única forma de usar la IA. La experimentación te permitirá descubrir qué funciona mejor en tu proyecto.

> [!TIP]
> Todos los proyectos que usan GitHub Copilot deberían contar con una colección sólida de archivos de instrucciones. Puede que veas instrucciones para muchos tipos de tareas, incluidas las [actualizaciones de la interfaz][ui-instructions] y [Astro][astro-instructions].
>
> Copilot también puede ayudarte a generar archivos de instrucciones. Cada entorno ofrece esta opción de una forma distinta (por ejemplo, **Configure Chat → Generate Agent Instructions** en VS Code o `/init` en Copilot CLI). El ejercicio correspondiente al entorno que utilices lo indicará cuando sea pertinente.
>
> ¿Buscas plantillas o un punto de partida? Explora [awesome-copilot][awesome-copilot], un repositorio con archivos de instrucciones, agentes personalizados y otros recursos.

[ui-instructions]: https://github.com/github-samples/tailspin-toys/blob/main/.github/instructions/ui.instructions.md
[astro-instructions]: https://github.com/github-samples/tailspin-toys/blob/main/.github/instructions/astro.instructions.md
[awesome-copilot]: https://github.com/github/awesome-copilot
[custom-instructions-support]: https://docs.github.com/copilot/reference/custom-instructions-support

## Explorar los archivos de instrucciones personalizadas de este proyecto

Dedica un momento a leer los archivos de instrucciones incluidos en este repositorio: hay un archivo principal `copilot-instructions.md` y varios archivos `*.instructions.md` para distintas tareas. Ábrelos en tu editor o en la interfaz web de GitHub.

1. Abre `.github/copilot-instructions.md`.
2. Explora el archivo y fíjate en la breve descripción del proyecto y en secciones como **Agent notes**, **Code standards**, **Scripts** y **Repository Structure**. En **Code standards**, fíjate en las directrices anidadas **GitHub Actions Workflows**. Se aplican a cualquier interacción con Copilot.
3. Abre la carpeta `.github/instructions` y explora su contenido. Verás instrucciones para archivos Astro, la capa de datos de Drizzle, pruebas y más.
4. Abre `.github/instructions/unit-tests.instructions.md`. Fíjate en el campo `applyTo` de la parte superior: define un patrón glob (relativo a la raíz del repositorio) que determina a qué archivos se aplican las instrucciones. En este caso, coincidirá con cualquier archivo de pruebas TypeScript (por ejemplo, uno que coincida con `**/*.test.ts`).
5. Observa las instrucciones específicas para crear pruebas unitarias para este proyecto.
6. Por último, abre `.github/instructions/drizzle.instructions.md` y desplázate hasta el final. Fíjate en los enlaces a otros archivos de instrucciones, como `unit-tests.instructions.md`, y a archivos existentes del proyecto. Así puedes dividir conjuntos de instrucciones grandes en archivos más pequeños y reutilizables, y mostrar a Copilot ejemplos que debe seguir. Las rutas son relativas al archivo de instrucciones, no a la raíz del repositorio.

> [!NOTE]
> La sección **Code formatting requirements** de `copilot-instructions.md` documenta los estándares de programación del proyecto, pero aún no exige documentación dentro del código. En los pasos siguientes añadirás reglas para los comentarios de documentación TSDoc y los encabezados de comentario a nivel de archivo.

## Crear una rama para los cambios

Crea una rama para trabajar en los cambios.

1. Vuelve al Codespace del ejercicio anterior.
2. Abre un terminal con <kbd>Control</kbd>+<kbd>`</kbd> (Mac) o <kbd>Ctrl</kbd>+<kbd>`</kbd> (Windows/Linux).
3. Crea una rama y cámbiate a ella:

   ```bash
   git checkout -b custom-instructions
   ```

## Usar Copilot Chat *antes* de actualizar las instrucciones

Para observar el efecto de las instrucciones personalizadas, empieza enviando un prompt con el archivo de instrucciones actual. Más adelante, lo actualizarás y volverás a enviar el mismo prompt para comparar el resultado.

1. Cierra las pestañas abiertas de ejercicios anteriores para que Copilot reciba solo el contexto que necesitas.
2. Abre `src/lib/publishers.ts` para indicar a Copilot dónde debe ubicarse el helper.
3. En el menú desplegable de agentes de la vista Chat, selecciona **Agent** para que Copilot pueda aplicar cambios a los archivos.

   ![Captura del selector de agentes en la vista Chat](../../../_images/shared-chat-mode-selector.png)

4. Envía el prompt siguiente:

   ```plaintext
   Create or update src/lib/publishers.ts with a data-access helper that returns a list of all publishers with the name and id for each. Apply the file changes.
   ```

5. Copilot explorará el proyecto y aplicará cambios de código, a menudo en el archivo del helper y sus pruebas.
6. Fíjate en que el helper propuesto es una función tipada que recibe un cliente `db` como primer argumento y devuelve un array tipado de editoriales. Esto procede de las convenciones de la capa de datos de `.github/instructions/drizzle.instructions.md`, que se aplican a `src/lib/*.ts`.
7. Observa que al código propuesto **le faltan** comentarios de documentación TSDoc y un encabezado de comentario a nivel de archivo.

> [!CAUTION]
> Como Copilot es probabilístico, puede que añada comentarios de documentación aunque no se lo pidas. No pasa nada; el objetivo sigue siendo observar la mejora de la *coherencia* después de actualizar las instrucciones.

## Añadir un nuevo estándar del repositorio

`.github/copilot-instructions.md` proporciona información del proyecto a Copilot. Documenta los estándares de programación del repositorio para mejorar las sugerencias.

1. Vuelve a abrir `.github/copilot-instructions.md`.
2. Localiza la sección **Code formatting requirements**, que debería estar cerca de la línea 27. Observa cómo documenta los estándares de programación, pero todavía no tiene una regla para la documentación dentro del código. Por eso el helper generado no incluía comentarios de documentación.
3. Añade las líneas de Markdown siguientes justo debajo de los estándares existentes para indicar a Copilot que añada encabezados de comentario a nivel de archivo y comentarios de documentación TSDoc:

   ```markdown
   - Every exported function should have a TSDoc comment describing its purpose, parameters, and return value.
   - Before imports or any code, add a comment block to the file that explains its purpose.
   ```

4. Guarda `copilot-instructions.md`.

> [!TIP]
> Como has visto en el ejercicio anterior, los archivos de instrucciones pueden crearse a nivel de repositorio (`.github/copilot-instructions.md`) para proporcionar directrices globales, o como archivos `*.instructions.md` para lenguajes, tipos de archivo o tareas concretos. El archivo a nivel de repositorio es el lugar adecuado para estándares generales del proyecto, como la regla sobre los comentarios de documentación que acabas de añadir.

## Volver a ejecutar el prompt y observar el cambio

1. Vuelve a Copilot Chat y selecciona **New Chat** para borrar el contenido de la conversación.
2. Selecciona de nuevo `src/lib/publishers.ts` para que Copilot se centre en el archivo correcto.
3. Envía el **mismo prompt** de antes:

   ```plaintext
   Create or update src/lib/publishers.ts with a data-access helper that returns a list of all publishers with the name and id for each. Apply the file changes.
   ```

4. Observa que el archivo propuesto ahora empieza con un bloque de comentarios parecido a este:

   ```typescript
   /**
    * Publisher data-access helpers for the Tailspin Toys Crowd Funding platform.
    * Provides functions to retrieve publisher information from the database.
    */
   ```

5. Observa que la función propuesta ahora incluye un comentario TSDoc parecido a este:

   ```typescript
   /**
    * Returns a list of all publishers with their id and name.
    *
    * @param db - The Drizzle database client.
    * @returns A promise that resolves to an array of publisher objects.
    */
   ```

Has guiado a Copilot para que siga un estándar nuevo y lo aplique a código real que utilizarás en el ejercicio siguiente.

## Confirmar las instrucciones y enviar la rama

Los archivos de instrucciones, como los demás recursos del repositorio, se gestionan mediante el control de código fuente. Confirma los cambios y envía la rama.

1. Abre un terminal con <kbd>Control</kbd>+<kbd>`</kbd> (Mac) o <kbd>Ctrl</kbd>+<kbd>`</kbd> (Windows/Linux).
2. Comprueba que estén presentes la actualización de instrucciones y los cambios del helper:

   ```bash
   git status
   ```

3. Prepara y confirma la actualización de instrucciones y todos los archivos que Copilot haya modificado para crear la base del helper:

   ```bash
   git add .github/copilot-instructions.md src/lib/publishers.ts
   # If git status shows additional supporting updates (for example tests), add those files too.
   git commit -m "Add doc comment standards and publishers helper foundation"
   ```

4. Envía la rama al repositorio:

   ```bash
   git push -u origin custom-instructions
   ```

## Crear y fusionar una solicitud de incorporación de cambios

Una vez enviada la rama, crea una solicitud de incorporación de cambios y asóciala con la incidencia del backlog sobre el estándar de documentación. Copilot puede buscar la incidencia, crear la PR y fusionarla mediante las herramientas de GitHub conectadas al proyecto.

> [!NOTE]
> Este es el primer ejercicio en el que Copilot realiza acciones en GitHub. La plantilla del proyecto ya conecta las herramientas de GitHub con tu entorno, así que no hace falta configurarlas. La primera vez que Copilot use una herramienta, VS Code puede pedirte que inicies sesión en GitHub. Sigue las instrucciones para permitirlo. En un ejercicio posterior aprenderás cómo funciona esta conexión (Model Context Protocol).

1. Abre Copilot Chat en tu Codespace.
2. Pulsa <kbd>Control</kbd>+<kbd>Command</kbd>+<kbd>I</kbd> (Mac) o <kbd>Ctrl</kbd>+<kbd>Alt</kbd>+<kbd>I</kbd> (Windows/Linux) para abrir la vista Copilot Chat y confirma que **Agent** esté seleccionado en el selector de agentes.
3. Pide a Copilot que encuentre la incidencia relacionada con la actualización de los archivos de instrucciones y cree una PR desde la rama actual. Indícale que describa tanto la actualización de instrucciones como la base del helper de editoriales:

   ```plaintext
   Find the issue related to updating the instructions file. Create a new PR from the current branch, highlight that the PR closes that issue, and include that we also added the publishers helper foundation for the upcoming filtering work.
   ```

4. Copilot buscará la incidencia y creará la PR.
5. Cuando Copilot te pida permiso para realizar acciones de GitHub, revisa el comando y selecciona **Allow** si estás de acuerdo.
6. Cuando se cree la PR, pídele a Copilot que la fusione y vuelva a `main` en el repositorio local:

   ```plaintext
   Merge the PR into main. Then return to main locally, and pull the latest code so we are up to date.
   ```

7. Cuando Copilot solicite permiso para realizar acciones de GitHub y ejecutar comandos de shell, revisa el comando y selecciona **Allow** si estás de acuerdo.

¡Has creado y fusionado una solicitud de incorporación de cambios con ayuda de GitHub Copilot!

## Resumen y siguientes pasos

Has explorado cómo Copilot obtiene el contexto de los archivos de instrucciones y has utilizado Copilot Chat en VS Code para:

- enviar un prompt de generación de código y observar el resultado con las instrucciones *existentes*;
- añadir un estándar general del repositorio a `.github/copilot-instructions.md`;
- volver a ejecutar el mismo prompt y observar cómo el código propuesto adopta el nuevo estándar;
- confirmar en `main` la actualización de las instrucciones y el helper para que el siguiente ejercicio pueda aprovecharlos.

A continuación, aplicarás esas instrucciones en el [modo de agente][next-lesson] mientras Copilot añade una funcionalidad al código.

## Recursos

- [Archivos de instrucciones para personalizar GitHub Copilot][instruction-files]
- [Buenas prácticas para crear instrucciones personalizadas][instructions-best-practices]
- [5 consejos para escribir mejores instrucciones personalizadas][copilot-instructions-five-tips]
- [Instrucciones personalizadas para GitHub Copilot][personal-instructions]
- [Awesome Copilot: una colección de archivos de instrucciones y otros recursos][awesome-copilot]

[previous-lesson]: ../0-prerequisites/
[next-lesson]: ../2-agent-mode/
[instruction-files]: https://code.visualstudio.com/docs/copilot/copilot-customization
[instructions-best-practices]: https://docs.github.com/enterprise-cloud@latest/copilot/using-github-copilot/coding-agent/best-practices-for-using-copilot-to-work-on-tasks#adding-custom-instructions-to-your-repository
[personal-instructions]: https://docs.github.com/copilot/customizing-copilot/adding-personal-custom-instructions-for-github-copilot
[copilot-instructions-five-tips]: https://github.blog/ai-and-ml/github-copilot/5-tips-for-writing-better-custom-instructions-for-copilot/
