---
title: "Lección 3 - Modos del agente: Plan y Autopilot"
description: "Acuerda un enfoque con Plan, crea el filtrado con Autopilot y revisa el resultado en modo Interactive."
authors:
  - geektrainer
lastUpdated: 2026-09-18
---

Ya has añadido una pequeña funcionalidad al proyecto. Los cambios más amplios requieren un proceso más sólido. GitHub Copilot CLI puede adaptarse al flujo de trabajo de una organización para ayudarte a desarrollar lo adecuado. En esta lección y las siguientes seguirás un proceso habitual de desarrollo con agentes: partirás de una incidencia, crearás una funcionalidad, validarás el código y el comportamiento y, finalmente, fusionarás los cambios en el proyecto.

> [!NOTE]
> Continuarás el flujo de funcionalidad con la misma conversación y rama. Normalmente usarías ramas o PR diferentes para los distintos tipos de archivos; aquí simplificaremos el proceso para centrarnos en los conceptos principales.

En esta lección:

- iniciarás una conversación de Copilot desde una incidencia de GitHub.
- definirás los requisitos en el modo Plan.
- implementarás la funcionalidad en el modo Autopilot.
- revisarás el código.
- validarás manualmente la funcionalidad en un navegador reenviado.

Durante el trabajo, también actualizarás las instrucciones del repositorio, adaptarás la habilidad `quality-checks`, añadirás validación con MCP, crearás un agente de control de calidad y abrirás la PR.

## Escenario

El catálogo de Tailspin Toys crece y los visitantes necesitan filtrar los juegos por categoría y editorial. La incidencia del backlog describe la funcionalidad, pero antes de programar hay que acordar detalles, como la combinación de categorías. Usarás Plan para resolver esas decisiones y después autorizarás una implementación acotada con Autopilot.

## Contexto

La incorporación de agentes de programación con IA no cambia los fundamentos del desarrollo; los hace aún más importantes. Un flujo habitual consiste en:

1. Partir de una incidencia que describa el trabajo.
2. Planificar lo que se debe crear.
3. Desarrollar y revisar el código.
4. Ejecutar pruebas para validar el código.
5. Validar manualmente la funcionalidad.
6. Crear una pull request (PR).
7. Fusionar el código después de la revisión y de que se completen correctamente las comprobaciones de integración continua.

> [!NOTE]
> Los detalles varían según el equipo y la organización, pero suelen seguir este patrón.

Este proceso garantiza que el código generado por IA cumpla los requisitos y pase por la misma revisión que el código escrito manualmente.

## Modos de conversación

El **modo de conversación** determina la autonomía del agente. Pulsa <kbd>Shift</kbd>+<kbd>Tab</kbd> para alternar entre los modos:

- **Interactive**: trabajas con el agente. Propone cambios y espera tus indicaciones antes de continuar.
- **Plan**: el agente crea un plan y no puede editar archivos del proyecto.
- **Autopilot**: el agente trabaja de forma autónoma: escribe código, ejecuta pruebas e itera hasta completar la tarea.

Empieza en Plan, revisa el plan y, después, usa Autopilot para implementarlo.

## Parte de la incidencia

Antes de empezar a filtrar, prepara el repositorio y el terminal.

1. Vuelve a tu Codespace. Si está detenido, reinícialo.
2. Confirma que se ha fusionado la PR de las valoraciones.
3. Si el terminal no está abierto, pulsa <kbd>Control</kbd>+<kbd>\`</kbd> (Mac) o <kbd>Ctrl</kbd>+<kbd>\`</kbd> (Windows/Linux).
4. Actualiza `main` y crea una rama:

   ```bash
   git checkout main
   git pull --ff-only
   git checkout -b game-filters-cli
   ```

5. Inicia Copilot CLI:

   ```bash
   copilot --yolo
   ```

6. Pulsa <kbd>Tab</kbd> dos veces para abrir la pestaña **Issues**.
7. Pulsa <kbd>A</kbd> para mostrar todas las incidencias.
8. Usa las flechas para resaltar **Allow users to filter games by category and publisher**.
9. Pulsa <kbd>C</kbd> para añadir la incidencia al prompt y volver a la pestaña **Session**.

El prompt empieza con `#7` o un número similar. `#` permite añadir al contexto una incidencia o pull request de GitHub.

