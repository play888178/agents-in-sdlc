---
title: "Lección 10 - Repaso y siguientes pasos"
description: "Repasa el flujo de trabajo con Copilot CLI, las dos pull requests, las personalizaciones reutilizables y otros recursos."
authors:
  - geektrainer
lastUpdated: 2026-09-29
---

Has usado GitHub Copilot CLI a lo largo de un flujo de trabajo continuo con Tailspin Toys. En concreto:

- preparaste un Codespace, instalaste Copilot CLI, exploraste el proyecto y localizaste la incidencia de filtrado precargada.
- añadiste valoraciones con estrellas, revisaste el resultado en un navegador reenviado y fusionaste manualmente tu primera pull request (PR).
- partiste de la incidencia de filtrado, definiste el enfoque en Plan, lo implementaste en Autopilot y lo revisaste en Interactive.
- orientaste al agente con instrucciones personalizadas, adaptaste la habilidad `quality-checks` y la usaste para ejecutar pruebas unitarias, lint y comprobaciones de tipos.
- añadiste el servidor Model Context Protocol (MCP) de Playwright y lo usaste para explorar el filtrado en un navegador real.
- creaste y seleccionaste un agente personalizado de control de calidad (QA) para evaluar requisitos, cobertura, resultados de la habilidad y pruebas en el navegador.
- revisaste todos los cambios del filtrado y autorizaste Agent Merge para la PR.
- exploraste comandos de barra para el contexto, los modelos, el uso compartido y la delegación opcional en la nube.

## Lo que has publicado

El taller incluye dos hitos de PR, cada uno en una rama creada a partir de `main` actualizado:

1. **Valoraciones con estrellas:** muestra en las tarjetas la propiedad `starRating` existente y un estado explícito cuando no hay valoración.
2. **Filtrado y flujo de calidad:** implementa el filtrado, actualiza las instrucciones y aplícalas a la funcionalidad, adapta el informe de `quality-checks`, crea un perfil de control de calidad e incluye las pruebas correspondientes.

Desde la planificación del filtrado hasta la apertura de su PR, usaste la misma conversación y rama. Agrupamos el trabajo en una PR para simplificar el taller.

## Distintos tipos de verificación

Comprobaste el código de varias formas: pruebas automatizadas, inspección propia en el navegador y exploración de Copilot mediante MCP. La habilidad `quality-checks` ejecutó pruebas unitarias, lint y comprobaciones de tipos, e informó de los resultados en el nuevo formato. El agente de control de calidad reunió esos resultados con la revisión de los requisitos y de la cobertura antes de crear la PR.

Las pruebas nuevas deben cubrir carencias reales; puede ser correcto que una revisión de control de calidad no necesite añadir pruebas. Revisa el código y las pruebas antes de autorizar la fusión y actualiza las pruebas afectadas después de los cambios.

## Buenas prácticas

El contexto y las herramientas que proporcionas determinan el trabajo de Copilot. En este taller actualizaste instrucciones, adaptaste una habilidad, creaste un perfil de control de calidad y configuraste un servidor MCP. Reutiliza estas personalizaciones en otras conversaciones y ajústalas a las necesidades del equipo. Las instrucciones definen estándares, las habilidades describen tareas repetibles, los agentes personalizados definen roles especializados y los servidores MCP conectan herramientas externas. Revisa los cambios y resultados reales, no solo el resumen del agente.

Adapta el **modo y el modelo** a la tarea. Usa **Plan** para estudiar el enfoque antes de desarrollar, **Interactive** para mantenerte al tanto de los cambios concretos y **Autopilot** en tareas bien delimitadas. Elige un modelo más rápido para ediciones rutinarias y uno más capaz para trabajo complejo.

El contexto sigue siendo tan importante como la infraestructura. Describir con claridad *qué* quieres crear, *por qué* y *cómo* cambia significativamente el resultado.

## Más opciones para explorar

Has completado el flujo principal. También puedes explorar estas funciones de CLI:

- `/review` para pedir al agente de revisión de código que analice los cambios.
- `/rubber-duck` para comentar un problema y obtener otra perspectiva.
- `/fleet` para coordinar subtareas independientes en paralelo.
- `/worktree` para aislar otra tarea.
- `/delegate` para enviar una tarea a Copilot cloud agent.

## Siguientes pasos

La mejor forma de mejorar con una herramienta es seguir usándola. Úsala en código de producción, proyectos personales o esa pequeña aplicación que llevas años pensando en crear. Comparte lo que aprendas con el equipo y aprende de sus experiencias. Y, como siempre, explora la documentación.

Si quieres ampliar Tailspin Toys desde el terminal, continúa con la serie opcional [Foundry Backer Concierge][foundry]. Para comparar otros entornos, explora los talleres de [VS Code][vscode], la [aplicación GitHub Copilot][app] o [Copilot cloud agent][cloud].

## Recursos

- [Acerca de GitHub Copilot CLI][about-cli]
- [Referencia de comandos de Copilot CLI][cli-reference]
- [Personalizar Copilot CLI][customize-cli]
- [Gestionar pull requests con Copilot CLI][manage-prs]

[previous-lesson]: ../9-cli-power-tools/
[foundry]: ../8-foundry-agent/
[vscode]: ../../vscode/
[app]: ../../app/
[cloud]: ../../cloud/
[about-cli]: https://docs.github.com/copilot/concepts/agents/about-copilot-cli
[cli-reference]: https://docs.github.com/copilot/reference/copilot-cli-reference/cli-command-reference
[customize-cli]: https://docs.github.com/copilot/how-tos/copilot-cli/customize-copilot
[manage-prs]: https://docs.github.com/copilot/how-tos/copilot-cli/use-copilot-cli/manage-pull-requests
