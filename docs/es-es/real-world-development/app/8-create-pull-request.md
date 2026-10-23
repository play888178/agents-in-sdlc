---
title: "Lección 8 - Crear y combinar la PR de la funcionalidad"
description: "Revisa conjuntamente el filtrado, las instrucciones, la actualización de la habilidad, el perfil de QA y las pruebas; después, crea una PR y utiliza Agent Merge."
authors:
  - geektrainer
lastUpdated: 2026-10-22
---

La implementación del filtrado, las actualizaciones de las instrucciones y de la habilidad, el perfil de control de calidad (QA) y las pruebas están guardados en una sola rama. Es el momento de revisarlos en conjunto y abrir una solicitud de incorporación de cambios. Ya combinaste personalmente la solicitud de incorporación de cambios (PR) de valoraciones por estrellas; esta vez dejarás que **Agent Merge** gestione el proceso.

> [!NOTE]
> Normalmente separaríamos la funcionalidad, las actualizaciones de las instrucciones, la actualización de la habilidad y el agente de QA en varias PR. Para agilizar el taller, has mantenido todo el flujo de filtrado y calidad en una sesión y una rama, por lo que todo ese trabajo irá en esta PR.

En esta lección:

- aprenderás qué es Agent Merge y cómo automatiza el ciclo de vida de la combinación.
- inspeccionarás la PR completa de la funcionalidad y las pruebas de verificación.
- autorizarás Agent Merge solo después de revisar el trabajo y confirmarás que la PR se ha combinado.

## Escenario

A lo largo del flujo de trabajo del filtrado, has utilizado Copilot para planificar, implementar y verificar una funcionalidad. Tailspin Toys quiere ahora automatizar el trabajo restante de la PR y mantener la autorización para combinar bajo el control del desarrollador.

## Introducción a Agent Merge

**Agent Merge** automatiza el trabajo restante para incorporar una solicitud de incorporación de cambios mediante la aplicación GitHub Copilot. Al habilitarlo, la sesión de la aplicación lee la PR y atiende los obstáculos: corrige comprobaciones de integración continua (CI) fallidas, responde a comentarios de revisión y reorganiza la base cuando es necesario. Después combina la PR en cuanto GitHub lo permite. Se ejecuta en segundo plano, continúa tras reiniciar la aplicación y se desactiva cuando la PR se ha combinado.

Hasta ahora, has seleccionado **Merge pull request** personalmente. Agent Merge puede asumir esa responsabilidad, pero para editar código y combinar sigue necesitando tu autorización explícita. Revisa las acciones permitidas y el trabajo antes de conceder permiso para combinar.

## Utilizar Agent Merge para gestionar la PR

Con todo el código creado y revisado, puedes permitir que Agent Merge gestione el proceso de la PR.

1. Usa el selector de agentes para elegir **Default agent**.
2. Selecciona el menú desplegable junto a **Create PR**.
3. Selecciona **Agent Merge**. El botón cambia a **Agent Merge**.
4. Selecciona el botón **Agent Merge** para iniciar el proceso. Elegir la opción del menú desplegable no basta para iniciarlo.

Se iniciará el proceso de Agent Merge. Este hará lo siguiente:

- crear la solicitud de incorporación de cambios con un título y una descripción.
- si has iniciado la sesión desde una incidencia, incluir una referencia a ella en el cuerpo de la descripción.
- reorganizar la base o gestionar posibles conflictos de combinación con la rama de destino.
- supervisar el proceso de CI para comprobar que se superan todas las verificaciones.
- supervisar la PR para detectar comentarios de otros desarrolladores o de la revisión de código de Copilot y realizar cambios para resolverlos.
- combinar automáticamente la PR cuando todo se haya completado correctamente, si así se configura.

Ahora permitirás que Agent Merge también combine la PR cuando todo se haya completado correctamente.

5. Revisa las diferencias de la PR y las pruebas de verificación antes de conceder permiso para combinar.
6. Selecciona el menú desplegable junto a **Agent Merge**.
7. Comprueba que **Merge pull request** está marcado.
8. Pide explícitamente al agente que combine la PR:

    ```plaintext
    Merge the PR once all required checks and approvals pass. Do not bypass repository protections.
    ```

9. Sigue el progreso y resuelve los bloqueos que se notifiquen. Antes de continuar, confirma que el estado de la PR es **Merged**; tener habilitado el permiso para combinar o un agente en ejecución no confirma que la combinación se haya completado.

> [!IMPORTANT]
> Agent Merge no elude las protecciones del repositorio ni los permisos ausentes. Resuelve esos bloqueos antes de continuar.

## Resumen y pasos siguientes

Has automatizado varias partes del proceso de desarrollo, como generar, probar y validar código y, ahora, gestionar el proceso de la PR. En concreto:

- has aprendido qué es Agent Merge y cómo automatiza el ciclo de vida de la combinación.
- has inspeccionado la PR completa de la funcionalidad y las pruebas de verificación.
- has autorizado Agent Merge solo después de revisar el trabajo y has confirmado que la PR se ha combinado.

A continuación, [utilizarás un lienzo existente y crearás uno para la clasificación][next-lesson] a fin de explorar una forma más completa de examinar, planificar y visualizar el trabajo con el agente.

## Recursos

- [Gestionar incidencias y solicitudes de incorporación de cambios con la aplicación GitHub Copilot][managing-issues-prs]
- [Acerca de la aplicación GitHub Copilot][about-copilot-app]

[next-lesson]: ../9-canvases/
[managing-issues-prs]: https://docs.github.com/copilot/how-tos/github-copilot-app/managing-issues-and-pull-requests
[about-copilot-app]: https://docs.github.com/copilot/concepts/agents/github-copilot-app