## Planifica el filtrado

La planificación permite acordar cómo implementar una funcionalidad antes de delegar el trabajo a Copilot. Para los cambios complejos, dedica tiempo a concretar el enfoque.

1. Pulsa <kbd>Shift</kbd>+<kbd>Tab</kbd> para seleccionar Plan. Confirma que el indicador bajo el prompt muestra **Plan**.
2. Después de la referencia a la incidencia, introduce:

   ```plaintext
   Create a plan for implementing this feature.
   ```

Copilot explorará el proyecto y determinará un enfoque.

3. Responde a las preguntas que pueda plantear sobre el filtrado, según tus preferencias.
4. Cuando termine el plan, pulsa <kbd>Control</kbd>+<kbd>E</kbd> (Mac) o <kbd>Ctrl</kbd>+<kbd>E</kbd> (Windows/Linux) para ampliarlo.
5. Desplázate por el plan y revísalo.
6. Pide a Copilot que revise cualquier parte que no coincida con tus decisiones.

## Aprueba Autopilot

Una vez revisado el plan, puedes implementarlo. Autopilot permite a Copilot iterar hasta considerar que ha completado la tarea.

1. Selecciona **Accept plan and build on autopilot (recommended)** o la opción equivalente de tu versión.
2. Confirma que el indicador muestra **Autopilot**.
3. Observa cómo Copilot sigue el plan, genera código y ejecuta pruebas.

> [!NOTE]
> Al aprobar el plan, la implementación puede comenzar de inmediato. Revísalo primero. Si Copilot informa de dependencias ausentes o de un conflicto de puertos, resuelve el problema de configuración antes de dar por completadas las comprobaciones.

## Revisa y verifica la implementación

Revisa el código generado antes de fusionarlo y ejecuta el sitio para comprobar el resultado.

1. Pulsa <kbd>Shift</kbd>+<kbd>Tab</kbd> para cambiar a Interactive. Confirma que el indicador ya no muestra **Plan** ni **Autopilot**.
2. Introduce `/diff` e inspecciona la implementación del filtrado y las pruebas.
3. Compara el resultado con la incidencia y las decisiones tomadas durante la planificación.
4. Cuando termines la revisión, pulsa <kbd>Esc</kbd> para salir de la vista de diferencias.
5. Revisa el resultado de las comprobaciones del proyecto y pide a Copilot que corrija los fallos.

## Explora la nueva funcionalidad

Inicia la aplicación y abre el sitio desde el puerto reenviado por Codespaces.

1. Pide a Copilot que inicie la aplicación:

   ```plaintext
   Start the app so I can try the filtering feature in my browser. Tell me the URL and leave the server running.
   ```

2. Cuando Codespaces indique que el puerto `4321` está disponible, selecciona **Open in Browser**.
3. Prueba el filtrado por categoría, por editorial y las combinaciones que acordaste en el plan.
4. Comprueba que el restablecimiento y los resultados vacíos coinciden con la incidencia y tus decisiones.
5. Vuelve al Codespace y pide a Copilot que detenga el servidor de desarrollo que inició.

## Resumen y siguientes pasos

Has usado distintos modos de conversación para crear y revisar una funcionalidad. En esta lección:

- iniciaste una conversación de Copilot desde una incidencia de GitHub.
- definiste los requisitos en el modo Plan.
- implementaste la funcionalidad en el modo Autopilot.
- revisaste el código.
- validaste manualmente la funcionalidad en un navegador reenviado.

A continuación, comprobarás cómo generar código que siga las prácticas documentadas mediante [instrucciones personalizadas][next-lesson].

## Recursos

- [Autopilot en GitHub Copilot CLI][autopilot]
- [Referencia de comandos de Copilot CLI][cli-reference]

[autopilot]: https://docs.github.com/copilot/concepts/agents/copilot-cli/autopilot
[cli-reference]: https://docs.github.com/copilot/reference/copilot-cli-reference/cli-command-reference
[previous-lesson]: ../2-add-star-rating/
[next-lesson]: ../4-custom-instructions/
