---
title: "Ejercicio 2: Añadir funcionalidad con el modo agente de Copilot"
authors:
  - geektrainer
lastUpdated: 2026-06-30
---

Incluso las actualizaciones más sencillas suelen afectar a varios archivos y requerir tareas como ejecutar pruebas. Como desarrollador, normalmente tienes que localizar todos los archivos necesarios, hacer los cambios, ejecutar las pruebas, depurar, averiguar qué archivo falta y volver a modificarlo. El proceso continúa.

Aquí es donde entra en juego el modo agente de Copilot.

El modo agente de Copilot está diseñado para trabajar con más autonomía en el entorno de desarrollo integrado (IDE). Se comporta de forma parecida a un desarrollador: empieza explorando la estructura del proyecto, realiza las actualizaciones necesarias, ejecuta tareas como las pruebas y corrige automáticamente los errores que encuentra. Ahora verás cómo usar el modo agente para añadir funcionalidad al sitio.

> [!NOTE]
> Aunque sus nombres se parecen, el modo agente y el agente en la nube ofrecen experiencias distintas. El modo agente trabaja en el IDE, lo que permite obtener comentarios e iterar rápidamente. El agente en la nube trabaja de forma asíncrona como un miembro del equipo y se comunica contigo mediante incidencias y solicitudes de incorporación de cambios.

En este ejercicio aprenderás cómo:

- Copilot en modo agente puede explorar el proyecto, identificar los archivos pertinentes y coordinar cambios.
- GitHub Copilot en modo agente puede implementar funcionalidades en la interfaz y la capa de datos.
- revisar los cambios y las pruebas que genera Copilot en modo agente antes de combinarlos en la base de código.

## Escenario

A medida que crece la lista de juegos, quieres que los usuarios puedan filtrar por categoría y editorial. En el ejercicio anterior ya creaste un asistente de editoriales; ahora completarás el trabajo restante en la capa de datos, la interfaz y las pruebas con el modo agente de Copilot.

## Ejecuta el sitio web de Tailspin Toys

Antes de hacer cambios, explora el sitio web de Tailspin Toys para conocer sus funciones actuales.

El sitio web es una plataforma de financiación colectiva para juegos de mesa con temática de desarrollo. Permite mostrar juegos y sus detalles. Es una única aplicación Astro que genera páginas de lenguaje de marcado de hipertexto (HTML) estáticas durante la compilación. Las páginas consultan directamente una base de datos SQLite local desde su frontmatter mediante Drizzle ORM; no hay una interfaz de programación de aplicaciones (API) de backend independiente ni un framework de interfaz del lado del cliente. Los asistentes reutilizables para acceder a los datos están en `src/lib/`, y la interactividad se implementa con un pequeño elemento `<script>` de Astro que usa las interfaces estándar del modelo de objetos del documento (DOM).

### Inicia el sitio web

Para facilitar el inicio del sitio, se ha preparado la tarea `npm run dev`, que ejecuta el servidor de desarrollo único de Astro. Puedes iniciarla en tu GitHub Codespace siguiendo estos pasos:

1. Vuelve a tu codespace. Después del ejercicio anterior, deberías estar en `main`.
2. Abre un terminal en el Codespace con <kbd>Control</kbd>+<kbd>\`</kbd> (Mac) o <kbd>Ctrl</kbd>+<kbd>\`</kbd> (Windows/Linux).
3. Crea una rama nueva para el trabajo de filtrado:

   ```bash
   git checkout main
   git pull
   git checkout -b filtering-vscode
   ```

4. Ejecuta este comando para iniciar el sitio web:

   ```bash
   npm run dev
   ```

   Cuando el servidor de desarrollo de Astro esté listo, verás un aviso con la URL, parecido al siguiente:

   ```bash
   🚀 Tailspin Toys is ready!
      Astro server: http://localhost:4321
      Press Ctrl-C to stop.
   ```

> [!NOTE]
> Si aparece un cuadro de diálogo que te ofrece abrir una ventana del navegador para `http://localhost:4321`, ciérralo seleccionando **x**.

5. Para abrir el sitio web, mantén pulsada la tecla <kbd>Command</kbd> (Mac) o <kbd>Ctrl</kbd> (Windows/Linux) y selecciona la dirección del servidor Astro `http://localhost:4321` en el terminal.

> [!NOTE]
> En un codespace, al seleccionar el enlace a la URL de localhost desde el terminal de Codespaces se te redirige automáticamente a `https://<your-codespace-name>-4321.app.github.dev/`. Es un túnel privado al codespace, que ahora aloja el servidor web.

