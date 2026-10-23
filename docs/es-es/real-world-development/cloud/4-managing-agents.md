---
title: "Ejercicio 4: Supervisar y gestionar agentes"
authors:
  - geektrainer
lastUpdated: 2026-06-30
---

En los últimos ejercicios has asignado al agente en la nube de Copilot tres tareas distintas para mejorar la experiencia de usuario y añadir funciones. Aunque está diseñado para trabajar de forma autónoma y asíncrona, es importante poder supervisar esas tareas.

Hay varias herramientas para gestionar las tareas que asignas al agente en la nube, como la [página de agentes][agents-page] de GitHub.com. Este panel de control muestra todas las tareas de los agentes que tienen solicitudes de incorporación de cambios (PR) abiertas. Puedes consultar las operaciones realizadas y orientar una sesión en curso.

En esta lección:

- explorarás la página de agentes para supervisar las tareas del agente en la nube.
- orientarás una sesión en curso para solicitar funcionalidad adicional.

## Escenario

Después de asignar al agente la implementación de un modo de alto contraste, el equipo decidió que también sería útil añadir un modo claro. Como ya se estaban actualizando los estilos del sitio y añadiendo controles, tenía sentido incluir esta función. Orientarás el trabajo del agente para que añada el modo claro además del alto contraste.

## Revisa las tareas del agente en la nube de Copilot

Consulta el estado actual de todas las tareas asignadas al agente en la nube de Copilot.

1. Ve a la [página de agentes de Copilot](https://github.com/copilot/agents).
2. Fíjate en la lista de tareas del panel principal y del panel izquierdo. Deberías ver las que has asignado a Copilot, entre ellas:
   - actualizar la documentación de la base de código.
   - generar interfaces de programación de aplicaciones (API) para modificar juegos.
   - añadir un modo de alto contraste al sitio web.
3. Selecciona una de las tareas en curso y revisa las operaciones que ha realizado Copilot. Por ejemplo:
   - obtener el código del repositorio.
   - crear el entorno de trabajo de Copilot.
   - configurar servidores del Model Context Protocol (MCP).
   - realizar varios pasos para completar la tarea asignada.

> [!NOTE]
> Los pasos que aparezcan dependerán del estado del trabajo de Copilot y del enfoque que haya utilizado.

4. Observa también el panel de solicitudes de incorporación de cambios de la derecha. Permite ver la PR y los archivos modificados para supervisar el trabajo.

## Orienta al agente en la nube

Ahora que has revisado las tareas activas, pedirás a Copilot que incluya un control para el modo claro mientras trabaja en el alto contraste.

1. Selecciona la sesión sobre la implementación del modo de alto contraste. El título exacto variará según el nombre que haya elegido Copilot y el estado del trabajo.

   ![Sesión de accesibilidad en Mission Control](../../../_images/ex6-accessibility-session.png)

2. Espera unos minutos hasta que la sesión indique que ha terminado la configuración y ha empezado a trabajar. Lo notarás cuando aparezcan mensajes parecidos a los siguientes.
3. En el cuadro de diálogo **Steer active session while Copilot is working**, escribe este prompt:

   ```plaintext
   Mientras implementamos el modo de alto contraste, añade también un modo claro. Debe haber un control para este modo que permita a los usuarios elegir el modo de visualización que prefieran.
   ```

   ![Captura de la tarea del agente en la nube en la página de agentes, con el cuadro de diálogo de orientación de la sesión activa resaltado](../../../_images/ex6-steer-cloud-agent-task.png)

4. Pulsa <kbd>Enter</kbd> para enviar el prompt.
5. Observa cómo Copilot confirma la solicitud y la incorpora al flujo de trabajo.

## Deja que Copilot trabaje

Copilot empezará a trabajar en la tarea actualizada. Incorporará la nueva solicitud cuando termine el paso que estaba realizando en el momento de recibir tu mensaje.

El trabajo puede tardar varios minutos, así que puedes aprovechar este tiempo para repasar lo que has aprendido y explorado.

## Resumen y pasos siguientes

En esta lección has explorado la página de agentes de Copilot, el centro de control para supervisar y orientar las tareas del agente en la nube de GitHub Copilot. Puedes hacer seguimiento de las tareas activas y completadas, revisar el trabajo y redirigir las tareas en curso para ajustar el alcance o proporcionar más directrices.

Has explorado estos conceptos:

- has explorado Mission Control y la página de agentes para supervisar las tareas del agente en la nube.
- has redirigido una sesión en curso para solicitar funcionalidad adicional.

Mientras Copilot termina el trabajo en las funciones de accesibilidad, puedes [iterar sobre las solicitudes de incorporación de cambios que ha creado][next-lesson].

## Recursos

- [Página de agentes de Copilot][agents-page]
- [Acerca de los agentes personalizados][custom-agents]

---

| [← Lección anterior: Agentes personalizados][previous-lesson] | [Siguiente lección: Iterar sobre el trabajo de Copilot →][next-lesson] |
|:--|--:|

[previous-lesson]: ../3-custom-agents/
[next-lesson]: ../5-iterating/
[agents-page]: https://github.blog/changelog/2025-07-03-agents-page-for-copilot-coding-agent-in-public-preview
[custom-agents]: https://docs.github.com/copilot/concepts/agents/cloud-agent/about-custom-agents
