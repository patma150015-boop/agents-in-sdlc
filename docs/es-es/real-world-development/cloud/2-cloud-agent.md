---
title: "Ejercicio 2: Agente en la nube de GitHub Copilot"
authors:
  - geektrainer
lastUpdated: 2026-06-30
---

| [← Lección anterior: Instrucciones personalizadas][previous-lesson] |
|:--|

Es probable que pocas organizaciones estén libres de deuda técnica. Puede tratarse de problemas de seguridad sin resolver, código heredado que necesita actualizarse o solicitudes de funcionalidades que llevan tiempo en el backlog porque no había tiempo para implementarlas. El agente en la nube de GitHub Copilot puede actualizar código y añadir funcionalidades de forma autónoma. Cuando termina el trabajo, genera una solicitud de incorporación de cambios en borrador para que la revise un desarrollador. Así puedes delegar tareas tediosas, acelerar el desarrollo y centrarte en cuestiones de mayor alcance.

En este ejercicio explorarás cómo usar el agente en la nube de Copilot para:

- personalizar el entorno de generación de código.
- garantizar que las operaciones se realizan de forma segura.
- comprender la importancia de definir incidencias con un alcance claro.
- asignar incidencias a Copilot.

## Escenarios

Tailspin Toys tiene deuda técnica pendiente. Los contratistas que crearon la primera versión del sitio web dejaron la documentación en mal estado; de hecho, verás que no hay ninguna. Como primer paso, el equipo quiere añadir comentarios TSDoc a todas las funciones exportadas de la aplicación.

Además, el equipo de diseño quiere facilitar que se descubran juegos. Le gustaría que la página de detalles de cada juego mostrara otros títulos de la misma categoría para que los visitantes puedan seguir explorando. Por ahora no necesita una implementación pulida: quiere algo con lo que probar la experiencia de usuario (UX). Aunque este trabajo está bloqueado, hay otras incidencias más prioritarias.

Estas tareas se suelen aplazar y son adecuadas para el agente en la nube de Copilot. Puede trabajar en ellas de forma asíncrona mientras el desarrollador se ocupa de otras tareas y, después, revisar el trabajo realizado y comprobar que cumple las expectativas.

## Presentación del agente en la nube de GitHub Copilot