### Explora el sitio web

Con el sitio web en ejecución, puedes explorar sus funciones principales:

- **Página de inicio**: muestra una lista de juegos de mesa con sus títulos, imágenes y descripciones.
- **Página de detalles del juego**: al seleccionar un juego, se abre una página con más información, como el título, la descripción, la editorial y la categoría.

## Explora el backlog con Copilot

> [!TIP]
> **Abre Copilot Chat**
>
> Antes de empezar los ejercicios siguientes, vuelve al codespace, abre el panel de Copilot Chat y selecciona **New Chat** para iniciar una conversación limpia. La selección de modo y modelo varía en cada ejercicio; los pasos indicarán cuándo sea importante.

La implementación inicial del sitio funciona, pero queremos añadir funciones. Para empezar, revisaremos el backlog. Al crear el proyecto desde la plantilla, se generó un backlog de incidencias automáticamente. Pide a GitHub Copilot que te muestre esas incidencias.

1. Selecciona **Agent** en el menú desplegable de agentes de la vista Chat. El agente **Agent** puede planificar e implementar cambios en varios archivos de forma autónoma, ejecutar comandos del terminal e invocar herramientas.

   ![Captura del selector de agente en la vista Chat](../../../_images/shared-chat-mode-selector.png)

2. Selecciona **Claude Sonnet 4.5** en la lista de modelos disponibles.

> [!CAUTION]
> Los autores de este taller no expresan una preferencia por un modelo concreto. Durante su desarrollo se usó Claude Sonnet 4.5 para que las sugerencias de código fueran relativamente coherentes y la experiencia resultara adecuada. Como los modelos de lenguaje grandes (LLM) son probabilísticos, las sugerencias pueden diferir de las instrucciones del taller. Es normal y previsible.

> [!NOTE]
> Debido a la naturaleza probabilística de los LLM, Copilot puede usar un comando distinto del Model Context Protocol (MCP), pero debería poder completar la tarea.

3. Envía a Copilot este prompt para consultar el backlog de incidencias:

   ```plaintext
   Enséñame el backlog de elementos del repositorio de GitHub. Ayúdame a priorizarlos según cuáles resulten más útiles para los usuarios.
   ```

4. Selecciona **Continue** para ejecutar el comando que muestra todas las incidencias.
5. Revisa la lista generada.

Observa que Copilot ha priorizado los elementos según cuáles considera más útiles para los usuarios.

## Revisa los archivos de instrucciones

