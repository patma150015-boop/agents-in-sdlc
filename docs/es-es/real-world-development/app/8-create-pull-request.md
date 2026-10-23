---
title: "Lección 8 - Crear y combinar la solicitud de incorporación de cambios (PR) de la funcionalidad"
description: "Revisa conjuntamente el filtrado, las instrucciones, la actualización de la habilidad, el perfil de control de calidad (QA) y las pruebas; después, autoriza Agent Merge solo tras revisar los cambios."
authors:
  - geektrainer
lastUpdated: 2026-10-22
---

La implementación del filtrado, las actualizaciones de instrucciones y de la habilidad, el perfil de control de calidad (QA) y las pruebas están guardados en una sola rama. Es hora de revisarlos juntos y abrir una solicitud de incorporación de cambios. Ya has combinado personalmente la solicitud de incorporación de cambios (PR) de las valoraciones por estrellas; esta vez permitirás que **Agent Merge** gestione el proceso.

> [!NOTE]
> Normalmente separaríamos la funcionalidad, las actualizaciones de instrucciones y de la habilidad y el agente de QA en varias PR. Para agilizar el taller, has mantenido todo el flujo de filtrado y calidad en una sesión y una rama, y todo ese trabajo se incluirá en esta PR.

En esta lección:

- aprenderás qué es Agent Merge y cómo automatiza el ciclo de combinación.
- inspeccionarás la PR completa de la funcionalidad y las pruebas de verificación.
- autorizarás Agent Merge solo después de revisar los cambios y confirmarás que la PR se ha combinado.

## Escenario

A lo largo del flujo de filtrado has usado Copilot para planificar, implementar y verificar una funcionalidad. Ahora Tailspin Toys quiere automatizar el trabajo restante de la PR y mantener la autorización para combinar bajo el control del desarrollador.

## Presentación de Agent Merge

**Agent Merge** automatiza el trabajo restante para integrar una solicitud de incorporación de cambios en la aplicación GitHub Copilot. Cuando lo habilitas, la sesión de la aplicación lee la solicitud y resuelve lo que la bloquea: corrige comprobaciones de integración continua (CI) fallidas, responde a comentarios de revisión y reorganiza la base cuando es necesario. Después, combina la solicitud en cuanto GitHub lo permite. Se ejecuta en segundo plano, continúa tras reiniciar la aplicación y se desactiva cuando se combina la solicitud.

Hasta ahora has seleccionado **Merge pull request** personalmente. Agent Merge puede asumir esa responsabilidad, pero su capacidad de editar código y combinar sigue necesitando tu autorización explícita. Revisa las acciones permitidas y el trabajo antes de conceder permiso para combinar.

## Utiliza Agent Merge para gestionar la PR

Con todo el código creado y revisado, permitirás que Agent Merge gestione el proceso de la PR.

1. Usa el selector de agentes para seleccionar **Default agent**.
2. Selecciona el menú desplegable junto a **Create PR**.
3. Selecciona **Agent Merge**. El botón cambiará a **Agent Merge**.
4. Selecciona el botón **Agent Merge** para iniciar el proceso. Seleccionar la opción del menú desplegable no lo inicia.

Se iniciará el proceso de Agent Merge. Estas son algunas de las acciones que realizará:

- Crear la solicitud de incorporación de cambios con un título y una descripción.
- Si iniciaste la sesión desde una incidencia, incluir una referencia a ella en el cuerpo de la descripción.
- Reorganizar la base o gestionar posibles conflictos de combinación con la rama de destino.
- Supervisar el proceso de CI para garantizar que se superen todas las comprobaciones.
- Supervisar la PR para detectar comentarios de otros desarrolladores o de la revisión de código de Copilot y realizar los cambios necesarios para resolverlos.
- Opcionalmente, combinar automáticamente la PR cuando todo se haya completado correctamente.

Ahora pedirás a Agent Merge que combine también la PR cuando se superen todas las comprobaciones.

5. Revisa las diferencias de la PR y las pruebas de verificación antes de conceder permiso para combinar.
6. Selecciona el menú desplegable junto a **Agent Merge**.
7. Comprueba que **Merge pull request** está marcado.
8. Pide explícitamente al agente que combine la PR:

    ```plaintext
    Combina la PR cuando se superen todas las comprobaciones y aprobaciones obligatorias. No eludas las protecciones del repositorio.
    ```

9. Sigue el progreso y resuelve los bloqueos que se indiquen. Antes de continuar, confirma que el estado de la PR es **Merged**; tener activado el permiso de combinación o un agente en ejecución no confirma que se haya combinado.

> [!IMPORTANT]
> Agent Merge no elude las protecciones del repositorio ni los permisos que falten. Resuelve esos bloqueos antes de continuar.

## Resumen y pasos siguientes

Has automatizado varias partes del desarrollo: la generación del código, las pruebas y la validación, y ahora el proceso de la solicitud de incorporación de cambios. En concreto:

- has aprendido qué es Agent Merge y cómo automatiza el ciclo de combinación.
- has inspeccionado la PR completa de la funcionalidad y las pruebas de verificación.
- has autorizado Agent Merge después de revisar los cambios y has confirmado que la PR se ha combinado.

A continuación, [usarás un lienzo existente y crearás uno de triaje][next-lesson] para explorar una forma más completa de inspeccionar, planificar y visualizar el trabajo con el agente.

## Recursos

- [Gestionar incidencias y solicitudes de incorporación de cambios con la aplicación GitHub Copilot][managing-issues-prs]
- [Acerca de la aplicación GitHub Copilot][about-copilot-app]

[next-lesson]: ../9-canvases/
[managing-issues-prs]: https://docs.github.com/copilot/how-tos/github-copilot-app/managing-issues-and-pull-requests
[about-copilot-app]: https://docs.github.com/copilot/concepts/agents/github-copilot-app
