---
title: "Lección 3 - Modos de agente: Plan y Autopilot"
description: "Explora los modos de agente: utiliza Plan para acordar un enfoque, Autopilot para crear el filtrado a partir de una incidencia e Interactive para revisar y verificar el resultado."
authors:
  - geektrainer
lastUpdated: 2026-10-22
---

Empezamos por añadir una pequeña funcionalidad al proyecto, pero los cambios más amplios requieren un proceso más sólido. La aplicación GitHub Copilot está diseñada para adaptarse al flujo de trabajo existente de una organización y ayudarte a desarrollar lo adecuado de la forma correcta. Esta es la primera de varias lecciones en las que seguirás un proceso habitual de desarrollo con agentes: partirás de una incidencia para crear una funcionalidad, comprobarás que el código es válido y que la funcionalidad se comporta como se espera y, finalmente, la integrarás correctamente en el proyecto.

> [!NOTE]
> Utilizarás la misma sesión durante todo el flujo de trabajo de la funcionalidad. Lo habitual sería usar sesiones o PR diferentes para los distintos tipos de archivo, pero simplificaremos el proceso para centrarnos en los conceptos principales.

En esta lección:

- iniciarás una sesión de agente desde una incidencia de GitHub.
- definirás los requisitos en modo **Plan**.
- implementarás la funcionalidad en modo **Autopilot**.
- revisarás el código.
- validarás manualmente la funcionalidad en un lienzo de navegador.

Mientras desarrollas esta funcionalidad, actualizarás las instrucciones del repositorio, personalizarás la habilidad `quality-checks`, añadirás validación con el protocolo de contexto de modelo (MCP), crearás un agente de control de calidad (QA) y abrirás la PR de la funcionalidad.

## Escenario

El catálogo de Tailspin Toys está creciendo y sus visitantes necesitan filtrar los juegos por categoría y editorial. La incidencia de la lista de trabajo pendiente describe la funcionalidad, pero hay que acordar detalles —como la combinación de categorías— antes de programar. Utilizarás el modo Plan para resolver esas decisiones y, después, autorizarás una implementación acotada con Autopilot.

## Contexto

La incorporación de agentes de programación con IA al flujo de desarrollo no cambia los principios fundamentales; los hace aún más importantes. La mayoría de los desarrolladores siguen un proceso similar al siguiente:

1. Abrir una incidencia que describa el trabajo pendiente.
2. Crear un plan de lo que se debe desarrollar.
3. Desarrollar y revisar el código.
4. Ejecutar pruebas para validar el código.
5. Validar manualmente la nueva funcionalidad.
6. Crear una solicitud de incorporación de cambios (PR).
7. Integrar el código después de que se haya revisado y se hayan completado correctamente las comprobaciones de integración continua (CI).

> [!NOTE]
> Los detalles concretos varían según el equipo y la organización, pero la mayoría de los procesos siguen una variante del flujo anterior.

Al seguir este enfoque, te aseguras de que el código generado por IA cumpla los requisitos establecidos y pase por el mismo proceso de revisión que el código escrito manualmente.

## Modos de sesión

El **modo de sesión** controla el grado de autonomía del agente. Puedes seleccionarlo en el menú desplegable situado debajo del campo del prompt y cambiarlo en cualquier momento:

- **Interactive**: trabajas junto con el agente. Propone cambios y espera tus indicaciones antes de continuar.
- **Plan**: el agente crea primero un plan. Revisas y apruebas el plan antes de que lo ejecute.
- **Autopilot**: el agente trabaja de forma totalmente autónoma: escribe código, ejecuta pruebas e itera sin esperar indicaciones.

Empieza en modo Plan, revisa el plan y, después, usa Autopilot para implementarlo.

## Iniciar una sesión desde la incidencia

Antes de empezar, confirma que se ha combinado la PR de valoraciones por estrellas y que tu `main` local está actualizado.

1. En **Sessions**, en la barra lateral, selecciona la carpeta del repositorio de Tailspin Toys y, después, **Issues** en la navegación superior del repositorio. Abre **Allow users to filter games by category and publisher**. Esta incidencia ya existe en la plantilla; no tienes que crear otra.
2. Selecciona **New session** y elige un **new working tree** basado en la rama `main` actualizada.

    ![Vista de la incidencia en la aplicación GitHub Copilot con una flecha que señala el botón New session](../../../_images/app-new-session-from-issue.png)

3. Confirma que la incidencia está adjunta a la sesión y selecciona **Plan** en el selector de modo.

> [!NOTE]
> Si no se encuentra Node.js o npm, pide al agente que ejecute `node --version` y `npm --version`. Si funcionan en el terminal, pero no en la sesión, comprueba la configuración de PATH o del gestor de versiones de la aplicación y, después, ciérrala por completo y vuelve a abrirla. Repite la operación y las comprobaciones que fallaron; no reinstales npm solo porque el agente no lo encuentre.

## Planificar la funcionalidad de filtrado