Antes de pedir al agente que genere el código, revisa el archivo de instrucciones que proporciona contexto a Copilot. Usarás el archivo de [interfaz de usuario (UI)](https://github.com/github-samples/tailspin-toys/blob/main/.github/instructions/ui.instructions.md), que explica cómo abordar la incorporación de funciones al sitio web.

1. En el codespace, abre `.github/instructions/ui.instructions.md`.
2. Fíjate en las directrices generales para añadir funciones, que incluyen:
   - una descripción general de la arquitectura.
   - principios de diseño de componentes, capacidad de prueba y accesibilidad.
   - enlaces a archivos de instrucciones específicos para varios tipos de archivos, incluidos Astro, Tailwind CSS y Drizzle.

> [!TIP]
> Los archivos de instrucciones permiten hacer referencia a otros archivos de instrucciones y a archivos del proyecto. Las rutas son relativas a la ubicación del archivo de instrucciones. Así se pueden reutilizar directrices, dividir instrucciones complejas en archivos más manejables e incluir ejemplos y plantillas.

## Implementa la funcionalidad de filtrado

Para completar el filtrado, habrá que hacer al menos tres cambios en la aplicación:

- añadir o perfeccionar la lógica de filtrado restante en la capa de datos (`src/lib/`).
- añadir o actualizar las pruebas del comportamiento del filtrado.
- actualizar la página de listado de juegos para incorporar la interfaz de filtrado.

Además, hay que ejecutar las pruebas y comprobar que se superan antes de combinar los cambios. El modo agente de Copilot puede encargarse de estas tareas.

1. Continúa en la conversación actual de Copilot o inicia una nueva seleccionando **New Chat**.
2. Selecciona **Add Context**, **Instructions** y **ui** como archivo de instrucciones.

   ![Captura de la selección del archivo de instrucciones de UI](../../../_images/ex3-select-instructions-file.png)

3. Comprueba que **Agent** sigue seleccionado en el menú desplegable de agentes de la vista Chat.

   ![Captura del selector de agente en la vista Chat](../../../_images/shared-chat-mode-selector.png)

4. Comprueba que **Claude Sonnet 4.5** sigue seleccionado como modelo.
5. Pide a Copilot que implemente la funcionalidad según la incidencia correspondiente del backlog con este prompt:

   ```plaintext
   Actualiza el sitio para permitir filtrar por editorial y categoría, según los requisitos de la incidencia correspondiente del backlog de GitHub. El asistente de editoriales del ejercicio anterior ya existe, así que consérvalo y mejóralo según sea necesario mientras completas el trabajo restante en la capa de datos, la interfaz y las pruebas. Asegúrate de que todas las pruebas se superan antes de terminar. El servidor ya está en ejecución, no hace falta que lo inicies.
   ```

6. Observa cómo Copilot explora el proyecto y localiza los archivos relacionados con la funcionalidad, incluidos los asistentes de la capa de datos, la interfaz y las pruebas. Después, modifica los archivos y ejecuta las pruebas.

   ![Captura de Copilot explorando los archivos del proyecto](../../../_images/ex3-agent-mode-explores.png)

> [!NOTE]
> Copilot realizará varias tareas, como explorar el proyecto, modificar archivos y ejecutar pruebas. El proceso puede tardar unos minutos, según la complejidad de la tarea y la base de código. Mientras tanto, quizá aparezcan los botones **Keep** y **Undo** en el editor. Cuando Copilot termine, tendrás la opción **Keep** o **Undo** para cada cambio; no hace falta seleccionarlas mientras el agente esté trabajando.

7. Cuando Copilot te lo solicite, selecciona **Continue** para ejecutar las pruebas.

   ![Captura de un cuadro de diálogo en el panel Copilot Chat que pide confirmar la ejecución de las pruebas](../../../_images/ex3-agent-mode-run-tests.png)

8. Puede haber pausas y algunas pruebas podrían fallar durante el proceso. No pasa nada: Copilot alterna entre la generación de código y la ejecución de pruebas hasta completar la tarea y dejar de detectar errores.

   ![Captura de una sesión completa de Copilot en modo agente](../../../_images/ex3-agent-mode-proposed-changes.png)

9. Explora el código generado para detectar posibles problemas.

> [!CAUTION]
> Recuerda que siempre es importante revisar el código generado por Copilot o cualquier otra herramienta de inteligencia artificial (IA).

10. Vuelve al navegador donde está abierto el sitio y explora la nueva funcionalidad.
11. Cuando hayas confirmado que todo funciona y revisado el código, selecciona **Keep** en la ventana de Copilot Chat.

## Resumen y pasos siguientes

¡Enhorabuena! En este ejercicio has explorado cómo usar el modo agente de GitHub Copilot para añadir nuevas funciones al sitio web de Tailspin Toys. Has aprendido cómo:

- GitHub Copilot en modo agente puede implementar funciones en la interfaz y la capa de datos.
- Copilot en modo agente puede explorar el proyecto, identificar los archivos pertinentes y coordinar cambios.
- revisar los cambios y las pruebas que genera Copilot en modo agente antes de combinarlos en la base de código.

Ahora [probarás la funcionalidad con el servidor MCP de Playwright][next-lesson] y abrirás una solicitud de incorporación de cambios.

### Ejercicio adicional: implementar la paginación

A medida que crece la lista de juegos, será necesario habilitar la paginación. Usa lo aprendido en este ejercicio y pide a Copilot que añada paginación al sitio. Al redactar el prompt:

- sigue las prácticas recomendadas, incluidos los archivos de instrucciones existentes.
- decide cómo quieres implementar la paginación y si el usuario podrá elegir el tamaño de página o será fijo.
- proporciona a Copilot las directrices necesarias para que la implementación se ajuste a tus preferencias.
- itera con GitHub Copilot y proporciona contexto cuando le pidas cambios; es lo habitual al trabajar con Copilot.

## Recursos

- [Modos ask, edit y agent de Copilot: qué hacen y cuándo usarlos][choose-mode]
- [Modo agente en VS Code][vs-code-agent-mode]

[previous-lesson]: ../1-custom-instructions/
[next-lesson]: ../3-mcp/
[choose-mode]: https://github.blog/ai-and-ml/github-copilot/copilot-ask-edit-and-agent-modes-what-they-do-and-when-to-use-them/
[vs-code-agent-mode]: https://code.visualstudio.com/docs/copilot/chat/chat-agent-mode
