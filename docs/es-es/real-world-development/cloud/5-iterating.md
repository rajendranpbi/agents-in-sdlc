---
title: "Ejercicio 5: Iterar sobre el trabajo de GitHub Copilot"
authors:
  - geektrainer
lastUpdated: 2026-06-30
---

| [← Ejercicio anterior: Gestión de agentes][previous-lesson] |
|:--|

## Revisar el trabajo

Durante este taller has trabajado con GitHub Copilot en varias tareas para mejorar la experiencia y añadir funcionalidades. Le has pedido que añada documentación al código, implemente una función para mostrar juegos relacionados y añada funciones de accesibilidad, incluidos modos de alto contraste y claro. Ahora revisarás los cambios y, si hace falta, pedirás a Copilot que los mejore.

### Escenario

Los fundamentos del diseño de software y DevOps no cambian al incorporar la IA generativa. Siempre hay que revisar el código generado y seguir el proceso habitual de DevOps. Antes de activar la revisión para el resto del equipo, revisarás las propuestas de GitHub Copilot para la documentación, los juegos relacionados y las funciones de accesibilidad.

## Seguridad y agente de GitHub Copilot en la nube

Como el agente de GitHub Copilot en la nube trabaja de forma asíncrona y sin supervisión, se han establecido ciertas medidas de seguridad:

- Copilot solo tiene acceso de lectura al repositorio y acceso de escritura **únicamente** a la rama que utilizará.
- El agente en la nube se ejecuta en GitHub Actions y crea un entorno independiente y efímero para trabajar.
- Una persona debe aprobar cualquier flujo de trabajo de GitHub Actions antes de ejecutarlo.
- [El acceso a recursos externos está limitado de forma predeterminada](https://docs.github.com/copilot/how-tos/copilot-on-github/customize-copilot/customize-cloud-agent/customize-the-agent-firewall), incluidos los servidores del protocolo Model Context Protocol (MCP).

## Revisar la documentación generada

Empieza por la primera solicitud de incorporación de cambios (PR) que generó el agente de GitHub Copilot en la nube: la documentación del código. Revisarás los cambios en la interfaz habitual de PR de GitHub.com.

> [!NOTE]
> Es posible que la PR muestre un aviso de que Copilot está bloqueado por un firewall. Es **lo esperado**, ya que Copilot tiene acceso limitado de forma predeterminada a los recursos externos, incluidas las llamadas a servidores MCP externos. Si quieres, puedes [personalizar o deshabilitar el firewall del agente en la nube][agent-firewall].

1. Vuelve al repositorio en github.com.
2. Selecciona **Pull Requests** para abrir la lista de solicitudes.
3. Abre la solicitud titulada, por ejemplo, **Add missing documentation**.

> [!NOTE]
> Si Copilot sigue trabajando, la solicitud incluirá la etiqueta **[WIP]**. En ese caso, espera a que termine. Puede tardar unos minutos.

4. Cuando la solicitud esté lista, selecciona la pestaña **Files changed** y revisa los cambios.

   ![Pestaña Files changed](../../../_images/shared-pr-files-changed.png)

5. Explora el código actualizado, que incluirá nuevos comentarios de documentación de TypeScript (TSDoc) y otra documentación. Los cambios concretos pueden variar.

   Mientras revisas los cambios, busca comentarios de documentación TSDoc, convenciones de TypeScript y encabezados de comentario. Proceden de los archivos de instrucciones personalizadas que revisaste al principio del ejercicio 2.

6. Cuando hayas revisado los cambios, vuelve a la pestaña **Conversation** y desplázate hacia abajo.
7. Comprueba si hay un indicador que señale que hay flujos de trabajo pendientes de aprobación.
8. Si los hay, selecciona **Approve and run workflows**.

   ![Aprobar y ejecutar flujos de trabajo](../../../_images/shared-approve-workflows.png)

9. Comprueba que los flujos de trabajo aparezcan en la sección de comprobaciones de la PR. Si todo va bien, las comprobaciones del proyecto para la aplicación Astro se superarán. El proceso puede tardar unos minutos.

## Pedir cambios a GitHub Copilot

La colaboración con Copilot no tiene por qué ser unidireccional. Puedes mencionarlo en comentarios de la PR, igual que harías con otra persona del equipo, o añadir comentarios en línea al código. Copilot verá esos comentarios e iniciará otra sesión para resolverlos. Como los resultados son probabilísticos, no es posible indicar un texto exacto. Puedes pedirle, por ejemplo, que:

- añada encabezados de comentario al principio de cada archivo de código con una breve descripción;
- añada comentarios de documentación TSDoc a los archivos TypeScript y Astro;
- cree un README que describa la estructura de la aplicación Astro.

1. Añade un comentario para solicitar un cambio en la documentación generada y menciona a **@copilot**, como mencionarías a cualquier persona. Usa una de las ideas anteriores u otra petición relacionada con la documentación del código.
2. Selecciona **View Session** para observar cómo Copilot trabaja. Verás que inicia una sesión nueva para realizar los cambios.
3. Si quieres volver a la solicitud, selecciona **Back to pull request**.

   ![Volver a la solicitud de incorporación de cambios](../../../_images/ex7-back-to-pr.png)

4. Cuando Copilot termine, debería aparecer una confirmación nueva en la solicitud.
5. Selecciona **Files changed** y revisa los cambios.

Puedes seguir iterando hasta que estés conforme. Después, puedes marcar la PR en borrador como lista para revisión y fusionarla en la rama principal.

![Marcar una PR como lista para revisión](../../../_images/ex7-ready-for-review.png)

## Revisar la función de juegos relacionados

Vuelve a la PR que Copilot generó para resolver la incidencia sobre los juegos relacionados en la página de detalles.

1. Vuelve al repositorio en GitHub.com.
2. Selecciona la pestaña **Pull Requests**.
3. Selecciona la PR titulada, por ejemplo, **Show related games on the game details page**.
4. Selecciona la pestaña **Files changed** y revisa el código generado.
5. Cuando hayas revisado los cambios, vuelve a la pestaña **Conversation** y desplázate hacia abajo.
6. Comprueba si hay un indicador que señale que hay flujos de trabajo pendientes de aprobación.
7. Si los hay, selecciona **Approve and run workflows**.

   ![Aprobar y ejecutar flujos de trabajo](../../../_images/shared-approve-workflows.png)

8. Comprueba que los flujos de trabajo aparezcan en la sección de comprobaciones de la PR. Si todo va bien, las comprobaciones del proyecto para la aplicación Astro se superarán. El proceso puede tardar unos minutos.
9. **Opcional:** puedes cambiar a esta rama en tu Codespace para probar manualmente la función de juegos relacionados. Abre el terminal y ejecuta estos comandos; sustituye `<branch-name>` por el nombre de la rama creada por Copilot, por ejemplo, **copilot/fix-8**:

   ```bash
   git fetch origin
   git checkout <branch-name>
   ```

Copilot ha creado la función de juegos relacionados. Como antes, puedes pedir cambios al agente en la nube mediante comentarios nuevos en la pestaña **Conversation**. Por ejemplo, puedes ajustar la cantidad de juegos relacionados o pedir que añada encabezados de comentario y comentarios TSDoc. Recuerda que esta tarea se asignó **antes** de actualizar las instrucciones personalizadas.

## Revisar las funciones de accesibilidad

Por último, revisa las funciones de accesibilidad que implementó el agente personalizado. Esta PR debería incluir el modo de alto contraste que asignaste en el ejercicio 3 y el modo claro que solicitaste desde Mission Control en el ejercicio 4.

1. Vuelve al repositorio en GitHub.com.
2. Selecciona la pestaña **Pull Requests**.
3. Selecciona la PR titulada, por ejemplo, **Add high contrast mode to website**.

> [!NOTE]
> Si Copilot todavía está trabajando, la PR incluirá la etiqueta **[WIP]**. En ese caso, espera a que termine; puede tardar unos minutos.

4. Selecciona la pestaña **Files changed** y revisa el código generado.
5. Revisa la implementación y presta atención a:
   - los componentes de interfaz para alternar entre los modos;
   - el uso del almacenamiento local para conservar las preferencias;
   - los cambios en las hojas de estilo en cascada (CSS) o en otros estilos para los modos de alto contraste y claro;
   - los atributos de accesibilidad, como los de Accessible Rich Internet Applications (ARIA) y la navegación con teclado;
   - el código JavaScript/TypeScript que gestiona el cambio de modo.
6. Cuando hayas revisado los cambios, vuelve a la pestaña **Conversation** y desplázate hacia abajo.
7. Comprueba si hay un indicador que señale que hay flujos de trabajo pendientes de aprobación.
8. Si los hay, selecciona **Approve and run workflows**.

   ![Aprobar y ejecutar flujos de trabajo](../../../_images/shared-approve-workflows.png)

9. Comprueba que los flujos de trabajo aparezcan en la sección de comprobaciones de la PR. Si todo va bien, las comprobaciones del proyecto para la aplicación Astro se superarán. El proceso puede tardar unos minutos.
10. **Opcional:** puedes cambiar a esta rama en tu Codespace para probar manualmente las funciones de accesibilidad. Abre el terminal y ejecuta estos comandos; sustituye `<branch-name>` por el nombre de la rama creada por Copilot:

    ```bash
    git fetch origin
    git checkout <branch-name>
    ```

    Después, inicia la aplicación y prueba los modos de alto contraste y claro en el navegador. Confirma que funcionan y que las preferencias se conservan al volver a cargar la página.

Observa cómo el agente personalizado de accesibilidad ha ayudado a Copilot a implementar las funciones siguiendo buenas prácticas de accesibilidad. Si detectas problemas o mejoras posibles, puedes mencionar a **@copilot** en un comentario y pedirle que haga cambios, igual que con las PR anteriores.

## Ejercicio opcional: seguir delegando

El agente en la nube resulta útil para delegar tareas reales del backlog mientras te ocupas de otras cosas. Para adquirir esa costumbre, crea algunas incidencias más en el repositorio y asígnaselas a Copilot. Por ejemplo:

- crear un formulario de interés para patrocinadores en la página de detalles del juego;
- implementar la paginación en la página de la lista de juegos;
- añadir validación de entradas y gestión de errores a los helpers de acceso a datos.

## Resumen

Has completado el entorno del agente en la nube. En estas lecciones, has:

- **inspeccionado los archivos de instrucciones personalizadas incluidos en el repositorio** para reconocer su efecto en el trabajo del agente;
- **asignado incidencias al agente de GitHub Copilot en la nube** y observado cómo configura el entorno, planifica y trabaja de forma asíncrona;
- **creado y utilizado un agente personalizado** de accesibilidad para añadir los modos de alto contraste y claro;
- **utilizado la página de agentes de Copilot como Mission Control** para supervisar y guiar la sesión de accesibilidad;
- **revisado e iterado sobre las solicitudes de incorporación de cambios del agente en la nube**, mencionando a `@copilot` para solicitar cambios y aprobando flujos de trabajo.

## Revisión y siguientes pasos

Has completado el entorno del agente en la nube. Si quieres seguir explorando, los otros entornos complementan lo que has practicado:

- **[Entorno de VS Code](../../vscode/)**: explora el modo de agente de Copilot Chat y la integración de MCP directamente en el entorno de desarrollo integrado.
- **[Entorno de CLI](../../cli/)**: entrega desde el terminal dos cambios revisados con los modos Plan y Autopilot, habilidades de agente, un agente personalizado de garantía de calidad (QA), Playwright MCP, Agent Merge y el puente opcional `/delegate` al agente en la nube.

En tu propio repositorio, prueba estas ideas:

- asigna al agente en la nube una incidencia de refactorización o cobertura de pruebas;
- crea un agente personalizado para otro ámbito;
- configura `copilot-setup-steps.yml` para otra pila tecnológica.

## Recursos

- [GitHub Copilot][github-copilot]
- [Acerca de los agentes de Copilot][copilot-agents]
- [Asignar incidencias de GitHub a Copilot][assign-issue]
- [Buenas prácticas para el flujo de trabajo de configuración del agente en la nube][cloud-agent-best-practices]
- [Configurar el firewall del agente de GitHub Copilot en la nube][agent-firewall]

---

| [← Ejercicio anterior: Gestión de agentes][previous-lesson] |
|:--|

[github-copilot]: https://github.com/features/copilot
[cloud-agent-overview]: https://docs.github.com/copilot/concepts/agents/cloud-agent/about-cloud-agent
[assign-issue]: https://docs.github.com/copilot/how-tos/use-copilot-agents/cloud-agent/start-copilot-sessions
[setup-workflow]: https://docs.github.com/copilot/how-tos/copilot-on-github/customize-copilot/customize-cloud-agent/customize-the-agent-environment
[copilot-agents]: https://docs.github.com/copilot/concepts/agents/cloud-agent/about-cloud-agent
[cloud-agent-best-practices]: https://docs.github.com/copilot/how-tos/copilot-on-github/customize-copilot/customize-cloud-agent/customize-the-agent-environment
[agent-firewall]: https://docs.github.com/copilot/how-tos/copilot-on-github/customize-copilot/customize-cloud-agent/customize-the-agent-firewall

[previous-lesson]: ../4-managing-agents/
