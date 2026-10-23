---
title: "Ejercicio 3 - Probar la funcionalidad con el servidor MCP de Playwright"
authors:
  - geektrainer
lastUpdated: 2026-06-30
---

[← Lección anterior: Modo de agente][previous-lesson] · [Siguiente lección: Agentes personalizados →][next-lesson]

Ya has creado el filtrado con el modo de agente. Antes de abrir una pull request, confirma que funciona en el navegador. Conectarás el **servidor MCP de Playwright** para que Copilot controle un navegador real y pruebe la funcionalidad. Después, publicarás la rama y abrirás la PR.

En este ejercicio:

- aprenderás qué es Model Context Protocol (MCP) y cómo sus servidores amplían las herramientas de Copilot.
- añadirás el servidor MCP de Playwright al espacio de trabajo.
- pedirás a Copilot que pruebe manualmente el filtrado en un navegador.
- publicarás la rama y abrirás una pull request.

## ¿Qué es Model Context Protocol (MCP)?

El modo de agente es más potente cuando puede acceder a recursos externos. Model Context Protocol (MCP) es un estándar que permite a Copilot comunicarse con herramientas y servicios externos.

![Diagrama del funcionamiento interno del modo de agente y su interacción con el contexto, el LLM y las herramientas, incluidas las proporcionadas por servidores MCP y extensiones de VS Code](../../../_images/ex1-mcp-diagram.png)

