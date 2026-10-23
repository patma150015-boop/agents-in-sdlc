---
title: "Ejercicio 5: Iterar sobre el trabajo de GitHub Copilot"
authors:
  - geektrainer
lastUpdated: 2026-06-30
---

| [← Lección anterior: Gestionar agentes][previous-lesson] |
|:--|

## Revisa el trabajo

Durante este laboratorio has trabajado con GitHub Copilot en varias tareas para mejorar la experiencia de usuario y añadir funciones. Le has pedido que añada documentación al código, cree una función de juegos relacionados para que el equipo de diseño pueda iterar y añada funciones de accesibilidad, incluidos controles para el alto contraste y el modo claro. Ahora revisarás los cambios de código y, si es necesario, proporcionarás comentarios a Copilot para mejorarlos.

### Escenario

Como ya se ha señalado varias veces, los principios de diseño de software y DevOps no cambian al incorporar inteligencia artificial (IA) generativa. Siempre hay que revisar el código generado y seguir el proceso habitual de DevOps. Revisarás las sugerencias de GitHub Copilot para la documentación, la función de juegos relacionados y las funciones de accesibilidad antes de habilitar la revisión para el resto del equipo.

## Seguridad y agente en la nube de GitHub Copilot

Como el agente en la nube de Copilot trabaja de forma asíncrona y sin supervisión, se han establecido restricciones de seguridad:

