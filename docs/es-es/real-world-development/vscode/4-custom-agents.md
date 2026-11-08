---
title: "Ejercicio 4 - Agentes personalizados"
authors:
  - geektrainer
lastUpdated: 2026-06-30
---

## ¿Qué son los agentes personalizados?

Los [agentes personalizados][custom-agents-concept] de GitHub Copilot permiten crear asistentes de IA especializados en tareas o ámbitos concretos del flujo de desarrollo. Al definir agentes mediante archivos Markdown en la carpeta `.github/agents` del repositorio, puedes proporcionar a Copilot instrucciones específicas, buenas prácticas, patrones de programación y conocimientos del dominio para que realice determinados tipos de trabajo con más eficacia. Los equipos pueden plasmar su experiencia en agentes reutilizables: por ejemplo, un agente de accesibilidad que aplique las [Pautas de Accesibilidad para el Contenido Web (WCAG)][wcag], uno de seguridad que siga prácticas de programación segura o uno de pruebas que mantenga patrones coherentes.

Los agentes personalizados se definen en archivos Markdown de la carpeta `.github/agents` del proyecto o, de forma global, en `~/.copilot/agents`. Cada archivo tiene metadatos YAML con al menos los campos `name` y `description`, seguidos de un prompt en Markdown que define el comportamiento, la especialización y las instrucciones del agente.

### Diferencias entre agentes personalizados y habilidades de agente

Los agentes personalizados y las [habilidades de agente][agent-skills-concept] comparten algunos aspectos: ambos se definen principalmente mediante archivos Markdown e indican a una IA cómo realizar ciertas operaciones. La diferencia más sencilla es que un **agente personalizado** es quien realiza el trabajo, mientras que las **habilidades** son herramientas.

Los agentes personalizados tienen su propia ventana de contexto y pueden coordinar habilidades e incluso otros agentes como parte del trabajo. En este taller, el agente personalizado de accesibilidad revisará y actualizará el sitio según las directrices de accesibilidad; para hacerlo, podría invocar habilidades como una habilidad de flujo de trabajo de solicitudes de incorporación de cambios o una que ejecute y gestione pruebas.

> [!NOTE]
> No hay una única forma «correcta» de crear un agente personalizado. Como ocurre con todo en IA, prueba distintas opciones e itera para descubrir qué funciona mejor en tus entornos y escenarios.

[custom-agents-concept]: https://docs.github.com/copilot/concepts/agents/cloud-agent/about-custom-agents
[agent-skills-concept]: https://docs.github.com/copilot/concepts/agents/about-agent-skills
[wcag]: https://www.w3.org/WAI/standards-guidelines/wcag/

## Escenario

Tailspin Toys se compromete a que todas las personas puedan usar su plataforma de financiación colectiva, independientemente de sus capacidades visuales o preferencias. Los comentarios recientes indican que a algunas personas les cuesta leer el tema oscuro actual debido al bajo contraste entre el texto y el fondo. Para responder a este problema de accesibilidad, el equipo de diseño ha solicitado un modo de alto contraste que se pueda activar y desactivar.

Como la accesibilidad es importante, conviene implementar esta funcionalidad cuanto antes. Para ello, usarás un agente personalizado.

En este ejercicio, vas a:

- revisar un agente personalizado de accesibilidad existente;
- usar el agente de accesibilidad en Copilot Chat para implementar un modo de alto contraste.

## Revisar el agente personalizado de accesibilidad

Ya se ha creado un agente personalizado de accesibilidad. Revisa su contenido para entender cómo guiará a Copilot.

1. Vuelve a tu Codespace y abre un terminal. Cambia a una rama nueva basada en `main` para el trabajo de accesibilidad. Así mantendrás separada la PR de filtrado del ejercicio 3:

   ```bash
   git checkout main
   git pull
   git checkout -b accessibility-vscode
   ```

2. Abre `.github/agents/accessibility.md`.
3. Fíjate en los metadatos YAML `name` y `description`.