El [agente en la nube de GitHub Copilot](https://docs.github.com/copilot/concepts/agents/cloud-agent/about-cloud-agent#overview-of-copilot-cloud-agent-formerly-copilot-coding-agent) puede trabajar en segundo plano de forma parecida a un desarrollador. Por ejemplo, puedes [asignar una incidencia de GitHub a Copilot](https://docs.github.com/copilot/how-tos/use-copilot-agents/cloud-agent/start-copilot-sessions). Después de recibir la asignación, Copilot crea una solicitud de incorporación de cambios en borrador para registrar el progreso, prepara un entorno y empieza a trabajar. Puedes consultar su sesión mientras trabaja o cuando termina. Cuando la solución propuesta esté lista para que la revises, te mencionará en la solicitud de incorporación de cambios.

## La importancia de instrucciones con un alcance claro

GitHub Copilot no hace magia: no puedes dejar de pensar en la tarea. Incluso las operaciones que parecen sencillas pueden volverse complejas al examinar sus detalles. Por eso, [define con cuidado las tareas para el agente en la nube de Copilot][cloud-agent-best-practices]. Trátalo como a un compañero de programación con inteligencia artificial (IA): trabaja por etapas, aprende, experimenta y adapta el proceso. Los principios fundamentales del desarrollo de software no cambian al incorporar IA generativa.

## Instrucciones personalizadas de este repositorio

En ejercicios anteriores has visto cómo las instrucciones personalizadas orientan a Copilot. Si ya has completado un ejercicio práctico sobre ellas, conoces `.github/copilot-instructions.md`; si no, aprovecha para echar un vistazo. Antes de asignar trabajo al agente en la nube de Copilot, revisa los archivos de instrucciones existentes en este repositorio, sin modificarlos, para poder reconocer su efecto más adelante.

Abre estos archivos en la interfaz web de GitHub o en un codespace que ya esté en ejecución:

- `.github/copilot-instructions.md`: revisa la sección **Code standards**, en especial las convenciones de TypeScript y los comentarios de documentación TSDoc.
- `.github/instructions/unit-tests.instructions.md`: fíjate en el frontmatter `applyTo`, que aplica estas instrucciones a los archivos `**/*.test.ts`.

Cuando asignes la incidencia `Code lacks documentation` al agente en la nube en la siguiente sección, comprueba si la solicitud de incorporación de cambios resultante incluye comentarios TSDoc, convenciones de TypeScript y encabezados de comentarios. Estos elementos se basan en los archivos de instrucciones que acabas de revisar. Volverás a comprobarlo al revisar las PR en un ejercicio posterior.

## Configura el entorno de desarrollo del agente en la nube de Copilot

Normalmente, crear código requiere preparar un entorno concreto y ejecutar scripts de configuración para dejarlo listo. Lo mismo ocurre al asignar tareas a Copilot, que trabaja de forma parecida a un ingeniero de software (SWE).

El agente en la nube utiliza [GitHub Actions][github-actions] para preparar el entorno de trabajo. Puedes personalizarlo con un [flujo de trabajo de configuración especial][setup-workflow], definido en `.github/workflows/copilot-setup-steps.yml`, que se ejecuta antes de que empiece el trabajo. Así el agente tendrá acceso a las herramientas de desarrollo y dependencias necesarias. El flujo de trabajo ya está configurado para este laboratorio e instala Node.js y las dependencias del proyecto, descarga los navegadores para las pruebas de extremo a extremo y configura la base de datos SQLite migrada y precargada de la aplicación Astro:

```yaml
name: "Copilot Setup Steps"

# Permite probar los pasos de configuración desde la pestaña "Actions" del repositorio.
on: workflow_dispatch

env:
  ASTRO_TELEMETRY_DISABLED: "1"

jobs:
  copilot-setup-steps:
    runs-on: ubuntu-latest
    # Establece los permisos mínimos necesarios para estos pasos. Copilot recibirá su propio token para las operaciones.
    permissions:
      # Si los pasos de configuración clonan el repositorio, por ejemplo, para instalar dependencias, necesitan el permiso `contents: read`. Si no lo clonan, Copilot lo hará automáticamente cuando terminen los pasos.
      contents: read
    steps:
      - name: Checkout code
        uses: actions/checkout@v5

      # Configuración del frontend y la aplicación: Node.js (ahora toda la aplicación es Astro + Drizzle/libSQL).
      - name: Set up Node.js
        uses: actions/setup-node@v6
        with:
          node-version: "lts/*"
          cache: "npm"
          cache-dependency-path: "./package-lock.json"

      - name: Install JavaScript dependencies
        run: npm ci

      - name: Install Playwright browsers
        run: npx playwright install --with-deps chromium

      # Migra y precarga la base de datos SQLite local para que las compilaciones y pruebas tengan datos.
      - name: Set up the database
        run: npm run db:setup
```

Se parece a cualquier otro archivo de flujo de trabajo de GitHub, pero tiene algunos detalles importantes:

- Incluye un único trabajo llamado `copilot-setup-steps`. GitHub Actions lo ejecuta antes de que Copilot empiece a trabajar en la solicitud de incorporación de cambios.
- El desencadenador `workflow_dispatch` permite ejecutar el flujo de trabajo manualmente desde la pestaña **Actions** del repositorio. Es útil para comprobar que se ejecuta correctamente antes de esperar a que Copilot lo inicie.

## Añade documentación

Todo el mundo reconoce la importancia de la documentación, pero muchos proyectos tienen información obsoleta o carecen de ella. Esta deuda técnica reduce la productividad y dificulta el mantenimiento del código o la incorporación de nuevos desarrolladores. Copilot es capaz de generar documentación, así que esta es una tarea adecuada para el agente en la nube. Trabajará en segundo plano y, en un ejercicio posterior, revisarás los resultados.

1. Abre una pestaña nueva del navegador y ve al repositorio en github.com.
2. Selecciona la pestaña **Issues**.
3. Selecciona **New issue** para abrir el cuadro de diálogo de la incidencia.
4. Selecciona **Blank issue** para crear una incidencia vacía.
5. En el campo **Title**, escribe `Code lacks documentation`.
6. En **Description**, escribe:

    ```plaintext
    Nuestra organización exige que las funciones y los métodos incluyan comentarios de documentación TSDoc cuando resulten útiles. Sin embargo, las actualizaciones recientes no han seguido este estándar. Tenemos que actualizar el código existente para incluir comentarios de documentación cuando ayuden a aclarar el comportamiento.
    ```

7. Selecciona **Create** para crear la incidencia.
8. En el lado derecho, selecciona **Assign to Copilot** para abrir el cuadro de diálogo de asignación.

   ![Asignación de Copilot a una incidencia](../../../_images/shared-assign-copilot.png)

9. Selecciona **Assign**.
10. Selecciona la pestaña **Pull Requests**.
11. Abre la nueva solicitud de incorporación de cambios (PR), cuyo título será parecido a `[WIP]: Code lacks documentation`. Si no aparece en la lista, espera un minuto o dos y actualiza la página.
12. Al cabo de unos minutos, Copilot debería crear una lista de tareas.

> [!NOTE]
> La lista puede tardar varios minutos en aparecer en la PR. Copilot está preparando el entorno (ejecutando el flujo de trabajo anterior), analizando el proyecto y determinando el mejor enfoque.

13. Revisa la lista de tareas y las acciones que va a realizar.
14. Desplázate por la cronología de la PR para ver cuándo Copilot empieza a trabajar en la incidencia.
15. Selecciona el botón **View session**.

   ![Vista de la sesión de Copilot](../../../_images/ex4-view-session.png)

> [!CAUTION]
> Es posible que tengas que actualizar la página para ver el indicador actualizado.

16. Observa la sesión en directo para ver cómo Copilot resuelve el problema: explora el código y su estado, se detiene para pensar y definir un plan, y crea el código.

El proceso puede tardar varios minutos. Una de las principales ventajas del agente en la nube de Copilot es que puede trabajar de forma asíncrona mientras te ocupas de otras tareas. Aprovecharás esa función asignándole otra tarea y, después, dedicarás tu atención a escribir código para añadir funciones a la aplicación.

## Añade una sección de juegos relacionados a la página de detalles

Una de las ventajas del agente en la nube de GitHub Copilot es que permite dividir el trabajo: tú puedes ocuparte de una tarea mientras el agente realiza otra. Aunque añadir una sección de juegos relacionados para el equipo de diseño no lleve mucho tiempo, podrías dedicar ese tiempo a otra tarea. Se la asignarás al agente en la nube de Copilot.

1. Vuelve al repositorio en github.com.
2. Selecciona la pestaña **Issues**.
3. Selecciona **New issue** para abrir el cuadro de diálogo de la incidencia.
4. Selecciona **Blank issue** para usar la plantilla vacía.
5. En **Title**, escribe `Show related games on the game details page`.
6. En **Description**, escribe:

    ```markdown
    Queremos ayudar a los usuarios a descubrir más juegos mostrando juegos relacionados en la página de detalles de cada juego. El equipo de diseño quiere explorar la experiencia de usuario y realizar pruebas de aceptación. Estos son los requisitos:

   - Añadir en `src/lib/` un asistente de acceso a datos que devuelva otros juegos de la misma categoría que un juego dado, sin incluir ese juego
   - Mostrar una sección "Juegos relacionados" en la página de detalles del juego que use el asistente
   - Gestionar el caso en que un juego no tenga juegos relacionados
   - Crear pruebas unitarias para el nuevo asistente
   - Asegurarse de que todas las pruebas se superan antes de crear la PR
    ```

7. Selecciona **Create** para crear la incidencia.
8. En el lado derecho, selecciona **Assign to Copilot** para abrir el cuadro de diálogo de asignación.

   ![Asignación de Copilot a una incidencia](../../../_images/shared-assign-copilot.png)

9. Selecciona **Assign**.

Poco después, aparecerá un conjunto de emojis 👀 en el primer comentario de la incidencia, que indicará que Copilot está trabajando en ella.

![Copilot usa el emoji de ojos para indicar que está trabajando en la incidencia](../../../_images/ex4-issue-eyes-emoji.png)

Copilot ya está trabajando en tu segunda solicitud. El agente en la nube funciona de forma parecida a un ingeniero de software, así que no hace falta supervisarlo constantemente; podrás revisar los resultados cuando termine. Ahora puedes centrarte en crear y utilizar agentes personalizados.

## Resumen y pasos siguientes

En esta lección has explorado el [agente en la nube de GitHub Copilot][copilot-agents]. Puedes asignarle incidencias para que las resuelva de forma asíncrona. Copilot puede abordar deuda técnica, crear funcionalidades o ayudar a migrar código entre frameworks.

Has explorado estos conceptos:

- personalizar el entorno de generación de código.
- garantizar que las operaciones se realizan de forma segura.
- comprender la importancia de definir incidencias con un alcance claro.
- asignar incidencias a Copilot.

Mientras el agente en la nube trabaja en segundo plano, puedes crear y usar agentes personalizados. El [agente en la nube de Copilot también puede usar servidores del protocolo Model Context Protocol (MCP)][cloud-agent-mcp] y las instrucciones personalizadas que has explorado en módulos anteriores.

## Recursos

- [Acerca del agente en la nube de Copilot][copilot-agents]
- [Asignar incidencias de GitHub a Copilot][assign-issue]
- [Prácticas recomendadas para el flujo de configuración del agente en la nube de Copilot][cloud-agent-best-practices]

| [Siguiente lección: Agentes personalizados →][next-lesson] |
|--:|

[previous-lesson]: ../1-custom-instructions/
[next-lesson]: ../3-custom-agents/
[cloud-agent-mcp]: https://docs.github.com/copilot/how-tos/copilot-on-github/customize-copilot/customize-cloud-agent/extend-cloud-agent-with-mcp
[assign-issue]: https://docs.github.com/copilot/how-tos/use-copilot-agents/cloud-agent/start-copilot-sessions
[setup-workflow]: https://docs.github.com/copilot/how-tos/copilot-on-github/customize-copilot/customize-cloud-agent/customize-the-agent-environment
[copilot-agents]: https://docs.github.com/copilot/concepts/agents/cloud-agent/about-cloud-agent
[cloud-agent-best-practices]: https://docs.github.com/copilot/how-tos/copilot-on-github/customize-copilot/customize-cloud-agent/customize-the-agent-environment
[github-actions]: https://docs.github.com/actions
