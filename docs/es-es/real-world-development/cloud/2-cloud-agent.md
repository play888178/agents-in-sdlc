---
title: "Ejercicio 2 - Agente de GitHub Copilot en la nube"
authors:
  - geektrainer
lastUpdated: 2026-06-30
---

| [← Lección anterior: Instrucciones personalizadas][previous-lesson] |
|:--|

Muchas organizaciones tienen deuda técnica: problemas de seguridad sin resolver, código heredado que requiere actualizaciones o funcionalidades que llevan tiempo en el backlog. El agente de Copilot en la nube puede actualizar código y añadir funcionalidad de forma autónoma. Al terminar, crea una pull request (PR) en borrador para que la revise un desarrollador. Así puedes delegar tareas tediosas y centrarte en trabajos de mayor alcance.

En esta lección explorarás cómo:

- personalizar el entorno de generación de código.
- garantizar que las operaciones se realicen de forma segura.
- redactar incidencias con un ámbito claro.
- asignar incidencias a Copilot.

## Escenarios

Tailspin Toys quiere resolver dos tareas. Primero, añadir comentarios TSDoc a todas las funciones exportadas de la aplicación, ya que la documentación actual es insuficiente. Además, el equipo de diseño quiere mostrar juegos relacionados —otros títulos de la misma categoría— en la página de detalles de cada juego para probar la experiencia de usuario (UX). Son tareas que pueden quedar relegadas, por lo que resultan adecuadas para delegarlas de forma asíncrona y revisar el resultado más tarde.

## Presentación del agente de Copilot en la nube

