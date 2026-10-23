---
title: "Ejercicio 4 - Agentes personalizados"
authors:
  - geektrainer
lastUpdated: 2026-06-30
---

## ¿Qué son los agentes personalizados?

Los [agentes personalizados][custom-agents-concept] de GitHub Copilot permiten crear asistentes de IA especializados en tareas o dominios del flujo de desarrollo. Al definirlos mediante archivos Markdown en `.github/agents`, puedes proporcionar a Copilot instrucciones, buenas prácticas, patrones de código y conocimiento del dominio. Los equipos pueden convertir su experiencia en agentes reutilizables, como un agente de accesibilidad que compruebe el cumplimiento de las [Pautas de Accesibilidad para el Contenido Web (WCAG)][wcag], uno de seguridad o uno de pruebas.

Los agentes personalizados se definen en archivos Markdown de `.github/agents` o globalmente en `~/.copilot/agents`. Cada archivo incluye frontmatter YAML con al menos `name` y `description`, seguido de un prompt Markdown que define el comportamiento, la especialización y las instrucciones.

### Agentes personalizados y habilidades de agente

Los agentes personalizados y las [habilidades de agente][agent-skills-concept] se definen principalmente en Markdown y explican a la IA cómo realizar operaciones. La distinción más sencilla es que el **agente personalizado** realiza el trabajo y las **habilidades** son herramientas.

Los agentes personalizados tienen su propia ventana de contexto y pueden orquestar habilidades e incluso otros agentes. En este laboratorio, el agente de accesibilidad revisa y actualiza el sitio según las directrices de accesibilidad; podría invocar habilidades para gestionar pull requests o ejecutar pruebas.

> [!NOTE]
> No hay una única forma correcta de crear un agente personalizado. Prueba e itera para descubrir qué funciona mejor en cada entorno y escenario.

[custom-agents-concept]: https://docs.github.com/copilot/concepts/agents/cloud-agent/about-custom-agents
[agent-skills-concept]: https://docs.github.com/copilot/concepts/agents/about-agent-skills
[wcag]: https://www.w3.org/WAI/standards-guidelines/wcag/

## Escenario

Tailspin Toys quiere que su plataforma de financiación colectiva sea accesible para todas las personas, independientemente de sus capacidades visuales o preferencias. Algunos usuarios indican que el tema oscuro actual es difícil de leer por el contraste insuficiente entre texto y fondo. El equipo de diseño solicita un modo de alto contraste que los usuarios puedan activar y desactivar.

La accesibilidad es prioritaria, así que usarás un agente personalizado para generar la funcionalidad.

En este ejercicio:

- revisarás un agente personalizado de accesibilidad existente.
- usarás el agente de accesibilidad en Copilot Chat para implementar el modo de alto contraste.

## Revisa el agente personalizado de accesibilidad

Revisarás el agente existente para comprender cómo orientará a Copilot. Vuelve al codespace y crea una rama nueva desde `main` para mantener aparte la PR de filtrado del ejercicio 3.

```bash
git checkout main
git pull
git checkout -b accessibility-vscode
```

1. Abre `.github/agents/accessibility.md`.
2. Fíjate en el frontmatter YAML con los campos `name` y `description`.

> [!CAUTION]
> El frontmatter con `name` y `description` es obligatorio para los agentes personalizados.

3. Revisa las secciones de responsabilidades principales, buenas prácticas, ejemplos de HTML, CSS y JavaScript, y errores habituales.

## Usa el agente personalizado en Copilot Chat

VS Code muestra en el selector de agentes de Copilot Chat los agentes personalizados definidos en `.github/agents`. Selecciona uno para aplicar su configuración de instrucciones y herramientas a la conversación.

> [!TIP]
> **Abre Copilot Chat**
>
> Vuelve al codespace, abre el panel Copilot Chat y selecciona **New Chat** para iniciar una conversación limpia. El modo y el modelo se especificarán cuando sean relevantes.

1. En el selector de agentes de la vista Chat, selecciona **Agent** si aún no está seleccionado.
2. En la parte inferior de la vista Chat, abre el selector de agentes. Debería mostrar el agente activo, que de forma predeterminada es **default**.
3. Selecciona **Accessibility agent**.
4. Envía este prompt al agente:

   ```plaintext
   Add a high-contrast mode to the site. There should be a toggle for high contrast which the user can set, and the setting should persist across page reloads using local storage on the browser.
   ```

5. Copilot explorará el código, propondrá cambios y editará los archivos del proyecto. Cada edición aparecerá en el chat con la ruta y las diferencias para que puedas revisarla.
6. Observa cómo se actualiza el indicador **Files changed** con los archivos modificados.

> [!NOTE]
> El proceso puede tardar unos minutos. Copilot modifica archivos reales, como componentes Astro, estilos y pruebas relacionadas.

Revisarás y dirigirás el trabajo en curso en el siguiente ejercicio.

## Resumen y siguientes pasos

Has explorado los [agentes personalizados][custom-agents], asistentes especializados en tareas y dominios concretos. Permiten codificar la experiencia y los estándares del equipo en agentes reutilizables.

En esta lección:

- revisaste cómo se definen los agentes personalizados.
- usaste un agente personalizado en el modo de agente de Copilot Chat.

A continuación, [supervisarás y dirigirás el trabajo del agente][next-lesson], revisarás los cambios mientras se realizan y añadirás un modo claro a la misma sesión.

## Recursos

- [Acerca de los agentes personalizados][custom-agents]
- [Crear agentes personalizados en el IDE][creating-custom-agents-ide]
- [Agentes personalizados en VS Code][custom-agents-vscode]
- [Configuración de agentes personalizados][custom-agents-config]
- [Agentes personalizados en awesome-copilot][awesome-copilot-agents]

---

| [← Lección anterior: Probar la funcionalidad con Playwright MCP][previous-lesson] | [Siguiente lección: Supervisar y gestionar agentes →][next-lesson] |
|:--|--:|

[previous-lesson]: ../3-mcp/
[next-lesson]: ../5-managing-agents/
[custom-agents]: https://docs.github.com/copilot/concepts/agents/cloud-agent/about-custom-agents
[creating-custom-agents-ide]: https://docs.github.com/copilot/how-tos/use-copilot-agents/cloud-agent/create-custom-agents-in-your-ide
[custom-agents-vscode]: https://code.visualstudio.com/docs/copilot/customization/custom-agents
[custom-agents-config]: https://docs.github.com/copilot/reference/custom-agents-configuration
[awesome-copilot-agents]: https://github.com/github/awesome-copilot/tree/main/agents
