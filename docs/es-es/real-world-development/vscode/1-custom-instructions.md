---
title: "Ejercicio 1: Instrucciones personalizadas"
authors:
  - geektrainer
lastUpdated: 2026-06-30
---

[← Lección anterior: Requisitos previos][previous-lesson] · [Siguiente lección: Modo agente →][next-lesson]

El contexto es fundamental cuando se trabaja con inteligencia artificial (IA) generativa. Si una tarea debe realizarse de una manera concreta o Copilot necesita conocer información de fondo, ese contexto debe estar disponible. Hay varias formas de compartir contexto específico con Copilot. Una de las principales son los [archivos de instrucciones][instruction-files], que proporcionan directrices sobre la generación de código.

En este ejercicio:

- explorarás cómo llegan a Copilot el contexto específico del proyecto, las directrices de programación y los estándares de documentación mediante las instrucciones personalizadas del repositorio y los archivos de instrucciones aplicables a rutas concretas.
- generarás la primera parte de la capa de datos del filtrado (un asistente para las editoriales) con las instrucciones actuales.
- añadirás un estándar global al archivo `.github/copilot-instructions.md`.
- volverás a enviar el mismo prompt y observarás cómo el código generado adopta el nuevo estándar.
- confirmarás el cambio de instrucciones y la parte del filtrado en `main` para que Copilot pueda usar las directrices actualizadas en el siguiente ejercicio.

> [!CAUTION]
> El código generado puede no seguir algunos de los estándares indicados. Copilot es probabilístico. El objetivo es observar la *tendencia* del cambio de comportamiento tras actualizar las instrucciones, no obtener exactamente el mismo código que en los ejemplos.

## Archivos de instrucciones

### Escenario

Tailspin Toys tiene un conjunto de directrices y requisitos para las prácticas de desarrollo:

- La capa de datos siempre debe tener pruebas unitarias.
- La interfaz de usuario debe usar el modo oscuro y tener un aspecto moderno.
- El código debe incluir comentarios de documentación TSDoc.
- Cada archivo debe empezar con un bloque de comentarios que explique su finalidad.

Con los archivos de instrucciones, proporcionarás a Copilot la información necesaria para que realice las tareas de acuerdo con estas prácticas.

### Instrucciones personalizadas

Las instrucciones personalizadas proporcionan a Copilot contexto y preferencias para que comprenda mejor tu estilo de programación y tus requisitos. Permiten orientar sus sugerencias y fragmentos de código. Puedes especificar las convenciones de programación, las bibliotecas y los comentarios que prefieres. También puedes crear instrucciones para todo el repositorio o para tipos de archivos concretos.

Hay dos tipos de archivos de instrucciones:

- `.github/copilot-instructions.md`, un único archivo de instrucciones que se envía a Copilot en **todas** las solicitudes del repositorio. Debe incluir información del proyecto relevante para la mayoría de las solicitudes de chat o CLI: la pila tecnológica, una descripción general del proyecto, las prácticas recomendadas y otras directrices globales.
- Los archivos `.github/instructions/*.instructions.md`, que se pueden crear para tareas o tipos de archivos concretos. Permiten proporcionar directrices para lenguajes como TypeScript o Astro, así como tareas como crear un componente de interfaz o pruebas unitarias.

> [!NOTE]
> En el entorno de desarrollo integrado (IDE), los archivos de instrucciones se usan para generar código en Copilot Chat, pero no para completar código ni para las sugerencias de edición siguiente.
>
> Copilot Chat, Copilot CLI y el agente en la nube de Copilot utilizan las instrucciones del repositorio y los archivos `*.instructions.md` que incluyen el frontmatter `applyTo` al generar código.
>
> Copilot también [admite archivos de instrucciones en otros formatos][custom-instructions-support], como AGENTS.md y CLAUDE.md.

### Prácticas recomendadas para gestionar archivos de instrucciones

Crear archivos de instrucciones queda fuera del alcance de esta conversación. Sin embargo, el proyecto de ejemplo muestra un enfoque representativo:

- Mantén las instrucciones de `copilot-instructions.md` centradas en las directrices generales del proyecto, como su descripción, estructura y estándares globales de programación.
- Usa archivos `*.instructions.md` para dar instrucciones específicas sobre tipos de archivo (pruebas unitarias, componentes Astro o la capa de datos) o tareas.
- Usa lenguaje natural y directrices claras. Incluye ejemplos de cómo debería y no debería ser el código.

No hay una única forma de crear archivos de instrucciones ni de usar la IA. Experimenta para descubrir qué funciona mejor para tu proyecto.