El [agente de GitHub Copilot en la nube](https://docs.github.com/copilot/concepts/agents/cloud-agent/about-cloud-agent#overview-of-copilot-cloud-agent-formerly-copilot-coding-agent) realiza tareas en segundo plano, de forma parecida a un desarrollador. Por ejemplo, puedes [asignarle una incidencia de GitHub](https://docs.github.com/copilot/how-tos/use-copilot-agents/cloud-agent/start-copilot-sessions). El agente crea una PR en borrador para seguir el trabajo, prepara un entorno y comienza la tarea. Puedes consultar la sesión mientras trabaja o después; cuando la solución esté lista para revisar, te mencionará en la PR.

## La importancia de definir bien el alcance

GitHub Copilot no es magia: sigue siendo necesario pensar. Incluso las tareas aparentemente sencillas pueden ser complejas. Ten cuidado al definir el alcance de las tareas para el agente en la nube y [consulta sus buenas prácticas][cloud-agent-best-practices]. Trátalo como un programador en pareja con IA: trabaja por etapas, aprende, experimenta y adapta el enfoque. La IA generativa no cambia los fundamentos del desarrollo de software.

## Instrucciones personalizadas del repositorio

Antes de asignar trabajo, revisa en modo de solo lectura las instrucciones del repositorio para observar su efecto posteriormente.

1. En la interfaz web de GitHub o en un codespace, abre `.github/copilot-instructions.md` y revisa **Code standards**, sobre todo las convenciones de TypeScript y los comentarios TSDoc.
2. Abre `.github/instructions/unit-tests.instructions.md` y observa que el frontmatter `applyTo` aplica las instrucciones a los archivos `**/*.test.ts`.

Al asignar la incidencia `Code lacks documentation`, fíjate en los comentarios TSDoc, las convenciones TypeScript y los encabezados de archivo de la PR. Estas pautas proceden de los archivos de instrucciones.

## Configura el entorno de desarrollo del agente en la nube

El agente usa [GitHub Actions][github-actions] para preparar su entorno. Puedes personalizarlo con un flujo de trabajo especial configurado en `.github/workflows/copilot-setup-steps.yml`. El proyecto ya incluye uno para instalar Node.js y las dependencias, los navegadores de las pruebas end-to-end y la base SQLite migrada y precargada de la aplicación Astro:

```yaml
name: "Copilot Setup Steps"

# Permite probar los pasos de configuración desde la pestaña "Actions" del repositorio
on: workflow_dispatch

env:
  ASTRO_TELEMETRY_DISABLED: "1"

jobs:
  copilot-setup-steps:
    runs-on: ubuntu-latest
    # Establece los permisos mínimos necesarios para *tus pasos*. Copilot recibirá su propio token para sus operaciones.
    permissions:
      # Si quieres clonar el repositorio como parte de la configuración, por ejemplo para instalar dependencias, necesitarás el permiso `contents: read`. Si no lo clonas en los pasos de configuración, Copilot lo hará automáticamente cuando estos terminen.
      contents: read
    steps:
      - name: Checkout code
        uses: actions/checkout@v5

      # Configuración del frontend y la aplicación: Node.js (ahora toda la aplicación usa Astro + Drizzle/libSQL)
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

      # Migra y precarga la base de datos SQLite local para que la compilación y las pruebas tengan datos.
      - name: Set up the database
        run: npm run db:setup
```

El flujo contiene un único trabajo, `copilot-setup-steps`, que se ejecuta antes de que Copilot empiece la PR. El desencadenador `workflow_dispatch` permite probarlo manualmente desde la pestaña **Actions**.

## Añade documentación

La documentación obsoleta o inexistente ralentiza el trabajo y dificulta el mantenimiento. Asigna al agente una tarea para documentar el código; revisarás la PR en un ejercicio posterior.

1. En una pestaña nueva, abre el repositorio en github.com.
2. Selecciona la pestaña **Issues** y, después, **New issue**.
3. Selecciona **Blank issue**.
4. Escribe `Code lacks documentation` en **Title**.
5. En **Description**, introduce:

   ```plaintext
   Our organization has a requirement that functions and methods include TSDoc doc comments where helpful. Unfortunately, recent updates haven't followed this standard. We need to update the existing code to ensure doc comments are included where they clarify behavior.
   ```

6. Selecciona **Create**.
7. En el panel derecho, selecciona **Assign to Copilot** y, en el diálogo, selecciona **Assign**.

   ![Asignar Copilot a una incidencia](../../../_images/shared-assign-copilot.png)

8. Selecciona la pestaña **Pull Requests** y abre la PR nueva, con un título similar a `[WIP]: Code lacks documentation`. Si aún no aparece, espera un momento y actualiza la página.
9. Al cabo de unos minutos, revisa la lista de tareas que Copilot ha creado.

> [!NOTE]
> La lista puede tardar varios minutos en aparecer mientras Copilot prepara el entorno, ejecuta el flujo de trabajo y analiza el proyecto.

10. Revisa las tareas y desplázate por la cronología de la PR hasta ver que Copilot ha empezado a trabajar.
11. Selecciona **View session**. Si el indicador aún no aparece, actualiza la página.
12. Explora la sesión en directo para ver cómo Copilot examina el código, analiza el estado, planifica y crea código.

El agente trabaja de forma asíncrona, así que puedes ocuparte de otra tarea mientras tanto. Asignarás una segunda tarea y después trabajarás en una funcionalidad de la aplicación.

## Añade una sección de juegos relacionados

Delega en el agente la tarea de mostrar juegos relacionados en la página de detalles.

1. Vuelve al repositorio en github.com.
2. Selecciona **Issues** y **New issue**.
3. Selecciona **Blank issue**.
4. Escribe `Show related games on the game details page` en **Title**.
5. En **Description**, introduce:

   ```markdown
   We want to help people discover more games by showing related games on each game's details page. The design team wants to explore the UX and do some acceptance testing. Our requirements are:

   - Add a data-access helper in `src/lib/` that returns other games in the same category as a given game, excluding that game
   - Show a "Related games" section on the game details page that uses the helper
   - Handle the case where a game has no related games
   - There should be unit tests created for the new helper
   - Before creating the PR, ensure all tests pass
   ```

6. Selecciona **Create**. En el panel derecho, selecciona **Assign to Copilot** y, después, **Assign**.

   ![Asignar Copilot a una incidencia](../../../_images/shared-assign-copilot.png)

Verás un emoji de ojos en el primer comentario de la incidencia mientras Copilot trabaja. No es necesario supervisar continuamente al agente; revisa el trabajo cuando termine. A continuación, crearás y usarás agentes personalizados.

## Resumen y siguientes pasos

Has explorado el [agente de GitHub Copilot en la nube][copilot-agents]. Puedes asignarle incidencias para que trabaje de forma asíncrona en deuda técnica, nuevas funcionalidades o migraciones entre frameworks.

En esta lección:

- personalizaste el entorno de generación de código.
- consideraste la seguridad de las operaciones.
- definiste incidencias con un ámbito claro.
- asignaste incidencias a Copilot.

Ahora crearás y usarás agentes personalizados. El agente en la nube también puede usar [servidores MCP][cloud-agent-mcp] e instrucciones personalizadas.

## Recursos

- [Acerca del agente de Copilot en la nube][copilot-agents]
- [Asignar incidencias de GitHub a Copilot][assign-issue]
- [Buenas prácticas del flujo de configuración del agente][cloud-agent-best-practices]

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
