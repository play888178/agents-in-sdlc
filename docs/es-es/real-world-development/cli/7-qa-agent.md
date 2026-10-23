---
title: "Lección 7 - Crear y usar un agente de control de calidad"
description: "Crea un agente de control de calidad que combine los requisitos de la incidencia, la habilidad quality-checks y Playwright MCP."
authors:
  - geektrainer
lastUpdated: 2026-09-18
---

Has usado la habilidad `quality-checks` para ejecutar comprobaciones automatizadas y Playwright MCP para observar el filtrado en un navegador. Ahora reunirás estas capacidades en un agente personalizado con un proceso de control de calidad definido.

En esta lección:

- explorarás cómo un agente personalizado trabaja con instrucciones, habilidades y herramientas MCP.
- crearás e inspeccionarás un perfil reutilizable de control de calidad.
- seleccionarás el agente y revisarás sus resultados según la incidencia de filtrado.

## Escenario

Tailspin Toys quiere revisar de forma coherente los requisitos, la calidad del código, las comprobaciones automatizadas, la cobertura de pruebas y el comportamiento en el navegador antes de abrir una pull request (PR). Un agente personalizado puede coordinar este proceso y crear un informe reutilizable.

## ¿Qué es un agente personalizado?

Un agente personalizado es una versión especializada de Copilot definida en un perfil Markdown. El perfil describe su propósito, instrucciones y herramientas disponibles. En este taller definirás el rol de control de calidad en `.github/agents/qa.agent.md` y lo seleccionarás en Copilot CLI.

Cada personalización cumple una función: las instrucciones del repositorio describen los estándares del equipo; la habilidad `quality-checks` agrupa comprobaciones repetibles; Playwright MCP proporciona herramientas de navegador; y el perfil de control de calidad indica a Copilot cómo usar esas capacidades para evaluar los requisitos e informar de los resultados. No sustituye a las demás personalizaciones ni requiere otra conversación.

## Crea el perfil de control de calidad

Antes de abrir la PR de la funcionalidad, pedirás a Copilot que cree un perfil reutilizable con las comprobaciones y límites que debe respetar.

1. Vuelve a tu Codespace y confirma que la conversación de filtrado está en modo Interactive.
2. Pide al agente predeterminado que cree el agente personalizado:

   ```plaintext
   Create a custom agent named QA in .github/agents/qa.agent.md. It should check features against their issues and agreed requirements, follow the repository instructions, run the quality-checks skill, use Playwright MCP to verify behavior, and add tests when coverage is missing.

   Have it report each requirement as pass, fail, or blocked with supporting evidence. It must ask before changing implementation code, and it must not commit changes or open pull requests.

   Just create the profile for now so I can review it.
   ```

## Inspecciona el perfil

Revisa el perfil generado antes de usar el agente para confirmar que contiene el proceso y los límites previstos.

1. Introduce `/diff` y abre `.github/agents/qa.agent.md`.
2. Lee el frontmatter. `description` es obligatorio; `name` es opcional, pero proporciona un nombre visible claro.
3. Comprueba que el perfil empieza por los requisitos, sigue las instrucciones del repositorio, ejecuta la habilidad `quality-checks` y usa Playwright MCP.
4. Confirma que el informe incluye pruebas que respaldan sus resultados, que pide permiso antes de cambiar el código de implementación y que no confirma cambios ni abre pull requests.
5. Si falta alguna responsabilidad o límite, pide al agente predeterminado que revise el perfil antes de continuar.

## Ejecuta el control de calidad según la incidencia

Copilot CLI carga los agentes del proyecto al iniciar una conversación. Reanuda la conversación de filtrado para que el agente pueda usar la incidencia y las decisiones de planificación existentes.

1. Activa el agente con este prompt:

   ```plaintext
   /agent QA
   ```

> [!NOTE]
> Como acabas de crear el agente, puede que no aparezca en la lista. Aun así, el comando anterior lo activará.

2. Pídele que revise la funcionalidad:

   ```plaintext
   Review the filtering feature against the issue and the decisions in our plan. Is it ready for a PR?
   ```

3. Espera a que termine el agente.
4. Lee el informe.

## Resumen y siguientes pasos

Has añadido un rol especializado reutilizable al flujo y has revisado su trabajo. En esta lección:

- exploraste cómo trabaja un agente personalizado con instrucciones, habilidades y herramientas MCP.
- creaste e inspeccionaste un perfil reutilizable de control de calidad.
- seleccionaste el agente y revisaste sus resultados según la incidencia.

Ya tienes la implementación, la habilidad actualizada, el perfil de control de calidad, las pruebas y el informe de verificación. A continuación, [reunirás todo en una PR de funcionalidad y usarás Agent Merge][next-lesson].

## Recursos

- [Crear agentes personalizados para Copilot CLI][create-agents]
- [Configuración de agentes personalizados][agent-config]

[previous-lesson]: ../6-mcp-playwright/
[next-lesson]: ../8-create-pull-request/
[create-agents]: https://docs.github.com/copilot/how-tos/copilot-cli/customize-copilot/create-custom-agents-for-cli
[agent-config]: https://docs.github.com/copilot/reference/custom-agents-configuration