> [!TIP]
> Todo proyecto que use GitHub Copilot debería tener una colección sólida de archivos de instrucciones. Al explorar los de este proyecto, encontrarás archivos para numerosas tareas, como las [actualizaciones de la interfaz][ui-instructions] y [Astro][astro-instructions].
>
> Copilot también puede ayudarte a generar archivos de instrucciones. Cada entorno ofrece esta función de forma distinta (por ejemplo, **Configure Chat → Generate Agent Instructions** en VS Code o `/init` en Copilot CLI); la lección del entorno que estés usando indicará cuándo corresponde.
>
> Si buscas plantillas o un punto de partida, explora [awesome-copilot][awesome-copilot], un repositorio con archivos de instrucciones, agentes personalizados y otros recursos.

[ui-instructions]: https://github.com/github-samples/tailspin-toys/blob/main/.github/instructions/ui.instructions.md
[astro-instructions]: https://github.com/github-samples/tailspin-toys/blob/main/.github/instructions/astro.instructions.md
[awesome-copilot]: https://github.com/github/awesome-copilot
[custom-instructions-support]: https://docs.github.com/copilot/reference/custom-instructions-support

## Explora los archivos de instrucciones personalizadas del proyecto

Dedica un momento a leer los archivos de instrucciones que incluye este repositorio: hay un archivo `copilot-instructions.md` principal y varios archivos `*.instructions.md` para distintas tareas. Ábrelos en el editor o en la interfaz web de GitHub.

1. Abre `.github/copilot-instructions.md`.
2. Explora el archivo y fíjate en la breve descripción del proyecto y secciones como **Agent notes**, **Code standards**, **Scripts** y **Repository Structure**. En **Code standards**, observa las directrices anidadas de **GitHub Actions Workflows**. Se aplican a cualquier interacción con Copilot.
3. Abre la carpeta `.github/instructions` y explora su contenido. Hay instrucciones para archivos Astro, la capa de datos Drizzle, las pruebas y otros temas.
4. Abre `.github/instructions/unit-tests.instructions.md`. Fíjate en el campo `applyTo` de la parte superior, que define un patrón glob relativo a la raíz del repositorio para determinar los archivos a los que se aplican las instrucciones. En este caso, coincide con cualquier archivo de prueba TypeScript, como los que cumplen el patrón `**/*.test.ts`.
5. Fíjate en las instrucciones específicas para crear pruebas unitarias en este proyecto.
6. Abre `.github/instructions/drizzle.instructions.md` y desplázate hasta el final. Observa los enlaces a otros archivos de instrucciones, como `unit-tests.instructions.md`, y a archivos existentes del proyecto. Así puedes dividir conjuntos extensos de instrucciones en archivos más pequeños y reutilizables, y señalar a Copilot ejemplos que pueda seguir. Las rutas son relativas al archivo de instrucciones, no a la raíz del repositorio.

> [!NOTE]
> La sección **Code formatting requirements** de `copilot-instructions.md` documenta los estándares de programación del proyecto, pero aún no exige documentación en el código. En los pasos siguientes añadirás reglas para los comentarios TSDoc y los encabezados de comentarios de los archivos.

## Crea una rama para los cambios

Sigue las prácticas recomendadas y crea una rama para los cambios.

