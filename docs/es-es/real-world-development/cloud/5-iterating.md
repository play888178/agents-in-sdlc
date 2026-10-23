---
title: "Ejercicio 5: Iterar sobre el trabajo de GitHub Copilot"
authors:
  - geektrainer
lastUpdated: 2026-06-30
---

| [← Lección anterior: Gestionar agentes][previous-lesson] |
|:--|

## Revisa el trabajo

Durante este laboratorio has trabajado con GitHub Copilot en varias tareas para mejorar la experiencia de usuario y añadir funcionalidad: documentación en el código, juegos relacionados para el equipo de diseño y funciones de accesibilidad, incluidos los modos de alto contraste y claro. Revisa los cambios y, si hace falta, envía comentarios para que Copilot mejore el resultado.

### Escenario

Los fundamentos del diseño de software y DevOps no cambian con la IA generativa. Hay que revisar el código generado y seguir el proceso habitual. Revisarás las propuestas de Copilot para la documentación, los juegos relacionados y las funciones de accesibilidad antes de activar la revisión para el resto del equipo.

## Seguridad y agente de Copilot en la nube

Como el agente trabaja de forma asíncrona y sin supervisión, se aplican ciertas medidas de seguridad:

- Copilot solo tiene acceso de lectura al repositorio y acceso de escritura **únicamente** a la rama que usa para el código.
- El agente trabaja en GitHub Actions, en un entorno independiente y efímero.
- Una persona debe aprobar los flujos de trabajo de GitHub Actions antes de que se ejecuten.
- [El acceso a recursos externos está limitado de forma predeterminada](https://docs.github.com/copilot/how-tos/copilot-on-github/customize-copilot/customize-cloud-agent/customize-the-agent-firewall), incluidos los servidores MCP.

## Revisa la documentación generada

Empieza por la primera pull request (PR) generada por el agente para añadir documentación al código. Usa la interfaz habitual de PR de GitHub.com.

> [!NOTE]
> Puede que la PR muestre un aviso de que el firewall bloquea a GitHub Copilot. Es lo esperado: el acceso a recursos externos, incluidos servidores MCP, está limitado de forma predeterminada. Si quieres, puedes [personalizar o desactivar el firewall del agente][agent-firewall].

1. Abre el repositorio en una pestaña nueva de github.com.
2. Selecciona **Pull Requests** y abre la PR titulada, por ejemplo, **Add missing documentation**.

> [!NOTE]
> Si Copilot aún está trabajando, la PR incluirá la marca **[WIP]**. Espera a que termine; puede tardar unos minutos.

3. Cuando la PR esté lista, selecciona **Files changed** y revisa los cambios.

   ![Pestaña Files changed](../../../_images/shared-pr-files-changed.png)

4. Explora el código actualizado. Busca comentarios TSDoc, convenciones de TypeScript y encabezados de comentario procedentes de los archivos de instrucciones que revisaste al principio del ejercicio 2.
5. Cuando termines, vuelve a **Conversation** y desplázate hasta el final.
6. Si hay flujos de trabajo pendientes de aprobación, selecciona **Approve and run workflows**.

   ![Aprobar y ejecutar flujos de trabajo](../../../_images/shared-approve-workflows.png)

7. Comprueba en la sección de verificaciones de la PR que se ejecutan los flujos de trabajo y que las comprobaciones del proyecto de la aplicación Astro finalizan correctamente. Puede tardar unos minutos.

## Pide cambios a GitHub Copilot

Puedes mencionar a Copilot en comentarios de la PR o en comentarios insertados en el código. Copilot leerá esos comentarios e iniciará otra sesión para atenderlos. Como el resultado no es determinista, no hay un prompt único. Por ejemplo, puedes pedir que añada encabezados de comentario, documentación TSDoc a TypeScript y Astro o un README sobre la estructura de la aplicación Astro.

1. Añade un comentario que solicite cambios en la documentación generada y menciona a **@copilot**. Usa una de las ideas anteriores u otra mejora que quieras ver.
2. Selecciona **View Session** para observar el trabajo. Copilot iniciará una sesión nueva.
3. Selecciona **Back to pull request** para volver a la PR.

   ![Volver a la pull request](../../../_images/ex7-back-to-pr.png)

4. Cuando Copilot termine, comprueba que haya un commit nuevo en la PR.
5. Selecciona **Files changed** y revisa los cambios.

Puedes seguir iterando hasta que estés conforme. Después, puedes marcar la PR como lista para revisión y fusionarla en la rama principal.

![Marcar la PR como lista para revisión](../../../_images/ex7-ready-for-review.png)

## Revisa la funcionalidad de juegos relacionados

Vuelve a la PR que Copilot creó para mostrar juegos relacionados en la página de detalles.

1. Abre el repositorio en GitHub.com y selecciona **Pull Requests**.
2. Abre la PR titulada, por ejemplo, **Show related games on the game details page**.
3. Selecciona **Files changed** y revisa el código.
4. Vuelve a **Conversation** y busca si hay flujos de trabajo pendientes de aprobación. Si los hay, selecciona **Approve and run workflows**.
5. Comprueba que las verificaciones del proyecto finalicen correctamente. Puede tardar unos minutos.
6. **Opcional:** cambia a la rama en tu Codespace para probar manualmente la función. En el terminal, sustituye `<branch-name>` por el nombre de la rama creada por Copilot, por ejemplo, **copilot/fix-8**:

   ```bash
   git fetch origin
   git checkout <branch-name>
   ```

Copilot ya ha creado la función de juegos relacionados. Puedes solicitar cambios iterativos mediante comentarios en **Conversation**, por ejemplo, ajustar el número de juegos o añadir encabezados y comentarios TSDoc. Recuerda que esta tarea se asignó antes de actualizar las instrucciones personalizadas.

## Revisa las funciones de accesibilidad

Por último, revisa la PR del agente personalizado de accesibilidad. Debe incluir el modo de alto contraste asignado en el ejercicio 3 y el modo claro solicitado en mission control en el ejercicio 4.

1. Abre el repositorio en GitHub.com y selecciona **Pull Requests**.
2. Abre la PR titulada, por ejemplo, **Add high contrast mode to website**. Si aún está en curso, espera a que deje de tener la marca **[WIP]**.
3. Selecciona **Files changed** y revisa la implementación, especialmente:
   - los controles de interfaz para cambiar de modo;
   - el uso de almacenamiento local para conservar las preferencias;
   - los cambios de CSS o estilo para los modos de alto contraste y claro;
   - los atributos de accesibilidad, la navegación con teclado y las etiquetas ARIA;
   - el código JavaScript o TypeScript que gestiona el cambio de modo.
4. Vuelve a **Conversation** y, si hay flujos de trabajo pendientes de aprobación, selecciona **Approve and run workflows**.
5. Comprueba que las verificaciones de la PR se completan correctamente.
6. **Opcional:** cambia a la rama de Copilot en tu Codespace y ejecuta:

   ```bash
   git fetch origin
   git checkout <branch-name>
   ```

   Inicia la aplicación y prueba en el navegador los modos de alto contraste y claro. Comprueba que funcionan y que las preferencias se conservan después de recargar la página.

El agente personalizado de accesibilidad ha orientado a Copilot para implementar estas funciones siguiendo buenas prácticas. Si detectas problemas o mejoras posibles, menciona a **@copilot** en un comentario para solicitar cambios.

## Ejercicio opcional: sigue delegando

El agente en la nube es útil para delegar tareas reales del backlog y continuar con otro trabajo. Para practicar, crea más incidencias y asígnalas a Copilot. Algunas ideas:

- Crear un formulario de interés para patrocinadores en la página de detalles del juego.
- Implementar la paginación de la lista de juegos.
- Añadir validación de entradas y gestión de errores a los helpers de acceso a datos.

## Resumen

Has completado el entorno de agente en la nube. En estas lecciones:

- **Inspeccionaste los archivos de instrucciones personalizados** del repositorio para observar su efecto en el agente.
- **Asignaste incidencias al agente de Copilot en la nube** y observaste cómo preparaba el entorno, planificaba y trabajaba de forma asíncrona.
- **Creaste y usaste un agente personalizado** para accesibilidad y añadiste modos de alto contraste y claro.
- **Usaste la página de agentes de Copilot como centro de control** para supervisar y dirigir la sesión de accesibilidad.
- **Revisaste e iteraste sobre las pull requests del agente**, mencionando a `@copilot` y aprobando flujos de trabajo.

## Revisión y siguientes pasos

Has completado el entorno de agente en la nube. Si quieres seguir explorando, los demás entornos complementan lo que has practicado:

- 🖥️ **[Entorno de VS Code](../../vscode/)**: explora el modo de agente de Copilot Chat y la integración MCP desde el IDE.
- 💻 **[Entorno de CLI](../../cli/)**: entrega dos cambios revisados desde el terminal con Plan y Autopilot, habilidades, un agente de control de calidad, Playwright MCP, Agent Merge y el puente opcional `/delegate` al agente en la nube.

En tu propio repositorio, puedes asignar al agente tareas de refactorización o cobertura de pruebas, crear agentes personalizados para otros dominios o configurar `copilot-setup-steps.yml` para otra pila tecnológica.

## Recursos

- [GitHub Copilot][github-copilot]
- [Acerca de los agentes de Copilot][copilot-agents]
- [Asignar incidencias de GitHub a Copilot][assign-issue]
- [Buenas prácticas del flujo de configuración del agente][cloud-agent-best-practices]
- [Configurar el firewall del agente de Copilot en la nube][agent-firewall]

---

| [← Lección anterior: Gestionar agentes][previous-lesson] |
|:--|

[github-copilot]: https://github.com/features/copilot
[cloud-agent-overview]: https://docs.github.com/copilot/concepts/agents/cloud-agent/about-cloud-agent
[assign-issue]: https://docs.github.com/copilot/how-tos/use-copilot-agents/cloud-agent/start-copilot-sessions
[setup-workflow]: https://docs.github.com/copilot/how-tos/copilot-on-github/customize-copilot/customize-cloud-agent/customize-the-agent-environment
[copilot-agents]: https://docs.github.com/copilot/concepts/agents/cloud-agent/about-cloud-agent
[cloud-agent-best-practices]: https://docs.github.com/copilot/how-tos/copilot-on-github/customize-copilot/customize-cloud-agent/customize-the-agent-environment
[agent-firewall]: https://docs.github.com/copilot/how-tos/copilot-on-github/customize-copilot/customize-cloud-agent/customize-the-agent-firewall
[previous-lesson]: ../4-managing-agents/
