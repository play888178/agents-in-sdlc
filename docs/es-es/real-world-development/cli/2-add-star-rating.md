---
title: "Lección 2 - Añadir valoraciones con estrellas: una mejora rápida"
description: "Usa Copilot CLI para cambiar las tarjetas de juegos, revisa el resultado en el navegador y fusiona tu primera pull request."
authors:
  - geektrainer
lastUpdated: 2026-09-18
---

Ya has instalado Copilot CLI y probado una conversación. Ahora harás tu primer cambio en el proyecto. Será pequeño: los datos de los juegos ya incluyen una valoración con estrellas, pero las tarjetas de la página principal aún no la muestran. Pide al agente que la añada, revisa el cambio y fusiónalo en tu primera pull request.

En esta lección:

- iniciarás una conversación de Copilot centrada en una rama de funcionalidad.
- pedirás al agente un cambio pequeño.
- revisarás el cambio con `/diff`.
- ejecutarás la aplicación y comprobarás el cambio en un navegador reenviado.
- crearás y fusionarás tu primera pull request.

## Escenario

Cada juego de Tailspin Toys puede tener una valoración con estrellas, que ya aparece en su página de detalles. Las tarjetas de la página principal solo muestran el título, la categoría, la editorial y la descripción. Como ejercicio inicial, pedirás al agente que muestre en cada tarjeta la valoración existente: un cambio pequeño y autónomo, ideal para tu primera sesión.

## Anatomía de una conversación

Una **conversación** es el espacio donde trabajas con Copilot CLI en una tarea. A diferencia de la aplicación Copilot, una conversación normal de CLI usa el repositorio y la rama de Git que estén activos en el terminal, en vez de crear un árbol de trabajo dedicado. Las conversaciones guardadas te permiten retomar el diálogo; los archivos y la rama siguen siendo el estado normal de Git en el disco.

En una conversación verás tus prompts y las respuestas del agente, la actividad de sus herramientas mientras explora y edita archivos, y los cambios que puedes inspeccionar con `/diff`.

## Inicia una conversación y solicita el cambio

Empieza una conversación nueva para implementar la funcionalidad.

1. Vuelve a tu Codespace.
2. Si el terminal no está abierto, pulsa <kbd>Control</kbd>+<kbd>\`</kbd> (Mac) o <kbd>Ctrl</kbd>+<kbd>\`</kbd> (Windows/Linux).
3. Si Copilot no está en ejecución, inicia el agente con este comando:

   ```bash
   copilot --yolo
   ```

4. Inicia una sesión nueva con `/new` y pulsa <kbd>Enter</kbd>.
5. Envía este prompt:

   ```plaintext
   Show each game's starRating out of 5 in the game cards on the list page. If the rating is null, show "No rating yet". Keep the card layout as it is, add tests, and run the relevant checks.
   ```

Copilot explora el proyecto, localiza los archivos que muestran los detalles de los juegos y crea el código necesario.

## Revisa las diferencias

Revisa todos los cambios generados por IA antes de fusionarlos, aunque sean pequeños.

1. Introduce `/diff` e inspecciona todos los archivos modificados.
2. Confirma que cada tarjeta muestra la valoración numérica cuando existe y `No rating yet` cuando `starRating` es `null`.
3. Confirma que las pruebas cubren ambos estados.
4. Revisa los resultados de las comprobaciones que ejecutó Copilot y pídele que corrija los fallos.
5. Cuando termines, pulsa <kbd>Esc</kbd> para salir de la vista de diferencias.

> [!NOTE]
> Copilot, como cualquier herramienta de IA generativa, es probabilístico y no determinista; el código exacto puede variar. Revisa el comportamiento, no una implementación concreta.

## Comprueba los cambios

Pide a Copilot que inicie el sitio web para inspeccionar la interfaz de usuario (UI) actualizada en el navegador reenviado por Codespaces.

1. Pide a Copilot que inicie la aplicación:

   ```plaintext
   Start the app so I can inspect the star-rating change in my browser. Tell me the URL and leave the server running.
   ```

2. Cuando Codespaces indique que el puerto `4321` está disponible, selecciona **Open in Browser**.
3. Confirma que las tarjetas muestran las valoraciones sobre cinco.
4. Vuelve a Copilot y pídele que detenga el servidor que inició:

   ```plaintext
   Stop the development server you started.
   ```

## Abre y fusiona tu primera pull request

Ya has creado la funcionalidad. Ahora crearás una pull request (PR) para fusionar el código en el proyecto.

1. Pide al agente predeterminado que confirme el cambio:

   ```plaintext
   Commit the reviewed star-rating changes with an appropriate commit message.
   ```

2. Introduce `/pr create`. Copilot CLI envía el commit existente al crear la PR y muestra su URL.
3. Abre la PR manteniendo pulsada la tecla <kbd>Command</kbd> (Mac) o <kbd>Ctrl</kbd> (Windows/Linux) mientras seleccionas la URL que muestra Copilot CLI.
4. Revisa los archivos modificados y las comprobaciones.
5. Cuando esté todo listo, selecciona **Merge pull request** y confirma la fusión.
6. Vuelve a tu Codespace y sal de Copilot CLI con `/exit`.
7. Actualiza tu copia local de `main`:

   ```bash
   git checkout main
   git pull
   ```

## Resumen y siguientes pasos

Has publicado tu primer cambio con GitHub Copilot CLI. En concreto:

- iniciaste una conversación centrada en una rama de funcionalidad.
- pediste al agente que hiciera un cambio pequeño en las tarjetas.
- revisaste el cambio con `/diff`.
- ejecutaste la aplicación y comprobaste las valoraciones en un navegador reenviado.
- abriste y fusionaste tu primera pull request.

A continuación, partirás de la incidencia de filtrado y usarás los modos Plan y Autopilot para [crear una funcionalidad más amplia][next-lesson].

## Recursos

- [Acerca de GitHub Copilot CLI][about-copilot-cli]
- [Referencia de comandos de Copilot CLI][cli-reference]

[previous-lesson]: ../1-install-copilot-cli/
[next-lesson]: ../3-agent-modes/
[about-copilot-cli]: https://docs.github.com/copilot/concepts/agents/about-copilot-cli
[cli-reference]: https://docs.github.com/copilot/reference/copilot-cli-reference/cli-command-reference
