---
title: "Lección 4: Guiar a Copilot con instrucciones personalizadas"
description: "Explora las instrucciones del repositorio, añade un estándar de documentación y aplícalo al código de filtrado."
authors:
  - geektrainer
lastUpdated: 2026-09-18
---

El contexto es fundamental cuando se trabaja con inteligencia artificial (IA) generativa. Si una tarea debe realizarse de una forma concreta, conviene que Copilot tenga disponibles las directrices. Los [archivos de instrucciones][instruction-files] indican no solo *qué* código quieres, sino también *cómo* debe estructurarse. Ahora que has desarrollado el filtrado, explorarás las instrucciones que usó Copilot, añadirás un estándar de documentación y lo aplicarás al código.

En esta lección:

- explorarás cómo llegan al agente las instrucciones del repositorio y los archivos de instrucciones aplicables a rutas específicas.
- actualizarás el archivo de instrucciones para que se respeten los estándares de programación.
- observarás el efecto de los archivos de instrucciones en el código.

## Escenario

Tailspin Toys, como cualquier buena empresa de desarrollo, tiene una serie de directrices y requisitos para las prácticas de desarrollo:

- Los comentarios deben explicar la intención y las decisiones no obvias, no repetir el código.
- Las funciones exportadas de `db/` y `src/lib/` deben documentar su finalidad, sus parámetros y sus valores devueltos con TSDoc/JSDoc, incluido un argumento `db` inyectable cuando corresponda.
- Los componentes reutilizables de Astro deben documentar sus contratos `Props`, y los comentarios deben mantenerse actualizados cuando cambie el código relacionado.
- Se deben conservar las directrices de formato y lint existentes.

Con los archivos de instrucciones, darás a Copilot la información necesaria para que realice las tareas de acuerdo con estas prácticas.

## Archivos de instrucciones

Las instrucciones personalizadas proporcionan a Copilot contexto y preferencias para que pueda comprender mejor tu estilo de programación y tus requisitos. Permiten guiar las sugerencias y los fragmentos de código. Puedes especificar las convenciones de programación, las bibliotecas y los comentarios que prefieres. También puedes crear instrucciones para todo el repositorio o para tipos de archivos concretos.

Hay dos tipos de archivos de instrucciones:

- `.github/copilot-instructions.md`, un único archivo de instrucciones que se envía a Copilot en **todas** las solicitudes del repositorio. Debe incluir información general del proyecto que resulte pertinente para la mayoría de las solicitudes de chat o CLI.
- Los archivos `.github/instructions/*.instructions.md`, que contienen directrices para lenguajes, tipos de archivos o tareas concretos.

> [!NOTE]
> La compatibilidad con otros formatos de instrucciones varía según el entorno. Consulta la [referencia de compatibilidad con instrucciones personalizadas][custom-instructions-support] antes de utilizar un formato concreto.

## Explora los archivos de instrucciones personalizadas del proyecto

El proyecto inicial ya contiene varios archivos de instrucciones. Explóralos antes de hacer cambios para comprender su contenido.

1. Vuelve a tu Codespace.
2. En el editor de Codespaces, no en el terminal, abre `.github/copilot-instructions.md`.
3. Explora el archivo y fíjate en la breve descripción del proyecto y sus directrices de programación. Estas instrucciones se aplican a todas las interacciones de Copilot en este repositorio.
4. Abre la carpeta `.github/instructions` y explora sus archivos. Hay instrucciones para archivos Astro, la capa de datos Drizzle, las pruebas y otros temas.
5. Abre `.github/instructions/unit-tests.instructions.md`. Fíjate en el campo `applyTo` de la parte superior, que define un patrón glob para determinar a qué archivos se aplican las instrucciones.
6. Abre `.github/instructions/drizzle.instructions.md` y fíjate en sus enlaces a otros archivos de instrucciones y a archivos existentes del proyecto. Así puedes dividir las instrucciones extensas en archivos más pequeños y reutilizables, y señalar a Copilot ejemplos que pueda seguir.

## Actualiza los archivos de instrucciones para reflejar las directrices del equipo

Los archivos existentes son un buen comienzo, pero aún falta una directriz. Modificarás el archivo principal `copilot-instructions.md` para indicar que el TypeScript nuevo debe incluir comentarios TSDoc.

1. En `.github/copilot-instructions.md`, busca la sección **Code formatting guidance**, que debería estar alrededor de la línea 35.
2. Añade el siguiente elemento al final de la lista:

   ```markdown
   - All new TypeScript should contain TSDocs comments for documentation purposes.
   ```

El archivo se guardará automáticamente.

## Aplica las directrices actualizadas

Copilot CLI carga las instrucciones del repositorio al iniciar una conversación. Después de guardar las instrucciones actualizadas, sal de la conversación de filtrado y reanúdala para que Copilot cargue las nuevas directrices sin perder el contexto de la funcionalidad.

1. Si la conversación de filtrado sigue abierta, escribe `/exit` para cerrarla. Copilot CLI guarda las conversaciones automáticamente.
2. Desde la raíz del repositorio en la rama `game-filters-cli`, reanuda la conversación guardada:

   ```bash
   copilot --resume
   ```

   Si se te solicita, selecciona la conversación de filtrado y confirma que está seleccionada la rama `game-filters-cli`.
3. Pide a Copilot que aplique las directrices actualizadas:

   ```plaintext
   Acabamos de actualizar las instrucciones y las directrices de código. ¿Puedes actualizar el código generado para que las cumpla?
   ```

4. Escribe `/diff` y revisa los archivos TypeScript modificados. Fíjate en los comentarios TSDoc nuevos y confirma que describen el código correctamente.

## Resumen y pasos siguientes

Has explorado cómo Copilot CLI utiliza el contexto de los archivos de instrucciones y has aplicado un nuevo estándar a la funcionalidad. En concreto:

- has explorado cómo llegan al agente las instrucciones del repositorio y los archivos de instrucciones aplicables a rutas específicas.
- has actualizado el archivo de instrucciones para garantizar que se respetan los estándares de programación.
- has observado el efecto de los archivos de instrucciones en el código.

A continuación, [personalizarás y ejecutarás la habilidad reutilizable `quality-checks`][next-lesson] para que las pruebas y el lint se ejecuten de forma coherente.

## Recursos

- [Añadir instrucciones personalizadas para Copilot CLI][instruction-files]
- [Compatibilidad con instrucciones personalizadas][custom-instructions-support]
- [Prácticas recomendadas para crear instrucciones personalizadas][instructions-best-practices]
- [Awesome Copilot: colección de archivos de instrucciones y otros recursos][awesome-copilot]

[instruction-files]: https://docs.github.com/copilot/how-tos/copilot-cli/customize-copilot/add-custom-instructions
[instructions-best-practices]: https://docs.github.com/copilot/concepts/prompting/response-customization#writing-effective-custom-instructions
[awesome-copilot]: https://awesome-copilot.github.com/
[custom-instructions-support]: https://docs.github.com/copilot/reference/custom-instructions-support
[previous-lesson]: ../3-agent-modes/
[next-lesson]: ../5-agent-skills/
