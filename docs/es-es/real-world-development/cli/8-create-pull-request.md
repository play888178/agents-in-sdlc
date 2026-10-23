---
title: "Lección 8 - Crear y fusionar la PR de funcionalidad"
description: "Revisa el filtrado, las instrucciones, la habilidad, el perfil de control de calidad y las pruebas; después, crea la PR y usa Agent Merge."
authors:
  - geektrainer
lastUpdated: 2026-09-18
---

La implementación del filtrado, las actualizaciones de instrucciones y de la habilidad, el perfil de control de calidad (QA) y las pruebas están guardados en una rama. Ahora revisarás el conjunto y abrirás una pull request. Fusionaste manualmente la PR de las valoraciones; esta vez dejarás que **Agent Merge** gestione el proceso.

> [!NOTE]
> En condiciones normales, dividiríamos la funcionalidad, las instrucciones, la habilidad y el agente de control de calidad en varias PR. Para simplificar el taller, has mantenido todo el trabajo de filtrado y calidad en una conversación y una rama.

En esta lección:

- conocerás Agent Merge y cómo automatiza el ciclo de vida de una fusión.
- inspeccionarás todos los cambios y las pruebas de verificación.
- crearás la PR de filtrado.
- habilitarás Agent Merge solo después de la revisión y confirmarás la fusión.

## Escenario

Has usado Copilot para planificar, implementar y verificar una funcionalidad. Tailspin Toys quiere automatizar el trabajo restante de la PR, pero mantener bajo el control del desarrollador la autorización para fusionarla.

## Presentación de Agent Merge

**Agent Merge** automatiza el trabajo restante para completar una pull request. Cuando lo habilitas, Copilot resuelve los bloqueos de la PR —por ejemplo, corrige comprobaciones de integración continua (CI) fallidas, responde a comentarios de revisión y actualiza la rama mediante rebase cuando hace falta— y habilita la fusión automática de GitHub cuando el repositorio lo permite.

Hasta ahora has seleccionado **Merge pull request** manualmente. Agent Merge puede encargarse de ese trabajo. Revisa los cambios y confirma que están listos antes de habilitarlo.

## Usa Agent Merge para gestionar la PR

Revisa el código, crea la PR y deja que Agent Merge gestione el resto.

1. Vuelve a tu Codespace.
2. Abre el diálogo de agentes con `/agent`.
3. Selecciona **Default** y pulsa <kbd>Enter</kbd>.
4. Crea una PR con `/pr create`.
5. Activa Agent Merge con `/pr agentmerge`.
6. Copilot supervisará el proceso de integración continua en la PR y, cuando todo se complete correctamente, la fusionará.
7. Comprueba que Copilot muestra un mensaje parecido a «PR #14 was squash-merged successfully».

> [!IMPORTANT]
> Agent Merge no evita las aprobaciones obligatorias, la protección de ramas, las colas de fusión, la configuración del repositorio ni la falta de permisos. Si se bloquea, lee el motivo y completa manualmente la fusión revisada cuando el repositorio lo permita.

## Resumen y siguientes pasos

Has automatizado varias partes del desarrollo: la generación de código, las pruebas y la validación, y ahora el proceso de la pull request. En esta lección:

- conociste Agent Merge y cómo automatiza el ciclo de vida de una fusión.
- inspeccionaste todos los cambios y las pruebas de verificación.
- creaste la PR de filtrado.
- habilitaste Agent Merge después de revisar los cambios y confirmaste la fusión.

A continuación, [explorarás otros comandos de barra útiles de Copilot CLI][next-lesson] para trabajar con el contexto, los modelos, el uso compartido y la delegación opcional en la nube.

## Recursos

- [Gestionar pull requests con Copilot CLI][manage-prs]
- [Referencia de comandos de Copilot CLI][cli-reference]

[previous-lesson]: ../7-qa-agent/
[next-lesson]: ../9-cli-power-tools/
[manage-prs]: https://docs.github.com/copilot/how-tos/copilot-cli/use-copilot-cli/manage-pull-requests
[cli-reference]: https://docs.github.com/copilot/reference/copilot-cli-reference/cli-command-reference
