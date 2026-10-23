---
title: "Lección 6 - Validar la funcionalidad con Playwright MCP"
description: "Inspecciona o configura Playwright MCP en Copilot CLI y explora el filtrado en un navegador."
authors:
  - geektrainer
lastUpdated: 2026-09-18
---

Escribir código es solo una parte del trabajo. También hay que trabajar con datos y servicios externos y proporcionar automatizaciones adicionales a Copilot. Los servidores MCP amplían las herramientas y servicios disponibles en Copilot CLI.

En esta lección:

- conocerás Model Context Protocol (MCP) y cómo lo usa Copilot CLI.
- añadirás el servidor MCP de Playwright si aún no está disponible.
- pedirás al agente que controle un navegador y explore el filtrado.

## Escenario

Las pruebas unitarias y end-to-end son importantes, pero para validar cambios en la interfaz también hay que interactuar con ella. Permitirás que Copilot use el sitio web como lo haría una persona para automatizar la comprobación de los cambios.

## ¿Qué es Model Context Protocol (MCP)?

[Model Context Protocol (MCP)][mcp-blog-post] permite a los agentes de IA comunicarse con herramientas y servicios externos en tiempo real, acceder a información actualizada y realizar acciones en tu nombre.

Los agentes acceden a estas herramientas y recursos mediante servidores MCP, que actúan como puente entre el agente y los servicios. Cada servidor ofrece un conjunto distinto de herramientas y recursos.

- **[GitHub MCP Server][github-mcp]** proporciona acceso a API para gestionar repositorios, incidencias y pull requests de GitHub.
- **[Playwright MCP Server][playwright-mcp-server]** permite automatizar el navegador con Playwright.

GitHub aloja un [registro de MCP][mcp-registry] para facilitar el descubrimiento y las contribuciones al ecosistema.

> [!CAUTION]
> Trata los servidores MCP como cualquier otra dependencia. Antes de usar uno, revisa su código fuente, verifica quién lo publica y valora las implicaciones de seguridad.

## Añade el servidor MCP de Playwright

Si el servidor MCP de Playwright aún no está disponible, añádelo para que Copilot pueda interactuar con el sitio como lo haría un usuario.

> [!NOTE]
> Si Playwright MCP ya está configurado en Copilot CLI, omite estos pasos y continúa con la sección siguiente.

1. Vuelve a tu Codespace.
2. En Copilot CLI, introduce este comando para abrir el diálogo de servidores MCP:

   ```plaintext
   /mcp add
   ```

3. Escribe `playwright` como nombre y pulsa <kbd>Tab</kbd>.
4. Confirma el tipo **STDIO** con <kbd>Enter</kbd> y pulsa <kbd>Tab</kbd>.
5. Pega este comando en el diálogo **Command**:

   ```plaintext
   npx -y @playwright/mcp@latest --headless --no-sandbox
   ```

6. Pulsa <kbd>Control</kbd>+<kbd>S</kbd> (Mac) o <kbd>Ctrl</kbd>+<kbd>S</kbd> (Windows/Linux) para guardar el servidor MCP.
7. Pulsa <kbd>Esc</kbd> para salir del diálogo.

## Pide a Copilot que explore la funcionalidad con Playwright

Ya has comprobado manualmente que la funcionalidad funciona. Ahora usarás el servidor MCP de Playwright disponible en Copilot para hacer la misma prueba.

1. Envía este prompt para pedir a Copilot que valide la funcionalidad:

   ```plaintext
   Start the app and use Playwright MCP to check filtering against the issue and our plan. Tell me what works and what doesn't, without making changes. Stop the server you started when you're done.
   ```

> [!NOTE]
> No es imprescindible indicar a Copilot que use el servidor MCP; normalmente lo detecta por sí solo. Especificarlo puede ayudarle a usar la herramienta prevista.

2. Observa cómo Copilot realiza los pasos en el navegador para comprobar la funcionalidad.
3. Lee el informe y confirma que todo se comporta como se esperaba.

Copilot iniciará el servidor, usará Playwright para interactuar con el sitio, detendrá el servidor y te entregará un informe.

## Resumen y siguientes pasos

Has usado el servidor MCP de Playwright para explorar la funcionalidad en un navegador real desde Copilot CLI. En concreto:

- aprendiste qué es MCP y cómo lo usa Copilot CLI.
- añadiste el servidor MCP de Playwright si no estaba configurado.
- pediste al agente que controlara un navegador y explorara el filtrado.

A continuación, [crearás un agente personalizado de control de calidad][next-lesson] que combinará la habilidad con las herramientas del navegador.

## Recursos

- [¿Qué demonios es MCP y por qué todo el mundo habla de ello?][mcp-blog-post]
- [Servidor MCP de Playwright de Microsoft][playwright-mcp-server]
- [Añadir servidores MCP a Copilot CLI][add-mcp]

[previous-lesson]: ../5-agent-skills/
[next-lesson]: ../7-qa-agent/
[mcp-blog-post]: https://github.blog/ai-and-ml/llms/what-the-heck-is-mcp-and-why-is-everyone-talking-about-it/
[playwright-mcp-server]: https://github.com/microsoft/playwright-mcp
[github-mcp]: https://github.com/github/github-mcp-server
[mcp-registry]: https://github.com/mcp
[add-mcp]: https://docs.github.com/copilot/how-tos/copilot-cli/customize-copilot/add-mcp-servers