- Copilot solo tiene acceso de lectura al repositorio y acceso de escritura **únicamente** a la rama que usará para su código.
- El agente en la nube se ejecuta en GitHub Actions, donde crea un entorno temporal independiente para trabajar.
- Una persona debe aprobar los flujos de trabajo de GitHub Actions antes de que puedan ejecutarse.
- [El acceso a recursos externos está limitado de forma predeterminada](https://docs.github.com/copilot/how-tos/copilot-on-github/customize-copilot/customize-cloud-agent/customize-the-agent-firewall), incluidos los servidores del Model Context Protocol (MCP).

## Revisa la documentación generada

Empieza por la primera solicitud de incorporación de cambios (PR) que generó el agente en la nube de GitHub Copilot: la que añade documentación al código. La revisarás mediante la interfaz habitual de PR de GitHub.com.

> [!NOTE]
> Al abrir la PR, quizá veas un aviso de que el firewall bloquea a GitHub Copilot. Es **lo esperado**, ya que el acceso a recursos externos, incluidas las llamadas a servidores MCP externos, está limitado de forma predeterminada. Si quieres, puedes [personalizar o deshabilitar el firewall del agente en la nube de Copilot][agent-firewall].

1. Vuelve a tu repositorio en github.com.
2. Selecciona **Pull Requests** para abrir la lista de solicitudes de incorporación de cambios.
3. Abre la PR titulada algo parecido a **Add missing documentation** u otro título más detallado.

> [!NOTE]
> Si Copilot todavía está trabajando, el título de la PR incluirá el indicador **[WIP]**. Espera a que termine. Puede tardar unos minutos; mientras tanto, puedes descansar o repasar lo aprendido.

4. Cuando la PR esté lista, selecciona la pestaña **Files changed** y revisa los cambios.

   ![Pestaña Files changed](../../../_images/shared-pr-files-changed.png)

5. Explora el código actualizado, incluidos los comentarios de documentación TSDoc y otra documentación. Los cambios exactos pueden variar.

   Al revisar los cambios, busca comentarios TSDoc, convenciones de TypeScript y encabezados de comentarios. Proceden de los archivos de instrucciones personalizadas que revisaste al principio del ejercicio 2.

6. Cuando hayas revisado los cambios y estés conforme, vuelve a la pestaña **Conversation** y desplázate hacia abajo.
7. Debería aparecer un indicador de que hay flujos de trabajo pendientes de aprobación.
8. Si los hay, selecciona **Approve and run workflows**.

   ![Botón Approve and run workflows](../../../_images/shared-approve-workflows.png)

9. Los flujos de trabajo deberían aparecer en la sección de comprobaciones de la PR. Si todo va bien, las comprobaciones del proyecto para la única aplicación Astro se superarán. El proceso puede tardar unos minutos.

## Pide cambios a GitHub Copilot

La colaboración con Copilot en una solicitud de incorporación de cambios no es unidireccional. Puedes mencionarlo en comentarios de la PR o en comentarios en línea del código, como harías con otros miembros del equipo. Copilot leerá los comentarios e iniciará otra sesión para responderlos. Como el resultado no es determinista, no hay un prompt único que debas usar. Por ejemplo, puedes pedir a Copilot que:

- añada al principio de cada archivo de código un encabezado con una breve descripción de su finalidad.
- añada comentarios de documentación TSDoc a los archivos TypeScript y Astro.
- cree un README que describa la estructura de la aplicación Astro.

1. Añade un comentario para pedir un cambio en la documentación generada y menciona a **@copilot**, como harías con cualquier usuario. Usa una de las ideas anteriores o propón otro cambio de documentación.
2. Selecciona **View Session** para observar el trabajo de Copilot. Fíjate en que inicia una sesión nueva para aplicar los cambios.
3. Puedes seleccionar **Back to pull request** para volver a la solicitud.

   ![Volver a la solicitud de incorporación de cambios](../../../_images/ex7-back-to-pr.png)

4. Cuando Copilot termine, debería aparecer una confirmación nueva en la PR.
5. Selecciona la pestaña **Files changed** para revisar los cambios.

Puedes seguir iterando hasta quedar conforme. Después, puedes marcar la PR como lista para revisión y combinarla en la rama principal.

![Convertir la PR en lista para revisión](../../../_images/ex7-ready-for-review.png)

## Revisa la función de juegos relacionados

Ahora revisarás la PR que Copilot generó para resolver la incidencia de mostrar juegos relacionados en la página de detalles.

1. Vuelve a tu repositorio en GitHub.com.
2. Selecciona la pestaña **Pull Requests**.
3. Selecciona la PR con un título parecido a **Show related games on the game details page** u otro más detallado.
4. Selecciona la pestaña **Files changed** para revisar el código generado.
5. Cuando hayas revisado los cambios y estés conforme, vuelve a la pestaña **Conversation** y desplázate hacia abajo.
6. Debería aparecer un indicador de que hay flujos de trabajo pendientes de aprobación.
7. Si los hay, selecciona **Approve and run workflows**.

   ![Botón Approve and run workflows](../../../_images/shared-approve-workflows.png)

8. Los flujos de trabajo deberían aparecer en la sección de comprobaciones de la PR. Si todo va bien, las comprobaciones del proyecto para la única aplicación Astro se superarán. El proceso puede tardar unos minutos.
9. **Opcional:** puedes cambiar a esta rama en el Codespace para probar manualmente la función de juegos relacionados. Abre un terminal en el Codespace y ejecuta los comandos siguientes; sustituye `<branch-name>` por el nombre de la rama que creó Copilot, como **copilot/fix-8**:

    ```bash
    git fetch origin
    git checkout <branch-name>
    ```

Copilot ha creado la función de juegos relacionados. Como antes, puedes iterar con el agente en la nube de Copilot y pedirle cambios. Por ejemplo, podrías ajustar la cantidad de juegos relacionados que muestra o pedirle que añada encabezados y comentarios de documentación TSDoc (recuerda que esta incidencia se asignó **antes** de actualizar las instrucciones personalizadas). Para ello, añade otro comentario en la pestaña **Conversation**; Copilot lo leerá e iniciará una nueva sesión.

## Revisa las funciones de accesibilidad

Por último, revisarás las funciones de accesibilidad que implementó el agente personalizado. Esta PR debería incluir tanto el modo de alto contraste del ejercicio 3 como el modo claro solicitado desde Mission Control en el ejercicio 4.

1. Vuelve a tu repositorio en GitHub.com.
2. Selecciona la pestaña **Pull Requests**.
3. Selecciona la PR con un título parecido a **Add high contrast mode to website** u otro más detallado.

> [!NOTE]
> Si Copilot sigue trabajando en la tarea, el título de la PR incluirá el indicador **[WIP]**. Espera a que termine; puede tardar unos minutos.

4. Selecciona la pestaña **Files changed** para revisar el código generado.
5. Revisa la implementación, prestando atención a:
   - los componentes de interfaz para alternar entre los modos.
   - el uso del almacenamiento local para conservar las preferencias del usuario.
   - los cambios de CSS o estilos para los modos de alto contraste y claro.
   - los atributos de accesibilidad, como las etiquetas de Aplicaciones de Internet enriquecidas accesibles (ARIA) y la navegación con teclado.
   - el código JavaScript o TypeScript que gestiona el cambio de modo.

6. Cuando hayas revisado los cambios y estés conforme, vuelve a la pestaña **Conversation** y desplázate hacia abajo.
7. Debería aparecer un indicador de que hay flujos de trabajo pendientes de aprobación.
8. Si los hay, selecciona **Approve and run workflows**.

   ![Botón Approve and run workflows](../../../_images/shared-approve-workflows.png)

9. Los flujos de trabajo deberían aparecer en la sección de comprobaciones de la PR. Si todo va bien, las comprobaciones del proyecto para la única aplicación Astro se superarán. El proceso puede tardar unos minutos.
10. **Opcional:** puedes cambiar a esta rama en el Codespace para probar manualmente las funciones de accesibilidad. Abre un terminal en el Codespace y ejecuta estos comandos; sustituye `<branch-name>` por el nombre de la rama que creó Copilot:

    ```bash
    git fetch origin
    git checkout <branch-name>
    ```

    Después, inicia la aplicación y prueba en el navegador los controles de alto contraste y modo claro. Confirma que funcionan y que la configuración se mantiene después de recargar la página.

Observa cómo el agente personalizado de accesibilidad orientó a Copilot para implementar estas funciones según las prácticas recomendadas de accesibilidad. Si detectas algún problema o mejora de accesibilidad, puedes mencionarlo a **@copilot** en un comentario para pedir cambios, como hiciste con las PR anteriores.

## Ejercicio opcional: sigue delegando

El agente en la nube resulta especialmente útil cuando puedes asignarle elementos reales del backlog y dedicarte a otra cosa. Para adquirir esa práctica, registra algunas incidencias más en el repositorio y asígnaselas a Copilot. Por ejemplo:

- crear un formulario para expresar interés en un juego en su página de detalles.
- implementar la paginación de la lista de juegos.
- añadir validación de entrada y gestión de errores a los asistentes de acceso a datos.

## Resumen

Has completado el recorrido del agente en la nube. En estas lecciones:

- **Has inspeccionado los archivos de instrucciones personalizados del repositorio** para conocer su efecto en los resultados del agente en la nube.
- **Has asignado incidencias al agente en la nube de Copilot** y has observado cómo configura el entorno, planifica y trabaja de forma asíncrona.
- **Has creado y utilizado un agente personalizado** de accesibilidad para añadir los modos de alto contraste y claro.
- **Has usado la página de agentes de Copilot como centro de control** para supervisar y orientar la sesión de accesibilidad.
- **Has revisado e iterado sobre las solicitudes de incorporación de cambios del agente en la nube**, mencionando a `@copilot` para pedir cambios y aprobando flujos de trabajo.

## Revisión y pasos siguientes

Has completado el recorrido del agente en la nube. Para seguir explorando, puedes completar los otros recorridos:

- 🖥️ **[Recorrido de VS Code](../../vscode/)**: explora el modo agente de Copilot Chat y la integración con MCP directamente en el IDE.
- 💻 **[Recorrido de CLI](../../cli/)**: entrega dos cambios revisados desde el terminal con los modos Plan y Autopilot, habilidades de agente, un agente personalizado de QA, Playwright MCP, Agent Merge y la opción de volver a delegar al agente en la nube con `/delegate`.

En tu propio repositorio, prueba estas ideas:

- asignar al agente en la nube una incidencia de refactorización o cobertura de pruebas.
- crear un agente personalizado para otro ámbito.
- configurar `copilot-setup-steps.yml` para otra pila tecnológica.

## Recursos

- [GitHub Copilot][github-copilot]
- [Acerca de los agentes de Copilot][copilot-agents]
- [Asignar incidencias de GitHub a Copilot][assign-issue]
- [Prácticas recomendadas para el flujo de configuración del agente en la nube de Copilot][cloud-agent-best-practices]
- [Configurar el firewall del agente en la nube de Copilot][agent-firewall]

---

| [← Lección anterior: Gestionar agentes][previous-lesson] |
|:--|

[github-copilot]: https://github.com/features/copilot
[cloud-agent-overview]: https://docs.github.com/copilot/concepts/agents/cloud-agent/about-cloud-agent
[assign-issue]: https://docs.github.com/copilot/how-tos/use-copilot-agents/cloud-agent/start-copilot-sessions
[setup-workflow]: https://docs.github.com/copilot/how-tos/copilot-on-github/customize-copilot/customize-cloud-agent/customize-the-agent-environment
[copilot-agents]: https://docs.github.com/copilot/concepts/agents/cloud-agent/about-cloud-agent
[cloud-agent-best-practices]: https://docs.github.com/copilot/how-tos/copilot-on-github/customize-copilot/customize-cloud-agent/customize-the-agent-environment
[agent-firewall]: https://docs.github.com/copilot/how-tos/copilot-on-github/customize-copilot/customize-cloud-agent/customize-the-agent-firewall

[previous-lesson]: ../4-managing-agents/