[MCP](https://github.blog/ai-and-ml/llms/what-the-heck-is-mcp-and-why-is-everyone-talking-about-it/) permite a los agentes de IA comunicarse en tiempo real con herramientas y servicios externos, acceder a información actualizada mediante recursos y realizar acciones en tu nombre mediante herramientas. Un servidor MCP conecta el agente con esas herramientas; cada servidor proporciona un conjunto distinto.

- **[GitHub MCP Server](https://github.com/github/github-mcp-server)** proporciona acceso a API para gestionar repositorios, incidencias y pull requests.
- **[Playwright MCP Server](https://github.com/microsoft/playwright-mcp)** permite automatizar el navegador con Playwright.

GitHub aloja un [registro de MCP](https://github.com/mcp) para facilitar el descubrimiento de servidores y las contribuciones.

> [!CAUTION]
> Trata los servidores MCP como cualquier otra dependencia. Revisa cuidadosamente su código fuente, verifica quién los publica y valora las implicaciones de seguridad. Usa solo servidores de confianza y sé prudente al conceder acceso a recursos u operaciones sensibles.

## Revisa la configuración MCP

El archivo `.vscode/mcp.json` configura los servidores MCP disponibles en el espacio de trabajo de VS Code.

1. Abre `.vscode/mcp.json` en el codespace.
2. Comprueba que ya hay configurado un servidor `github`:

   ```json
   {
     "servers": {
       "github": {
         "type": "http",
         "url": "https://api.githubcopilot.com/mcp/"
       }
     }
   }
   ```

La plantilla incluye la entrada `github`, que permitió a Copilot leer el backlog en el ejercicio anterior. Usa el servidor GitHub MCP remoto; VS Code se conecta mediante HTTP y solicita autenticación cuando Copilot usa una herramienta por primera vez.

## Añade el servidor MCP de Playwright

Añade un segundo servidor. Playwright MCP proporciona a Copilot un navegador que puede controlar para probar la funcionalidad.

1. En `.vscode/mcp.json`, añade `playwright` junto a `github`:

   ```json
   {
     "servers": {
       "github": {
         "type": "http",
         "url": "https://api.githubcopilot.com/mcp/"
       },
       "playwright": {
         "command": "npx",
         "args": ["@playwright/mcp@latest", "--headless"]
       }
     }
   }
   ```

2. Guarda el archivo.

`playwright` es un servidor **local** que VS Code inicia con `npx @playwright/mcp@latest`. La opción `--headless` ejecuta el navegador sin ventana visible, necesaria en un codespace sin escritorio.

> [!NOTE]
> Tailspin Toys ya usa Playwright para pruebas end-to-end, así que normalmente el navegador está instalado. Si Copilot informa de que falta, pídele que ejecute `npx playwright install chromium` y vuelve a intentarlo.

## Inicia y confía en el servidor

VS Code inicia los servidores MCP cuando se necesitan y te pide que confirmes si confías en ellos.

1. En `.vscode/mcp.json`, selecciona **Start**, que aparece sobre la entrada `playwright`, para iniciar el servidor y comprobar la configuración.
2. Si VS Code pregunta si confías en el servidor, revisa la configuración y confirma que confías en él.

Iniciar el servidor manualmente es opcional. Si omites estos pasos, VS Code lo iniciará y te pedirá permiso la primera vez que Copilot necesite sus herramientas.

## Prueba el filtrado

Playwright MCP permite que Copilot abra páginas, navegue, aplique filtros y te comunique lo que ha observado. Trabaja con el [árbol de accesibilidad][playwright-mcp-server] de la página en lugar de usar capturas de pantalla; el agente examina elementos estructurados y etiquetados, como botones, enlaces y elementos de lista. Así, una comprobación funcional también ofrece una verificación básica de accesibilidad.

Con el servidor conectado y la aplicación en ejecución, pide a Copilot que pruebe el filtrado:

```text
Using the Playwright MCP server, open a browser to the running app at http://localhost:4321 and verify the new game filtering feature:

1. Go to the games page and note how many games are listed.
2. Apply a category filter and confirm the list updates to only show games in that category.
3. Clear it, then apply a publisher filter and confirm the list updates to that publisher.
4. Combine a category and a publisher filter and confirm the results respect both.

Report what you observe at each step, and call out anything that does not behave as expected.
```

Compara el informe con los criterios de aceptación de la incidencia. Si algo no parece correcto, pregunta o pide a Copilot que corrija el código antes de abrir la PR.

> [!NOTE]
> La aplicación debe estar disponible en `http://localhost:4321`. Si detuviste el servidor de desarrollo, vuelve a iniciarlo antes de enviar el prompt. La primera vez, Copilot puede necesitar descargar un navegador; si falta, pídele que ejecute `npx playwright install chromium`.

[playwright-mcp-server]: https://github.com/microsoft/playwright-mcp

## Publica la rama y crea una pull request

Cuando hayas confirmado que la funcionalidad funciona, publica la rama `filtering-vscode` para que el equipo pueda revisar los cambios.

1. Abre el panel **Source Control** del codespace y revisa los cambios de Copilot.
2. Prepara los cambios seleccionando el icono **+**.
3. Genera un mensaje de commit con el botón **Sparkle**.
4. Selecciona **Publish** para enviar la rama al repositorio.

   ![Panel Source Control con los cambios realizados](../../../_images/ex3-source-control-changes.png)

## Crea la pull request

Pide a Copilot que encuentre la incidencia correspondiente y cree una PR asociada.

1. Abre Copilot Chat y selecciona **New Chat**.
2. En el selector de agentes, confirma que está seleccionado **Agent** para que Copilot pueda usar las herramientas de GitHub.
3. Envía este prompt:

   ```plaintext
   Find the issue in the repo related to filtering by category and publisher. Create a new pull request for the current branch, and associate it with the correct issue.
   ```

4. Cuando sea necesario, selecciona **Continue** para permitir que Copilot reúna información y realice operaciones. La primera vez que use una herramienta de GitHub, sigue las indicaciones para autenticarte.
5. Comprueba que Copilot encuentra la incidencia adecuada y crea la PR.
6. Selecciona el vínculo de la PR para revisarla, pero **no la fusiones todavía**.

## Resumen y siguientes pasos

En este ejercicio:

- aprendiste qué es MCP y cómo amplían sus servidores las herramientas de Copilot.
- añadiste el servidor MCP de Playwright al espacio de trabajo.
- lo usaste para probar manualmente el filtrado en un navegador.
- publicaste la rama y abriste una pull request.

Has usado un servidor MCP para probar una funcionalidad. A continuación, [crearás un agente personalizado][next-lesson] para agilizar tareas como las revisiones de accesibilidad.

## Recursos

- [¿Qué demonios es MCP y por qué todo el mundo habla de ello?][mcp-blog-post]
- [Servidor MCP de Playwright de Microsoft][playwright-mcp-server]
- [Servidor MCP de GitHub][github-mcp-server]
- [Registro de MCP de GitHub][mcp-registry]
- [Servidores MCP en VS Code][vscode-mcp-config]

[previous-lesson]: ../2-agent-mode/
[next-lesson]: ../4-custom-agents/
[mcp-blog-post]: https://github.blog/ai-and-ml/llms/what-the-heck-is-mcp-and-why-is-everyone-talking-about-it/
[github-mcp-server]: https://github.com/github/github-mcp-server
[mcp-registry]: https://github.com/mcp
[remote-github-mcp-server]: https://github.blog/changelog/2025-06-12-remote-github-mcp-server-is-now-available-in-public-preview/
[vscode-mcp-config]: https://code.visualstudio.com/docs/agents/reference/mcp-configuration
