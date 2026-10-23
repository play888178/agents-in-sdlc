---
title: "Ejercicio 2 - Añadir funcionalidad con el modo de agente de Copilot"
authors:
  - geektrainer
lastUpdated: 2026-06-30
---

Hasta una actualización sencilla puede requerir cambios en varios archivos y ejecutar pruebas. El modo de agente de Copilot puede explorar la estructura del proyecto, localizar los archivos pertinentes, coordinar cambios y ejecutar tareas como pruebas. Si encuentra errores, puede corregirlos. En este ejercicio usarás el modo de agente para añadir funcionalidad al sitio.

> [!NOTE]
> Aunque sus nombres se parecen, el modo de agente y el agente en la nube ofrecen experiencias distintas. El modo de agente trabaja en tu IDE y permite iterar rápidamente. El agente en la nube trabaja de forma asíncrona como un miembro del equipo y se comunica mediante incidencias y pull requests.

En este ejercicio aprenderás cómo el modo de agente explora el proyecto, implementa funcionalidades en la interfaz y la capa de datos, y genera cambios y pruebas que debes revisar antes de fusionarlos.

## Escenario

El catálogo de juegos crece y quieres que los usuarios puedan filtrar por categoría y editorial. En el ejercicio anterior creaste un helper de editoriales; ahora completarás con el modo de agente el trabajo restante en la capa de datos, la interfaz y las pruebas.

## Ejecuta el sitio de Tailspin Toys

Antes de cambiar el código, explora el sitio para conocer su funcionalidad actual. Tailspin Toys es una plataforma de financiación colectiva de juegos de mesa con un tema visual para desarrolladores. Es una aplicación Astro que genera páginas HTML estáticas durante la compilación. Las páginas consultan directamente una base de datos SQLite local mediante Drizzle ORM; no hay una API de backend independiente ni un framework de interfaz del lado del cliente. Los helpers de acceso a datos reutilizables están en `src/lib/`; la interactividad usa scripts Astro acotados y las API DOM estándar.

### Inicia el sitio web

1. Vuelve al codespace; después del ejercicio anterior, deberías estar en `main`.
2. Abre un terminal con <kbd>Control</kbd>+<kbd>`</kbd> (Mac) o <kbd>Ctrl</kbd>+<kbd>`</kbd> (Windows/Linux).
3. Actualiza `main` y crea una rama:

   ```bash
   git checkout main
   git pull
   git checkout -b filtering-vscode
   ```

4. Inicia el sitio:

   ```bash
   npm run dev
   ```

   Cuando el servidor de desarrollo Astro esté listo, aparecerá una URL similar a `http://localhost:4321`.
5. Mantén pulsada la tecla <kbd>Command</kbd> (Mac) o <kbd>Ctrl</kbd> (Windows/Linux) y selecciona la dirección `http://localhost:4321` en el terminal.

> [!NOTE]
> En Codespaces, la dirección localhost se redirige a `https://<your-codespace-name>-4321.app.github.dev/`, un túnel privado al servidor del codespace.

La página principal muestra juegos de mesa con títulos, imágenes y descripciones. Al seleccionar un juego, se abre su página de detalles con información como el título, la descripción, la editorial y la categoría.

## Explora el backlog con Copilot

La aplicación funciona, pero quieres añadir capacidades. Pide a Copilot que muestre el backlog creado automáticamente al crear el repositorio a partir de la plantilla.

> [!TIP]
> **Abre Copilot Chat**
>
> Vuelve al codespace, abre el panel Copilot Chat y selecciona **New Chat** para iniciar una conversación limpia. El modo y el modelo se especifican cuando son relevantes.

1. En el selector de agentes de la vista Chat, selecciona **Agent**. Este agente puede planificar e implementar cambios en varios archivos, ejecutar comandos del terminal e invocar herramientas.

   ![Selector de agentes en la vista Chat](../../../_images/shared-chat-mode-selector.png)

2. Selecciona **Claude Sonnet 4.5** en la lista de modelos.

> [!CAUTION]
> No se recomienda un modelo en particular. Este taller se creó con Claude Sonnet 4.5 para que las sugerencias sean relativamente coherentes, pero los modelos de lenguaje son probabilísticos y los resultados pueden variar.

> [!NOTE]
> Copilot puede usar un comando MCP diferente, pero debería poder completar la tarea.

