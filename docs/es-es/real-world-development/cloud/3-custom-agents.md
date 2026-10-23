---
title: "Ejercicio 3 - Agentes personalizados"
authors:
  - geektrainer
lastUpdated: 2026-06-30
---

## ¿Qué son los agentes personalizados?

Los [agentes personalizados][custom-agents-concept] de GitHub Copilot permiten crear asistentes de IA especializados en tareas o dominios del flujo de desarrollo. Al definirlos mediante archivos Markdown en `.github/agents`, puedes proporcionar a Copilot instrucciones, buenas prácticas, patrones de código y conocimiento específico del dominio. Los equipos pueden convertir su experiencia en agentes reutilizables: por ejemplo, un agente de accesibilidad que compruebe el cumplimiento de las [Pautas de Accesibilidad para el Contenido Web (WCAG)][wcag], uno de seguridad o uno de pruebas.

Los agentes personalizados se definen en archivos Markdown de `.github/agents` o, de forma global, en `~/.copilot/agents`. Cada archivo incluye frontmatter YAML con `name` y `description`, seguido de un prompt Markdown que define el comportamiento, la especialización y las instrucciones del agente.

### Agentes personalizados y habilidades de agente

Los agentes personalizados y las [habilidades de agente][agent-skills-concept] se definen principalmente en Markdown y explican a la IA cómo realizar operaciones. La distinción más sencilla es que el **agente personalizado** realiza el trabajo y las **habilidades** son herramientas.

Los agentes personalizados tienen su propia ventana de contexto y pueden orquestar habilidades e incluso otros agentes. En este laboratorio, el agente de accesibilidad revisa y actualiza el sitio según las directrices de accesibilidad; podría invocar habilidades para gestionar una pull request o ejecutar pruebas.

> [!NOTE]
> No hay una única forma correcta de crear un agente personalizado. Prueba e itera para averiguar qué funciona mejor en cada entorno y escenario.

[custom-agents-concept]: https://docs.github.com/copilot/concepts/agents/cloud-agent/about-custom-agents
[agent-skills-concept]: https://docs.github.com/copilot/concepts/agents/about-agent-skills
[wcag]: https://www.w3.org/WAI/standards-guidelines/wcag/

Con los agentes personalizados explorarás:

- cómo se definen.
- cómo se asigna una tarea a un agente.

## Escenario

Tailspin Toys quiere que su plataforma de financiación colectiva sea accesible para todas las personas, independientemente de sus capacidades visuales o preferencias. Algunos usuarios indican que les cuesta leer el tema oscuro actual por el contraste insuficiente entre el texto y el fondo. El equipo de diseño solicita un modo de alto contraste que se pueda activar y desactivar.

La accesibilidad es prioritaria, así que usarás un agente personalizado para generar esta funcionalidad cuanto antes.

## Revisa el agente personalizado de accesibilidad

Ya existe un agente personalizado para accesibilidad. Revisa su contenido para comprender cómo orienta a Copilot.

1. Vuelve a tu codespace y abre `.github/agents/accessibility.md`.
2. Fíjate en el frontmatter YAML con los campos `name` y `description`.

> [!CAUTION]
> El frontmatter con `name` y `description` es obligatorio para los agentes personalizados.

3. Revisa las secciones siguientes: responsabilidades principales, buenas prácticas, ejemplos de HTML, CSS y JavaScript, y errores habituales.

## Crea y asigna una incidencia

Mission control es el espacio central para trabajar con agentes. Permite asignar tareas al agente en la nube, supervisarlas y dirigirlas. Asignarás una tarea para crear el modo de alto contraste.

1. Abre tu repositorio.
2. Selecciona la pestaña **Issues** y, después, **New issue**.
3. Selecciona **Blank issue**.
4. Escribe `Add high contrast mode to website` en **Title**.
5. En **Description**, introduce:

   ```plaintext
   We need a high contrast mode for the site. There should be a toggle for high contrast which the user can set. It should store the setting in local storage on the browser.
   ```

6. Selecciona **Create**.
7. En el panel derecho, selecciona **Assign to Copilot**.
8. Selecciona **Accessibility agent** en la lista de agentes personalizados y, después, **Assign**.

   ![Captura de la asignación del agente en la nube con el agente personalizado de accesibilidad seleccionado](../../../_images/ex5-select-custom-agent.png)

Copilot empezará a trabajar en segundo plano.

## Resumen y siguientes pasos

Has explorado los [agentes personalizados][custom-agents], asistentes especializados en tareas y dominios concretos. Permiten codificar la experiencia y los estándares del equipo en agentes reutilizables.

En esta lección:

- exploraste cómo se definen los agentes personalizados.
- asignaste una tarea a un agente personalizado.

A continuación, [supervisarás y dirigirás la sesión del agente][next-lesson] desde mission control.

## Recursos

- [Acerca de los agentes personalizados][custom-agents]
- [Preparar el uso de agentes personalizados en una organización][org-custom-agents]
- [Preparar el uso de agentes personalizados en una empresa][enterprise-custom-agents]

---

| [← Lección anterior: Agente de GitHub Copilot en la nube][previous-lesson] | [Siguiente lección: Supervisar y gestionar agentes →][next-lesson] |
|:--|--:|

[previous-lesson]: ../2-cloud-agent/
[next-lesson]: ../4-managing-agents/
[custom-agents]: https://docs.github.com/copilot/concepts/agents/cloud-agent/about-custom-agents
[org-custom-agents]: https://docs.github.com/copilot/how-tos/administer-copilot/manage-for-organization/prepare-for-custom-agents
[enterprise-custom-agents]: https://docs.github.com/copilot/how-tos/administer-copilot/manage-for-enterprise/manage-agents/prepare-for-custom-agents
