---
title: "Ejercicio 5: Supervisar y gestionar agentes"
authors:
  - geektrainer
lastUpdated: 2026-06-30
---

| [← Lección anterior: Agentes personalizados][previous-lesson] |
|:--|

Cuando usas GitHub Copilot en modo agente, trabaja de forma autónoma: explora la base de código, propone cambios, edita archivos y ejecuta comandos. Como el trabajo se realiza en tu equipo y en tiempo real, Copilot Chat en Visual Studio Code (VS Code) muestra las llamadas a herramientas y las ediciones de archivos a medida que ocurren. Puedes revisar las diferencias en línea, aceptar o rechazar cambios concretos y orientar la conversación con prompts de seguimiento para ajustar o ampliar el trabajo sin salir del editor.

En este ejercicio:

- supervisarás en tiempo real el trabajo del agente de accesibilidad.
- revisarás y aceptarás las ediciones de archivos propuestas.
- orientarás al agente durante la sesión para que añada un modo claro junto al alto contraste.

## Escenario

El agente personalizado de accesibilidad del [ejercicio 4][previous-lesson] está implementando el modo de alto contraste. Mientras trabaja, el equipo de diseño ha pedido también un modo claro para quienes consideran demasiado oscuro el tema predeterminado. En vez de empezar una conversación nueva, orientarás la sesión existente para ampliar el trabajo.

## Supervisa el progreso de Copilot

La vista Copilot Chat muestra cada paso del agente: los archivos que explora, los comandos que ejecuta y las ediciones que propone. Cada cambio de archivo aparece en el chat con las diferencias y los controles **Keep** y **Undo**.

1. Vuelve a la vista Copilot Chat del ejercicio 4.
2. Mientras trabaja el agente, desplázate por el chat para ver las llamadas a herramientas, la lectura y edición de archivos y los comandos de terminal que solicite ejecutar.
3. Cuando el agente proponga editar un archivo, verás las diferencias en línea:
   - Selecciona **Keep** para aceptar el cambio.
   - Selecciona **Undo** para revertir el cambio y permitir que el agente vuelva a intentarlo.
4. También puedes abrir el indicador **Files changed** de la parte superior del chat para ver todos los archivos modificados durante la sesión.

> [!NOTE]
> Si una llamada a una herramienta requiere permiso (por ejemplo, para ejecutar un comando del terminal), el agente se detendrá y mostrará un aviso **Allow**. Aprueba solo los comandos que consideres seguros.

5. Espera a que el agente termine de implementar el modo de alto contraste. Debería mostrar un mensaje final que resuma los cambios realizados.

## Orienta al agente para añadir el modo claro

Ahora que ya está disponible el modo de alto contraste, ampliarás la misma conversación para añadir un control de modo claro. Como seguirás en la misma sesión de chat, el agente conservará el contexto de su trabajo anterior, incluidos los componentes que ha modificado, el enfoque de estilo y el uso del almacenamiento local.

1. Envía este prompt de seguimiento en el campo de entrada de Copilot Chat:

   ```plaintext
   Buen trabajo. Ahora añade un control de modo claro junto al control de alto contraste. El modo claro debe seguir el mismo enfoque: conservarse mediante el almacenamiento local y estar disponible en la misma zona de la interfaz. El usuario debe poder activar el alto contraste y el modo claro de forma independiente.
   ```

2. El agente retomará el trabajo, explorará los cambios anteriores y propondrá lo necesario para añadir el modo claro.
3. Revisa cada cambio nuevo a medida que aparezca, como hiciste con el modo de alto contraste. Usa **Keep** o **Undo** para cada diferencia.
4. Si el agente sigue un enfoque que no quieres —por ejemplo, intenta reorganizar la interfaz de los controles de una forma que no te gusta—, deténlo con el botón **Stop** situado junto al campo de entrada del chat y envía un mensaje para aclarar lo que necesitas.

> [!TIP]
> Orientar una sesión en curso es más rápido que empezar de nuevo. Envía prompts de seguimiento para pedir ajustes («Simplifica el CSS: usa variables CSS en lugar de duplicar selectores»), corregir el enfoque («No añadas un tercer control; integra el modo claro en el panel de configuración existente») o ampliar el alcance («Añade también métodos abreviados de teclado para los dos controles»).

## Revisa el conjunto de cambios

Antes de confirmar el trabajo, revisa todos los cambios realizados por el agente.

1. Abre la vista **Source Control** de VS Code.
2. Revisa la lista completa de archivos modificados. Deberías ver componentes Astro, estilos y las pruebas relacionadas.
3. Abre algunos de los archivos modificados y revisa las diferencias. Confirma que se aplican los patrones de accesibilidad del agente personalizado: atributos de Aplicaciones de Internet enriquecidas accesibles (ARIA), navegación con teclado, lenguaje de marcado de hipertexto (HTML) semántico y persistencia mediante el almacenamiento local.
4. Cuando estés conforme, prepara y confirma los cambios desde el panel Source Control. Los publicarás en [la siguiente lección][next-lesson].

## Resumen y pasos siguientes

Has usado el modo agente de Copilot Chat para supervisar y orientar una sesión en curso: has revisado cada edición, aceptado o rechazado cambios y enviado prompts de seguimiento para ampliar el trabajo sin perder el contexto.

En esta lección has explorado:

- cómo supervisar el trabajo de un agente en tiempo real.
- cómo aceptar o rechazar ediciones individuales.
- cómo orientar una sesión en curso con prompts de seguimiento.

En la [siguiente lección][next-lesson], publicarás el trabajo de accesibilidad en una solicitud de incorporación de cambios y revisarás los cambios del agente personalizado local realizados durante el laboratorio.

## Recursos

- [Copilot Chat en VS Code][copilot-chat-vscode]
- [Uso del modo agente][agent-mode]

---

| [← Lección anterior: Agentes personalizados][previous-lesson] | [Siguiente lección: Iterar sobre el trabajo de Copilot →][next-lesson] |
|:--|--:|

[previous-lesson]: ../4-custom-agents/
[next-lesson]: ../6-iterating/
[copilot-chat-vscode]: https://code.visualstudio.com/docs/copilot/chat/copilot-chat
[agent-mode]: https://code.visualstudio.com/docs/copilot/chat/chat-agent-mode
