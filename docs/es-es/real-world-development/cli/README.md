---
slug: es-es/real-world-development/cli
title: "GitHub Copilot CLI"
description: "Desarrolla, verifica y entrega dos cambios de Tailspin Toys mientras aprendes los modos, las personalizaciones, las herramientas MCP y la automatización de pull requests de Copilot CLI."
authors:
  - geektrainer
lastUpdated: 2026-09-18
---

**[GitHub Copilot CLI][about-copilot-cli]** incorpora GitHub Copilot a tu terminal como asistente de programación con agentes. Explora bases de código, genera código, ejecuta comandos y se conecta a herramientas externas, todo desde la línea de comandos, para que puedas mantener el flujo sin cambiar a un editor gráfico.

El taller sigue un flujo de trabajo continuo en Tailspin Toys:

1. Prepararás el proyecto en GitHub Codespaces, instalarás Copilot CLI y te familiarizarás con el entorno.
2. Añadirás una valoración con estrellas, revisarás el resultado en el navegador y fusionarás manualmente tu primera pull request (PR).
3. Partirás de la incidencia de filtrado, definirás el enfoque en Plan, lo implementarás en Autopilot y lo revisarás en Interactive.
4. Actualizarás las instrucciones del repositorio y las aplicarás al filtrado.
5. Adaptarás la habilidad existente `quality-checks` y la usarás para ejecutar las comprobaciones del proyecto.
6. Añadirás el servidor Model Context Protocol (MCP) de Playwright y lo usarás para explorar el filtrado en un navegador.
7. Crearás un agente personalizado de control de calidad (QA) para revisar los requisitos, la cobertura y las pruebas de verificación.
8. Revisarás todos los cambios del filtrado y usarás Agent Merge para la PR.
9. Explorarás comandos de barra para el contexto, los modelos, el uso compartido y la delegación opcional al agente en la nube.

Para simplificar el taller, crearás dos PR: una para las valoraciones con estrellas y otra para el filtrado, las instrucciones, la habilidad, el perfil de QA y las pruebas. El filtrado y el flujo de calidad comparten conversación y rama para que puedas partir del trabajo anterior mientras exploras cada herramienta.

## Ejercicios

| Lección | Tema | Descripción |
| ------ | ----- | ----------- |
| [0. Requisitos previos][ex0] | Configuración | Crear el repositorio y el Codespace |
| [1. Instalar Copilot CLI][ex1] | Instalación | Instalar y autenticar Copilot CLI, y familiarizarse con el entorno |
| [2. Añadir valoraciones con estrellas][ex2] | Primer cambio | Mostrar las valoraciones existentes y el estado sin valoración; después, fusionar la primera PR |
| [3. Modos del agente: Plan y Autopilot][ex3] | Modos del agente | Planificar la funcionalidad desde la incidencia, crearla con Autopilot y revisarla en Interactive |
| [4. Orientar Copilot con instrucciones personalizadas][ex4] | Contexto | Explorar y actualizar las instrucciones y aplicarlas al filtrado |
| [5. Personalizar y usar la habilidad quality-checks][ex5] | Comprobaciones repetibles | Explorar la habilidad existente, cambiar el formato del informe y ejecutarla |
| [6. Validar la funcionalidad con Playwright MCP][ex6] | Observación en el navegador | Configurar MCP en la CLI e inspeccionar el filtrado |
| [7. Crear y usar un agente de control de calidad][ex7] | Requisitos y cobertura | Crear y seleccionar un perfil especializado y reunir pruebas finales |
| [8. Crear y fusionar la PR de funcionalidad][ex8] | Revisión y fusión | Revisar el cambio completo, crear la PR y usar Agent Merge |
| [9. Comandos de barra en GitHub Copilot CLI][ex9] | Funciones de CLI | Explorar el contexto, los modelos, el uso compartido y la delegación opcional |
| [10. Repaso y siguientes pasos][ex10] | Resumen | Revisar el flujo, las personalizaciones reutilizables y otros recursos |
| [Opcional: Incorporar Foundry][foundry] | Agentes hospedados | Preparar un modelo, desplegar un agente basado en el catálogo y conectarlo al sitio |

## Requisitos previos

Antes de asistir a este taller, asegúrate de tener:

- [ ] Una cuenta de GitHub con un plan activo de **Copilot Student, Pro, Pro+, Business o Enterprise**
- [ ] Permiso para crear un repositorio y un Codespace
- [ ] Conocimientos básicos de operaciones de terminal/línea de comandos

> [!TIP]
> ¿No tienes un plan de pago? Los estudiantes cuya condición se haya verificado pueden obtener GitHub Copilot gratis mediante [GitHub Education][student-plan]. El plan **Copilot Student** incluye las funciones de agente, MCP, revisión de código y Copilot CLI que usa este taller.

[callout-student-plan-education]: https://github.com/education/students

> [!NOTE]
> Si usas Copilot Business o Copilot Enterprise, asegúrate de que tu administrador haya habilitado Copilot CLI.

## Primeros pasos

**[Empieza con los requisitos previos →][ex0]**

[about-copilot-cli]: https://docs.github.com/copilot/concepts/agents/about-copilot-cli
[student-plan]: https://github.com/education/students
[ex0]: 0-prerequisites/
[ex1]: 1-install-copilot-cli/
[ex2]: 2-add-star-rating/
[ex3]: 3-agent-modes/
[ex4]: 4-custom-instructions/
[ex5]: 5-agent-skills/
[ex6]: 6-mcp-playwright/
[ex7]: 7-qa-agent/
[ex8]: 8-create-pull-request/
[ex9]: 9-cli-power-tools/
[ex10]: 10-review/
[foundry]: 8-foundry-agent/
