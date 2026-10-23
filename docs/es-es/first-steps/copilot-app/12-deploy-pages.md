---
title: "Lección 12: Publicar el sitio con GitHub Pages"
description: "Usa la aplicación GitHub Copilot para configurar un flujo de trabajo de Pages, revisar su solicitud de incorporación de cambios y comprobar el Space Quiz publicado."
authors:
  - jamesmontemagno
lastUpdated: 2026-10-12
---

Publica el Space Quiz en GitHub Pages para que tenga una dirección pública. Copilot puede preparar el flujo de trabajo y guiar la publicación, pero tú eres responsable de revisar los cambios y confirmar que el sitio está disponible.

En esta lección:

- pedirás a Copilot que configure la publicación en GitHub Pages.
- revisarás el flujo de trabajo y la solicitud de incorporación de cambios antes de combinarla.
- seguirás la publicación y comprobarás el sitio publicado.

## Escenario

El Space Quiz ya está en un repositorio de GitHub, pero hace falta una forma sencilla de probarlo. Publicar el sitio de un solo archivo con GitHub Pages permite acceder a él desde una dirección pública sin añadir un servidor ni dependencias de ejecución.

## Pide a Copilot que publique el sitio

Para configurar la publicación y pedir al agente que no afirme que ha terminado hasta comprobar el sitio, envía este prompt en la sesión de la aplicación Copilot para `space-quiz`:

1. Envía este prompt:

   ```plaintext
   Habilita GitHub Pages para este repositorio. Crea un flujo de trabajo de GitHub Actions que compile el sitio web si hace falta y lo publique en GitHub Pages. Añade un README que describa la aplicación y explique cómo ejecutarla localmente, si todavía no existe. Crea una solicitud de incorporación de cambios con estos cambios y combínala cuando se superen las comprobaciones obligatorias. Supervisa la publicación hasta que termine correctamente y, después, indica la dirección web final del sitio activo. Si no tienes permiso para habilitar Pages o combinar la solicitud, dime exactamente qué tengo que aprobar o hacer; no afirmes que se ha publicado correctamente hasta que el sitio esté activo.
   ```

   ![Diagrama de tres etapas: una solicitud de incorporación de cambios de space-quiz combinada; una ejecución correcta de Deploy website en GitHub Actions con los pasos Build website, Upload artifact y Deploy to Pages; y el Space Quiz activo en yourname.github.io/space-quiz/.](../../../_images/first-steps-app-pages-deploy.svg)

   > [!WARNING]
   > Este prompt pide al agente que cree y combine una solicitud de incorporación de cambios y cambie la configuración de Pages del repositorio. Revisa las diferencias, los permisos de publicación y cualquier solicitud de aprobación de GitHub antes de continuar.

## Revisa el flujo de trabajo y comprueba la publicación

1. Confirma que el README solo se añade si el repositorio aún no tiene uno.
2. Revisa los cambios de la solicitud de incorporación de cambios, incluidos los pasos de compilación y publicación del flujo de trabajo.
3. Combina los cambios solo cuando se superen las comprobaciones obligatorias de la solicitud de incorporación de cambios.
4. Sigue la ejecución de publicación de GitHub Actions hasta que termine correctamente.
5. Abre la dirección URL indicada y confirma que se carga el Space Quiz.
6. Si el agente no puede habilitar Pages o combinar la solicitud de incorporación de cambios, sigue las instrucciones específicas que te proporcione. No des la tarea por terminada hasta que la publicación se complete y el sitio activo se cargue.

## Resumen y pasos siguientes

Has pedido a Copilot que configure la publicación, has revisado el flujo de trabajo y la solicitud de incorporación de cambios y has comprobado el sitio publicado. Continúa con la [lección 13: Repaso y pasos siguientes][next-lesson].

[next-lesson]: ../13-review/