3. Envía este prompt:

   ```plaintext
   Please show me the backlog of items from my GitHub repository. Help me prioritize them based on those which will be most useful to the user.
   ```

4. Selecciona **Continue** para ejecutar el comando que enumera las incidencias.
5. Revisa la lista y la prioridad propuesta.

## Revisa los archivos de instrucciones

Antes de pedir al agente que genere código, revisa las instrucciones que proporcionan contexto. El archivo de [interfaz de usuario (UI)](https://github.com/github-samples/tailspin-toys/blob/main/.github/instructions/ui.instructions.md) explica cómo añadir funcionalidad al sitio.

1. Abre `.github/instructions/ui.instructions.md` en el codespace.
2. Revisa la arquitectura, las pautas de diseño de componentes, pruebas y accesibilidad, y los vínculos a instrucciones de Astro, Tailwind CSS y Drizzle.

> [!TIP]
> Los archivos de instrucciones pueden enlazar a otros archivos de instrucciones y a archivos del proyecto. Las rutas son relativas a la ubicación del archivo, lo que permite reutilizar pautas y ejemplos.

## Implementa el filtrado

Completar el filtrado requiere cambios en la lógica de datos de `src/lib/`, pruebas de filtrado e interfaz de la lista de juegos. También hay que ejecutar las pruebas y confirmar que pasan antes de fusionar.

1. Continúa la conversación actual con Copilot o selecciona **New Chat**.
2. Selecciona **Add Context**, **Instructions** y **ui**.

   ![Ejemplo de selección del archivo de instrucciones de interfaz](../../../_images/ex3-select-instructions-file.png)

3. Confirma que **Agent** sigue seleccionado en el selector de agentes.
4. Confirma que sigue seleccionado el modelo **Claude Sonnet 4.5**.
5. Envía este prompt:

   ```plaintext
   Please update the site to include filtering by publisher and category based on the requirements from the related GitHub issue in the backlog. A publishers helper already exists from the previous exercise, so preserve and refine that work as needed while completing the remaining data-layer, UI, and tests. Ensure all tests are passing before completion. The server is already running, so you do not need to start it up.
   ```

6. Observa cómo Copilot explora el proyecto, localiza los helpers de datos, la interfaz y las pruebas, modifica archivos y ejecuta pruebas.
7. Cuando Copilot lo solicite, selecciona **Continue** para ejecutar las pruebas.
8. Si alguna prueba falla durante la iteración, deja que Copilot trabaje hasta que termine y no detecte errores. Mientras tanto, pueden aparecer los controles **Keep** y **Undo**; no hace falta seleccionarlos hasta que el agente termine.
9. Explora el código generado y busca problemas.

> [!CAUTION]
> Revisa siempre el código generado por Copilot o cualquier otra herramienta de IA.

10. Vuelve al navegador con el sitio en ejecución y explora la nueva funcionalidad.
11. Cuando hayas revisado el código y confirmado que funciona, selecciona **Keep** en la ventana de Copilot Chat.

## Resumen y siguientes pasos

Has usado el modo de agente de GitHub Copilot para añadir funcionalidad a Tailspin Toys. En concreto, has visto cómo explora el proyecto y coordina cambios en la interfaz y la capa de datos, y cómo revisar los cambios y pruebas generados antes de fusionar.

A continuación, [probarás la funcionalidad con el servidor MCP de Playwright][next-lesson] y abrirás una pull request.

### Ejercicio de exploración adicional: implementar paginación

La lista de juegos necesitará paginación a medida que crezca. Usa lo aprendido para pedir a Copilot que la implemente.

- Sigue las buenas prácticas y usa los archivos de instrucciones existentes.
- Decide si el tamaño de página debe ser fijo o configurable.
- Incluye en el prompt las directrices necesarias.
- Itera con Copilot y proporciona contexto cuando solicites cambios.

## Recursos

- [Modos ask, edit y agent de Copilot: qué hacen y cuándo usarlos][choose-mode]
- [Modo de agente en VS Code][vs-code-agent-mode]

[previous-lesson]: ../1-custom-instructions/
[next-lesson]: ../3-mcp/
[choose-mode]: https://github.blog/ai-and-ml/github-copilot/copilot-ask-edit-and-agent-modes-what-they-do-and-when-to-use-them/
[vs-code-agent-mode]: https://code.visualstudio.com/docs/copilot/chat/chat-agent-mode