1. Vuelve al codespace del ejercicio anterior.
2. Abre un terminal con <kbd>Control</kbd>+<kbd>\`</kbd> (Mac) o <kbd>Ctrl</kbd>+<kbd>\`</kbd> (Windows/Linux).
3. Crea una rama nueva y cámbiate a ella:

   ```bash
   git checkout -b custom-instructions
   ```

## Usa Copilot Chat *antes* de actualizar las instrucciones

> [!TIP]
> **Abre Copilot Chat**
>
> Antes de empezar los ejercicios siguientes, vuelve al codespace, abre el panel de Copilot Chat y selecciona **New Chat** para iniciar una conversación nueva. La selección de modo y modelo varía según el ejercicio; cada paso la indicará cuando sea necesario.

Para ver el efecto de las instrucciones personalizadas, primero enviarás un prompt con el archivo de instrucciones actual. Más adelante lo actualizarás y volverás a enviar el mismo prompt para comparar el resultado.

1. Cierra las pestañas abiertas de ejercicios anteriores para que Copilot use solo el contexto que necesitas.
2. Abre `src/lib/publishers.ts` para indicar a Copilot dónde debe ir el asistente.
3. Selecciona **Agent** en el menú desplegable de agentes de la vista Chat para que Copilot pueda aplicar los cambios en los archivos.

   ![Captura del selector de agente en la vista Chat](../../../_images/shared-chat-mode-selector.png)

4. Envía este prompt:

   ```plaintext
   Crea o actualiza src/lib/publishers.ts con un asistente de acceso a datos que devuelva una lista de todas las editoriales con su nombre e identificador. Aplica los cambios al archivo.
   ```

5. Copilot explora el proyecto y aplica los cambios de código, que pueden afectar al archivo del asistente y a sus pruebas.
6. Observa que el asistente propuesto es una función con tipos, que recibe un cliente `db` como primer argumento y devuelve una matriz con tipos de editoriales. Esto se debe a las convenciones de la capa de datos de `.github/instructions/drizzle.instructions.md`, que se aplica a `src/lib/*.ts`.
7. Observa que al código propuesto **le faltan** los comentarios de documentación TSDoc y el encabezado de comentarios del archivo.

> [!CAUTION]
> Como Copilot es probabilístico, quizá añada comentarios de documentación aunque no se lo hayas pedido. No pasa nada: el objetivo es observar la mejora de *coherencia* después de actualizar las instrucciones.

## Añade un estándar al repositorio

Como ya has visto, `.github/copilot-instructions.md` proporciona a Copilot información general del proyecto. Añadirás los estándares de programación para mejorar las sugerencias de código.

1. Vuelve a abrir `.github/copilot-instructions.md`.
2. Busca la sección **Code formatting requirements**, que debería estar cerca de la línea 27. Ahí se documentan los estándares de programación, pero aún no hay reglas para la documentación del código; por eso el asistente generado no incluía comentarios.
3. Añade estas líneas Markdown justo debajo de los estándares existentes para pedir a Copilot que incluya encabezados de comentarios de archivo y comentarios de documentación TSDoc:

   ```markdown
   - Cada función exportada debe incluir un comentario TSDoc que describa su finalidad, sus parámetros y el valor que devuelve.
   - Antes de las importaciones o de cualquier código, añade al archivo un bloque de comentarios que explique su finalidad.
   ```

4. Guarda `copilot-instructions.md`.

> [!TIP]
> Como has visto en la lección anterior, puedes crear archivos de instrucciones en `.github/copilot-instructions.md` para directrices globales o como archivos `*.instructions.md` para lenguajes, tipos de archivo o tareas concretas. El archivo del repositorio es el lugar adecuado para estándares generales, como la regla de comentarios de documentación que acabas de añadir.

## Vuelve a enviar el prompt y observa el cambio

Repetirás el prompt anterior para comprobar si Copilot adopta las nuevas directrices.

1. Vuelve a Copilot Chat y selecciona **New Chat** para borrar el contexto de la conversación.
2. Selecciona `src/lib/publishers.ts` para que Copilot se centre en el archivo correcto.
3. Envía el **mismo prompt** que antes:

   ```plaintext
   Crea o actualiza src/lib/publishers.ts con un asistente de acceso a datos que devuelva una lista de todas las editoriales con su nombre e identificador. Aplica los cambios al archivo.
   ```

4. Observa que el archivo propuesto ahora empieza con un bloque de comentarios parecido a este:

   ```typescript
   /**
    * Asistentes de acceso a datos de editoriales para la plataforma de financiación colectiva Tailspin Toys.
    * Proporciona funciones para recuperar información de las editoriales de la base de datos.
    */
   ```

5. Observa que la función propuesta ahora incluye un comentario TSDoc parecido a este:

   ```typescript
   /**
    * Devuelve una lista de todas las editoriales con su identificador y nombre.
    *
    * @param db - Cliente de base de datos Drizzle.
    * @returns Promesa que se resuelve en una matriz de objetos de editorial.
    */
   ```

Has orientado a Copilot para que siga un nuevo estándar y lo aplique a código real que servirá de base para el próximo ejercicio.

## Confirma las instrucciones y sube la rama

Los archivos de instrucciones, como cualquier otro recurso del repositorio, se gestionan mediante el control de código fuente.

1. Abre un terminal con <kbd>Control</kbd>+<kbd>\`</kbd> (Mac) o <kbd>Ctrl</kbd>+<kbd>\`</kbd> (Windows/Linux).
2. Ejecuta `git status` en el terminal para confirmar que están presentes las actualizaciones de instrucciones y del asistente:

   ```bash
   git status
   ```

3. Desde el terminal, prepara y confirma las actualizaciones de instrucciones y todos los archivos que Copilot haya modificado para crear la base del asistente:

   ```bash
   git add .github/copilot-instructions.md src/lib/publishers.ts
   # Si git status muestra otras actualizaciones auxiliares (por ejemplo, pruebas), añade también esos archivos.
   git commit -m "Add doc comment standards and publishers helper foundation"
   ```

4. Sube la rama al repositorio:

   ```bash
   git push -u origin custom-instructions
   ```

## Crea y combina una solicitud de incorporación de cambios

Con la rama publicada, crearás una solicitud de incorporación de cambios vinculada a la incidencia de tu backlog sobre los estándares de documentación. Puedes hacerlo manualmente, pero pedirás a Copilot que busque la incidencia, cree una PR para resolverla y la combine.

> [!NOTE]
> En este ejercicio Copilot interactuará por primera vez con GitHub en tu nombre. La plantilla del proyecto ya incluye las herramientas de GitHub, así que no tienes que configurar nada. Es posible que VS Code te pida iniciar sesión la primera vez que Copilot utilice una de esas herramientas. Sigue las instrucciones para permitirlo. En un ejercicio posterior aprenderás cómo funciona esta conexión (Model Context Protocol).

1. Abre Copilot Chat en el codespace.
2. Pulsa <kbd>Control</kbd>+<kbd>Command</kbd>+<kbd>I</kbd> (Mac) o <kbd>Ctrl</kbd>+<kbd>Alt</kbd>+<kbd>I</kbd> (Windows/Linux) para abrir la vista Copilot Chat y comprueba que **Agent** está seleccionado en el selector de agentes.
3. Pide a Copilot que busque la incidencia sobre la actualización de los archivos de instrucciones y cree una PR desde la rama actual. Indícale que describa tanto la actualización de las instrucciones como la base del asistente de editoriales:

   ```plaintext
   Busca la incidencia relacionada con la actualización del archivo de instrucciones. Crea una solicitud de incorporación de cambios desde la rama actual, indica que la resuelve e incluye que también hemos añadido la base del asistente de editoriales para el próximo trabajo de filtrado.
   ```

4. Copilot empezará a buscar la incidencia y crear la PR.
5. Cuando Copilot te pida permiso para realizar acciones de GitHub, revisa el comando y selecciona **Allow** si corresponde.
6. Una vez creada la PR, pide a Copilot que la combine y vuelva a `main`:

   ```plaintext
   Combina la solicitud de incorporación de cambios en main. Después, vuelve localmente a main y descarga el código más reciente para tener la rama actualizada.
   ```

7. Cuando Copilot te pida permiso para realizar acciones de GitHub y ejecutar comandos del shell, revisa el comando y selecciona **Allow** si corresponde.

¡Has creado y combinado una solicitud de incorporación de cambios con GitHub Copilot!

## Resumen y pasos siguientes

Has explorado cómo Copilot obtiene contexto de los archivos de instrucciones del proyecto y has usado Copilot Chat en VS Code para:

- enviar un prompt de generación de código y observar qué produce con las instrucciones *existentes*.
- añadir un estándar global al archivo `.github/copilot-instructions.md`.
- volver a enviar el mismo prompt y observar cómo el código propuesto adopta el nuevo estándar.
- confirmar las actualizaciones de instrucciones y la base del asistente en `main` para que el siguiente ejercicio pueda utilizarlas.

A continuación, aplicarás esas instrucciones en el [modo agente][next-lesson] mientras Copilot añade una funcionalidad nueva a la base de código.

## Recursos

- [Archivos de instrucciones para personalizar GitHub Copilot][instruction-files]
- [Prácticas recomendadas para crear instrucciones personalizadas][instructions-best-practices]
- [Cinco consejos para redactar mejores instrucciones personalizadas para Copilot][copilot-instructions-five-tips]
- [Instrucciones personales personalizadas para GitHub Copilot][personal-instructions]
- [Awesome Copilot: colección de archivos de instrucciones y otros recursos][awesome-copilot]

[previous-lesson]: ../0-prerequisites/
[next-lesson]: ../2-agent-mode/
[instruction-files]: https://code.visualstudio.com/docs/copilot/copilot-customization
[instructions-best-practices]: https://docs.github.com/enterprise-cloud@latest/copilot/using-github-copilot/coding-agent/best-practices-for-using-copilot-to-work-on-tasks#adding-custom-instructions-to-your-repository
[personal-instructions]: https://docs.github.com/copilot/customizing-copilot/adding-personal-custom-instructions-for-github-copilot
[copilot-instructions-five-tips]: https://github.blog/ai-and-ml/github-copilot/5-tips-for-writing-better-custom-instructions-for-copilot/
