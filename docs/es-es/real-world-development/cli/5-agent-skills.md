---
title: "Lección 5: Personalizar y usar una habilidad quality-checks"
description: "Explora la habilidad quality-checks existente, personaliza el formato del informe y úsala para validar el filtrado."
authors:
  - geektrainer
lastUpdated: 2026-09-29
---

Programar no consiste solo en escribir código. Ya hemos comprobado manualmente que el código funciona y hemos usado archivos de instrucciones para garantizar que sigue los estándares. ¿Y las pruebas, el lint y los demás aspectos de la integración continua (CI)?

Las **habilidades de agente** son adecuadas para este tipo de tareas. Ayudan a Copilot a entender cómo ejecutarlas correctamente.

En esta lección:

- explorarás la habilidad `quality-checks` existente.
- personalizarás el formato de los resultados.
- volverás a cargar y ejecutarás la habilidad.

## Escenario

Tailspin Toys usa la habilidad `quality-checks` para ejecutar pruebas unitarias, lint y comprobaciones de tipos. El equipo quiere mejorar el informe para que los resultados sean más fáciles de leer.

## Instrucciones, scripts y recursos

Las habilidades de agente agrupan instrucciones reutilizables para tareas, scripts ejecutables y recursos de apoyo que el agente carga cuando los necesita. En esencia, una habilidad es una carpeta que lleva su nombre e incluye un archivo Markdown llamado `SKILL.md`. El archivo Markdown contiene un frontmatter con el nombre y la descripción de la habilidad, una descripción general de sus funciones y directrices sobre cuándo debe utilizarse. También puede incluir subcarpetas con scripts y otros recursos.

> [!NOTE]
> No es obligatorio incluir carpetas y archivos adicionales. La habilidad `quality-checks` de Tailspin Toys solo contiene `SKILL.md` porque utiliza los comandos existentes del proyecto.

Las habilidades pueden estar en la carpeta `.github/skills` del proyecto para que el equipo las comparta y reutilice como recursos del repositorio, o en la carpeta de habilidades del usuario `~/.copilot/skills`.

## Explora la habilidad

Explora la habilidad `quality-checks` que creó el equipo de Tailspin Toys para ejecutar pruebas unitarias, lint y comprobaciones de tipos.

1. Vuelve a tu Codespace. En el editor de Codespaces, abre `.github/skills/quality-checks/SKILL.md`.
2. Lee los campos `name` y `description` de la parte superior. La descripción ayuda a Copilot a saber cuándo debe usar la habilidad.
3. Lee las instrucciones y fíjate en cómo orientan a Copilot durante las pruebas y el lint.
4. Observa que la habilidad todavía no tiene una sección **Results output formatting**.

## Ejecuta la habilidad antes de modificarla

Puedes invocar las habilidades directamente desde Copilot CLI o usar lenguaje natural para pedir que se ejecuten. Pide a Copilot que ejecute las tres comprobaciones de la habilidad.

1. Vuelve a la conversación de filtrado en modo Interactive.
2. Envía este prompt:

   ```plaintext
   Ejecuta la habilidad quality-checks para las pruebas unitarias, el lint y las comprobaciones de tipos.
   ```

3. Revisa el informe final.

## Personaliza el informe

El informe debería indicar qué se ejecutó, si se completó correctamente y qué resultados proporcionaron realmente las herramientas. Actualizarás la habilidad para que genere ese informe.

1. Vuelve a abrir `.github/skills/quality-checks/SKILL.md`.
2. Añade esta sección al final del archivo:

   ```markdown
   ## Formato de los resultados

   Al terminar, informa de cada comando ejecutado e indica si se ha superado, ha fallado o se ha bloqueado. Incluye el número de pruebas, las duraciones, los errores, las advertencias y otras métricas solo si la herramienta las proporciona. Indica el siguiente paso para cada fallo o bloqueo y nunca describas una comprobación omitida o incompleta como superada.
   ```

3. El archivo se guarda automáticamente.

## Ejecuta la habilidad actualizada

Ya has actualizado la habilidad. Copilot CLI puede volver a cargar las habilidades editadas sin que tengas que reiniciar la conversación.

1. Escribe:

   ```plaintext
   /skills reload
   ```

2. Envía exactamente el mismo prompt que antes:

   ```plaintext
   Ejecuta la habilidad quality-checks para las pruebas unitarias, el lint y las comprobaciones de tipos.
   ```

3. Revisa el informe final y compáralo con el primero.

## Resumen y pasos siguientes

Has personalizado y usado una habilidad de agente existente. En esta lección:

- has explorado la habilidad `quality-checks` para ejecutar pruebas unitarias, lint y comprobaciones de tipos.
- has personalizado el formato de los resultados.
- has vuelto a cargar y ejecutado la habilidad.

El cambio se incluirá con el filtrado en la PR de la funcionalidad. A continuación, permitirás que Copilot interactúe directamente con el sitio mediante el [servidor MCP de Playwright][next-lesson].

## Más ejemplos de habilidades

Estos ejemplos de la comunidad son referencias, no tareas adicionales:

- [Especificación de Agent Skills][skill-spec]
- [Flujo de contribución: `make-repo-contribution`][contribution-example]
- [Documentos de requisitos: `prd`][prd-example]
- [Diagramas y script de exportación incluido: `drawio`][drawio-example]
- [Pruebas en el navegador: `webapp-testing`][browser-example]

[previous-lesson]: ../4-custom-instructions/
[next-lesson]: ../6-mcp-playwright/
[skill-spec]: https://agentskills.io/specification
[contribution-example]: https://github.com/github/awesome-copilot/tree/main/skills/make-repo-contribution
[prd-example]: https://github.com/github/awesome-copilot/tree/main/skills/prd
[drawio-example]: https://github.com/github/awesome-copilot/tree/main/skills/drawio
[browser-example]: https://github.com/github/awesome-copilot/tree/main/skills/webapp-testing