> [!CAUTION]
> Los metadatos `name` y `description` son obligatorios para los agentes personalizados.

4. Revisa las secciones siguientes, que describen:
   - las responsabilidades principales al generar código para un sitio web accesible;
   - las buenas prácticas de accesibilidad;
   - ejemplos de código en lenguaje de marcado de hipertexto (HTML), hojas de estilo en cascada (CSS) y JavaScript;
   - una lista de errores y problemas habituales.

## Usar el agente personalizado en Copilot Chat

VS Code muestra todos los agentes personalizados definidos en `.github/agents` en el selector de agentes situado en la parte inferior de la vista Copilot Chat. Puedes seleccionar un agente personalizado para limitar la sesión de chat a sus instrucciones y herramientas.

1. Abre el panel Copilot Chat y selecciona **New Chat** para iniciar una conversación nueva.
2. Si aún no está seleccionado, elige **Agent** en el selector de agentes de la vista Chat.

   ![Captura del selector de agentes en la vista Chat](../../../_images/shared-chat-mode-selector.png)

3. Selecciona el selector de agentes de la parte inferior de la vista Chat. Mostrará el agente activo; de forma predeterminada, es **default**.
4. En la lista de agentes disponibles, selecciona **Accessibility agent**.
5. Envía al agente de accesibilidad el prompt siguiente:

   ```plaintext
   Add a high-contrast mode to the site. There should be a toggle for high contrast which the user can set, and the setting should persist across page reloads using local storage on the browser.
   ```

6. Copilot Chat explorará el código, propondrá cambios y editará los archivos del proyecto. Cada edición aparecerá en el chat con la ruta del archivo y las diferencias para que puedas revisarlas.
7. A medida que se realicen las ediciones, el indicador **Files changed** se actualizará para mostrar los archivos modificados por el agente.

> [!NOTE]
> El proceso puede tardar varios minutos. Copilot realizará cambios reales en el repositorio y editará archivos existentes, como componentes Astro, CSS y pruebas relacionadas.

En el siguiente ejercicio revisarás y guiarás el trabajo en curso.

## Resumen y siguientes pasos

Has explorado los [agentes personalizados][custom-agents], es decir, asistentes de IA de GitHub Copilot especializados en tareas y ámbitos concretos. Los agentes personalizados permiten plasmar la experiencia y los estándares del equipo en agentes reutilizables que guían a Copilot.

Has explorado estos aspectos:

- cómo se definen los agentes personalizados;
- cómo usar un agente personalizado en el modo de agente de Copilot Chat.

A continuación, [supervisarás y guiarás el trabajo del agente][next-lesson], revisarás los cambios mientras se realizan y añadirás un modo claro a la misma sesión.

## Recursos

- [Acerca de los agentes personalizados][custom-agents]
- [Crear agentes personalizados en el entorno de desarrollo integrado (IDE)][creating-custom-agents-ide]
- [Agentes personalizados en VS Code][custom-agents-vscode]
- [Configuración de agentes personalizados][custom-agents-config]
- [Agentes personalizados en awesome-copilot][awesome-copilot-agents]

---

| [← Ejercicio anterior: Probar la funcionalidad con el servidor MCP de Playwright][previous-lesson] | [Siguiente ejercicio: Supervisar y gestionar agentes →][next-lesson] |
|:--|--:|

[previous-lesson]: ../3-mcp/
[next-lesson]: ../5-managing-agents/
[custom-agents]: https://docs.github.com/copilot/concepts/agents/cloud-agent/about-custom-agents
[creating-custom-agents-ide]: https://docs.github.com/copilot/how-tos/use-copilot-agents/cloud-agent/create-custom-agents-in-your-ide
[custom-agents-vscode]: https://code.visualstudio.com/docs/copilot/customization/custom-agents
[custom-agents-config]: https://docs.github.com/copilot/reference/custom-agents-configuration
[awesome-copilot-agents]: https://github.com/github/awesome-copilot/tree/main/agents
