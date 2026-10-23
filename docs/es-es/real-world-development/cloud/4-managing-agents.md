---
title: "Ejercicio 4 - Supervisar y gestionar agentes"
authors:
  - geektrainer
lastUpdated: 2026-06-30
---

En los últimos ejercicios, has pedido al agente de Copilot en la nube que realice tres tareas para mejorar la experiencia de usuario y añadir funcionalidades. Aunque el agente trabaja de forma autónoma y asíncrona, sigue siendo importante supervisar estas tareas.

Puedes gestionarlas con varias herramientas, incluida [la página de agentes][agents-page] de GitHub.com. Desde este centro de control, consulta las tareas con pull requests abiertas, examina las operaciones y orienta una sesión en curso.

En esta lección:

- explorarás la página de agentes para supervisar tareas del agente en la nube.
- dirigirás una sesión en curso para pedir funcionalidad adicional.

## Escenario

Después de asignar la creación del modo de alto contraste, el equipo decide añadir también un modo claro. Como el agente ya está actualizando el estilo y la funcionalidad de selección de modo, pedirás que incorpore ese cambio durante el trabajo.

## Revisa las tareas del agente en la nube

Comprueba el estado de las tareas asignadas al agente de Copilot en la nube.

1. Abre la [página de agentes de Copilot](https://github.com/copilot/agents).
2. Revisa las tareas que aparecen en los paneles principal y lateral, como la documentación, las API para modificar juegos y el modo de alto contraste.
3. Selecciona una tarea en curso y revisa las operaciones realizadas, que pueden incluir la clonación del código, la preparación del entorno, la configuración de servidores MCP y otros pasos.

> [!NOTE]
> Los pasos concretos varían según el estado del trabajo y el enfoque del agente.

4. Revisa también el panel de pull requests situado a la derecha para consultar la PR y los archivos modificados.

## Dirige el agente en la nube

Pide al agente que añada el modo claro mientras trabaja en el modo de alto contraste.

1. Selecciona la sesión que corresponde al modo de alto contraste. El título depende del nombre que haya asignado Copilot y del estado de la tarea.

   ![Sesión de accesibilidad en mission control](../../../_images/ex6-accessibility-session.png)

2. Observa la sesión durante unos minutos, hasta que indique que ha terminado la configuración y ha empezado el trabajo.
3. En el diálogo **Steer active session while Copilot is working**, introduce:

   ```plaintext
   While we are working on a high contrast mode, let's also add a light mode. There should be a switch for this mode as well where users can select their desired display mode.
   ```

   ![Captura de la tarea del agente en la página de agentes con el diálogo para dirigir una sesión activa resaltado](../../../_images/ex6-steer-cloud-agent-task.png)

4. Pulsa <kbd>Enter</kbd> para enviar el prompt.
5. Comprueba que Copilot confirma el prompt y lo incorpora al flujo de trabajo.

## Deja que Copilot continúe

Copilot incorporará la solicitud después de terminar el paso que estaba realizando cuando enviaste el mensaje. El trabajo puede tardar varios minutos; aprovecha para repasar lo que has aprendido.

## Resumen y siguientes pasos

Has explorado la página de agentes de Copilot, el centro para supervisar y orientar las tareas del agente en la nube. Desde aquí puedes seguir las tareas activas y completadas, revisar el trabajo y redirigir sesiones en curso.

En esta lección:

- exploraste mission control y la página de agentes para supervisar tareas.
- dirigiste una sesión en curso para solicitar más funcionalidad.

A continuación, [iterarás sobre las pull requests del agente][next-lesson].

## Recursos

- [Página de agentes de Copilot][agents-page]
- [Acerca de los agentes personalizados][custom-agents]

---

| [← Lección anterior: Agentes personalizados][previous-lesson] | [Siguiente lección: Iterar sobre el trabajo de Copilot →][next-lesson] |
|:--|--:|

[previous-lesson]: ../3-custom-agents/
[next-lesson]: ../5-iterating/
[agents-page]: https://github.blog/changelog/2025-07-03-agents-page-for-copilot-coding-agent-in-public-preview
[custom-agents]: https://docs.github.com/copilot/concepts/agents/cloud-agent/about-custom-agents