La planificación te permite revisar el enfoque antes de que Copilot escriba código. Como has iniciado la sesión desde la incidencia, Copilot ya tiene la solicitud de la funcionalidad como contexto. Envía:

```plaintext
Build this feature.
```

Responde a las preguntas de Copilot y compara el plan con los criterios de aceptación de la incidencia. Comprueba que contempla el filtrado por categoría y editorial, controles accesibles, cambios en el acceso a datos y pruebas. Aclara cualquier comportamiento poco definido, como la combinación de varias categorías o qué ocurre cuando no hay juegos que coincidan.

El plan debe incluir lint, pruebas unitarias, pruebas de extremo a extremo (E2E) y comprobación de tipos con las herramientas existentes del proyecto. Debe centrarse en implementar y probar el filtrado; crearás la PR después de completar el flujo de calidad. Pide que se modifique el plan antes de aprobarlo y conserva la URL de la incidencia y las aclaraciones acordadas para la validación posterior.

## Aprobar Autopilot explícitamente

Cuando estés conforme con el plan, selecciona **Approve and implement this plan** o la opción equivalente en tu versión. No des por hecho que la aprobación cambia el modo: confirma que el indicador muestra **Autopilot**.

Copilot empezará a implementar la funcionalidad e irá siguiendo el plan, generando código y ejecutando pruebas.

> [!NOTE]
> Al aprobar el plan, la implementación puede comenzar de inmediato, así que revísalo primero. Si Copilot informa de dependencias ausentes o de un conflicto de puerto, resuelve el problema de configuración antes de dar las comprobaciones por completadas. Detén solo los servidores que hayas iniciado.

## Revisar y verificar la implementación

Revisa el código generado antes de combinarlo, igual que harías con cualquier otro código. Después, ejecuta el sitio para comprobar el resultado.

1. Abre **Changes** e inspecciona la implementación del filtrado y las pruebas.
2. Compara el resultado con la incidencia y las aclaraciones aprobadas, incluidas las combinaciones de varias categorías y editoriales. Comprueba que los cambios siguen las instrucciones existentes del repositorio.
3. Revisa los resultados de lint, las pruebas unitarias, las pruebas E2E y la comprobación de tipos. Una comprobación omitida no cuenta como superada.
4. Resuelve los fallos y vuelve a ejecutar las comprobaciones afectadas antes de aceptar la implementación. La configuración E2E de Playwright compila y sirve una vista previa y puede reutilizar un servidor local; asegúrate de que el servidor probado pertenece a este worktree, no a una lección anterior.

## Explorar la nueva funcionalidad

Comprueba en el navegador que la aplicación y el filtrado funcionan como se espera.

1. Usa este prompt para pedir a Copilot que inicie la aplicación y abra la página en el lienzo de navegador:

    ```plaintext
    Start the app and open it in the browser canvas.
    ```

2. Espera a que se inicie la aplicación y se abra una ventana del navegador dentro de la aplicación Copilot.
3. Selecciona una categoría y confirma que los resultados coinciden. Borra el filtro y, después, selecciona una editorial y comprueba sus resultados.
4. Combina filtros de categoría y editorial y compara los resultados con la incidencia y el plan aprobado. Si se admiten varias categorías, comprueba el comportamiento acordado al combinarlas.
5. Prueba una combinación sin juegos coincidentes y comprueba el estado vacío. Borra o restablece los filtros y confirma que vuelve a aparecer la lista completa.
6. Cuando termines, pide a Copilot que detenga el servidor de desarrollo que ha iniciado para esta sesión y cierre el lienzo:

    ```plaintext
    Stop the dev server and close the browser canvas.
    ```

## Mantener la sesión para las siguientes lecciones

No crees ni combines todavía la PR del filtrado. Conserva esta sesión, el worktree y la rama para las lecciones siguientes. A continuación, aplicarás instrucciones personalizadas al código de filtrado y, después, añadirás mejoras a la habilidad y al agente de QA antes de crear y combinar la PR conjunta. Puedes dejar la sesión inactiva tras detener el servidor de desarrollo y volver a ella en la siguiente lección.

## Resumen y pasos siguientes

Has utilizado distintos modos de agente para desarrollar y revisar una funcionalidad. En esta lección:

- has iniciado una sesión de agente desde una incidencia de GitHub.
- has definido los requisitos en modo **Plan**.
- has implementado la funcionalidad en modo **Autopilot**.
- has revisado el código.
- has validado manualmente la funcionalidad en un lienzo de navegador.

A continuación, profundizarás en cómo se genera el código y te asegurarás de que siga las prácticas documentadas mediante el [uso de instrucciones personalizadas][next-lesson].

## Recursos

- [Trabajar con sesiones de agente en la aplicación GitHub Copilot][agent-sessions]

[next-lesson]: ../4-custom-instructions/
[agent-sessions]: https://docs.github.com/copilot/how-tos/github-copilot-app/agent-sessions
