---
title: "Lección 4 - Guiar Copilot con instrucciones personalizadas"
description: "Explora las instrucciones del repositorio, añade un estándar de documentación y aplícalo al código de filtrado."
authors:
  - geektrainer
lastUpdated: 2026-09-18
---

El contexto es clave cuando se trabaja con IA generativa. Si una tarea debe realizarse de una forma concreta, conviene proporcionar esa orientación a Copilot. Los [archivos de instrucciones][instruction-files] describen no solo *qué* código quieres, sino también *cómo* debe estructurarse. Ahora que ya has creado el filtrado, explorarás las instrucciones que ha usado Copilot, añadirás un estándar de documentación y lo aplicarás al código.

En esta lección, vas a:

- explorar cómo llegan al agente las instrucciones del repositorio y las instrucciones específicas de rutas;
- actualizar el archivo de instrucciones para asegurar que se siguen los estándares de programación;
- observar el efecto de los archivos de instrucciones en el código.

## Escenario

Como cualquier buen equipo de desarrollo, Tailspin Toys tiene directrices y requisitos para sus prácticas de desarrollo. Entre ellos se incluyen:

- Los comentarios deben explicar la intención y las decisiones que no sean evidentes, no limitarse a repetir el código.
- Las funciones exportadas de `db/` y `src/lib/` deben documentar su propósito, sus parámetros y sus valores devueltos mediante comentarios de documentación de TypeScript (TSDoc) o JavaScript (JSDoc), incluido el argumento `db` inyectable cuando corresponda.
- Los componentes Astro reutilizables deben documentar sus contratos `Props`, y los comentarios deben mantenerse actualizados cuando cambie el código relacionado.
- Deben conservarse las directrices de formato y lint existentes.

El uso de archivos de instrucciones permitirá que Copilot disponga de la información adecuada para realizar las tareas de acuerdo con estas prácticas.

## Archivos de instrucciones

Las instrucciones personalizadas proporcionan contexto y preferencias a Copilot para que entienda mejor tu estilo de programación y tus requisitos. Esta función te permite orientar a Copilot para obtener sugerencias y fragmentos de código más pertinentes. Puedes especificar tus convenciones de programación, bibliotecas y los tipos de comentarios que quieres incluir en el código. Puedes crear instrucciones para todo el repositorio o para tipos concretos de archivos y proporcionar contexto específico para cada tarea.

Hay dos tipos de archivos de instrucciones:

- `.github/copilot-instructions.md`, un único archivo que se envía a Copilot en **todas** las solicitudes del repositorio. Debe contener información a nivel de proyecto pertinente para la mayoría de las solicitudes del repositorio que se envían a Copilot.
- Los archivos `.github/instructions/*.instructions.md`, que proporcionan directrices para lenguajes, tipos de archivo o tareas concretos.

> [!NOTE]
> La compatibilidad con otros formatos de instrucciones varía según el entorno. Consulta la [referencia de compatibilidad con las instrucciones personalizadas][custom-instructions-support] antes de depender de un formato concreto.

## Explorar los archivos de instrucciones personalizadas de este proyecto

El proyecto de inicio ya incluye varios archivos de instrucciones. Explóralos antes de hacer cambios para observar el efecto.

1. Vuelve a tu Codespace.
2. En el editor de Codespaces (no en el terminal), abre `.github/copilot-instructions.md`.
3. Explora el archivo y fíjate en la breve descripción del proyecto y las directrices de programación. Estas instrucciones se aplican a todas las interacciones con Copilot en este repositorio.
4. Abre la carpeta `.github/instructions` y explora los archivos. Verás instrucciones para archivos Astro, la capa de datos de Drizzle, pruebas y más.
5. Abre `.github/instructions/unit-tests.instructions.md`. Fíjate en el campo `applyTo` de la parte superior: define un patrón glob que determina a qué archivos se aplican las instrucciones.
6. Abre `.github/instructions/drizzle.instructions.md` y fíjate en los enlaces a otros archivos de instrucciones y a archivos existentes del proyecto. Así puedes dividir conjuntos de instrucciones grandes en archivos más pequeños y reutilizables, y mostrar a Copilot ejemplos que debe seguir.

## Actualizar los archivos de instrucciones según las directrices del equipo

Los archivos actuales son un buen punto de partida, pero aún falta una directriz. Modifica el archivo principal `copilot-instructions.md` para asegurar que se añaden comentarios TSDoc al código TypeScript generado.

1. En `.github/copilot-instructions.md`, localiza la sección titulada **Code formatting guidance**, que debería estar cerca de la línea 35.
2. Añade el siguiente elemento al final de la sección:

   ```markdown
   - All new TypeScript should contain TSDocs comments for documentation purposes.
   ```

El archivo se guardará automáticamente.

## Usar las directrices actualizadas

Copilot CLI carga las instrucciones del repositorio cuando se inicia una conversación. Después de guardar las instrucciones, sal de la conversación de filtrado y retómala para que Copilot cargue las directrices nuevas sin perder el contexto de la funcionalidad.

1. Si la conversación de filtrado sigue abierta, escribe `/exit` para cerrarla. Copilot guarda las conversaciones automáticamente.
2. Desde la raíz del repositorio en la rama `game-filters-cli`, reanuda la conversación guardada:

   ```bash
   copilot --resume
   ```

   Si se te solicita, selecciona la conversación de filtrado y confirma que está seleccionada la rama `game-filters-cli`.
3. Pide a Copilot que aplique las directrices actualizadas:

   ```plaintext
   We just updated our instructions and code guidance. Can you please update the code you generated to match that guidance?
   ```

4. Escribe `/diff` y revisa los archivos TypeScript modificados. Observa los comentarios TSDoc nuevos y confirma que describen el código correctamente.

## Resumen y siguientes pasos

Has explorado cómo Copilot CLI obtiene el contexto de los archivos de instrucciones y has aplicado un estándar nuevo a la funcionalidad. En concreto, has:

- explorado cómo llegan al agente las instrucciones del repositorio y las instrucciones específicas de rutas;
- actualizado el archivo de instrucciones para asegurar que se siguen los estándares de programación;
- observado el efecto de los archivos de instrucciones en el código.

A continuación, [personalizarás y ejecutarás la habilidad reutilizable `quality-checks`][next-lesson] para asegurarte de que se ejecutan el lint y las pruebas de forma coherente.

## Recursos

- [Añadir instrucciones personalizadas para Copilot CLI][instruction-files]
- [Compatibilidad con instrucciones personalizadas][custom-instructions-support]
- [Buenas prácticas para crear instrucciones personalizadas][instructions-best-practices]
- [Awesome Copilot: una colección de archivos de instrucciones y otros recursos][awesome-copilot]

[instruction-files]: https://docs.github.com/copilot/how-tos/copilot-cli/customize-copilot/add-custom-instructions
[instructions-best-practices]: https://docs.github.com/copilot/concepts/prompting/response-customization#writing-effective-custom-instructions
[awesome-copilot]: https://awesome-copilot.github.com/
[custom-instructions-support]: https://docs.github.com/copilot/reference/custom-instructions-support
[previous-lesson]: ../3-agent-modes/
[next-lesson]: ../5-agent-skills/
