---
title: "Lección 5 - Personalizar y usar la habilidad quality-checks"
description: "Explora la habilidad quality-checks existente, personaliza el formato del informe y úsala para validar el filtrado."
authors:
  - geektrainer
lastUpdated: 2026-09-29
---

Escribir código no lo es todo. Ya hemos comprobado manualmente que funciona y usado archivos de instrucciones para garantizar que siga los estándares. Pero ¿qué pasa con las pruebas, el lint y las demás partes de la integración continua (CI)?

Las **habilidades de agente** son adecuadas para estas tareas: ayudan a Copilot a ejecutar operaciones de forma coherente.

En esta lección:

- explorarás la habilidad existente `quality-checks`.
- personalizarás el formato de los resultados.
- volverás a cargar y ejecutarás la habilidad.

## Escenario

Tailspin Toys usa la habilidad `quality-checks` para ejecutar pruebas unitarias, lint y comprobaciones de tipos. El equipo quiere mejorar el informe para que los resultados sean más fáciles de leer.

## Instrucciones, scripts y recursos

Las habilidades de agente reúnen instrucciones de tareas reutilizables, scripts ejecutables y recursos de apoyo que el agente carga cuando los necesita. En esencia, son carpetas con un archivo Markdown llamado `SKILL.md`. El archivo contiene frontmatter con un nombre y una descripción, una explicación de la habilidad y pautas para saber cuándo debe usarse. También puede incluir subcarpetas con scripts y otros recursos.

> [!NOTE]
> Una habilidad no necesita carpetas ni archivos adicionales. La habilidad `quality-checks` de Tailspin Toys solo contiene `SKILL.md` porque usa los comandos existentes del proyecto.

Las habilidades pueden estar en `.github/skills` para que el equipo las comparta y reutilice, o en la carpeta de habilidades del usuario `~/.copilot/skills`.

## Explora la habilidad

Revisarás la habilidad `quality-checks` que creó el equipo de Tailspin Toys para ejecutar pruebas unitarias, lint y comprobaciones de tipos.

1. Vuelve a tu Codespace. En el editor de Codespaces, abre `.github/skills/quality-checks/SKILL.md`.
2. Lee `name` y `description` al principio. La descripción ayuda a Copilot a decidir cuándo usar la habilidad.
3. Lee las instrucciones y observa cómo orientan a Copilot durante las pruebas y el lint.
4. Comprueba que la habilidad aún no tiene una sección **Results output formatting**.

## Ejecuta la habilidad antes de cambiarla

Las habilidades se pueden invocar directamente desde Copilot CLI o mediante lenguaje natural. Pide a Copilot que ejecute las tres comprobaciones.

1. Vuelve a la conversación de filtrado en modo Interactive.
2. Envía este prompt:

   ```plaintext
   Run the quality-checks skill for unit tests, lint, and type checks.
   ```

3. Lee el informe final.

## Personaliza el informe

Mejora el informe para que indique qué se ejecutó, si se completó correctamente y qué resultados devolvieron las herramientas.

1. Vuelve a abrir `.github/skills/quality-checks/SKILL.md`.
2. Añade esta sección al final:

   ```markdown
   ## Results output formatting

   Upon completion, report each command that ran and whether it passed, failed, or was blocked. Include test counts, durations, errors, warnings, and other metrics only when the tool reports them. Identify the next action for any failure or blocker, and never describe a skipped or incomplete check as passed.
   ```

3. Guarda el archivo.

## Ejecuta la habilidad actualizada

Copilot CLI puede volver a cargar las habilidades editadas sin reiniciar la conversación.

1. Introduce:

   ```plaintext
   /skills reload
   ```

2. Envía el mismo prompt:

   ```plaintext
   Run the quality-checks skill for unit tests, lint, and type checks.
   ```

3. Lee el informe final y compáralo con el primero.

## Resumen y siguientes pasos

Has personalizado y usado una habilidad de agente existente. En esta lección:

- exploraste la habilidad `quality-checks` para ejecutar pruebas unitarias, lint y comprobaciones de tipos.
- personalizaste el formato de los resultados.
- volviste a cargar y ejecutaste la habilidad.

Este cambio acompañará al filtrado en la PR de funcionalidad. A continuación, permitirás que Copilot interactúe directamente con el sitio mediante el [servidor MCP de Playwright][next-lesson].

## Más ejemplos de habilidades

Estos ejemplos de la comunidad son referencias, no tareas adicionales:

- [Especificación de Agent Skills][skill-spec]
- [Flujo de contribución: `make-repo-contribution`][contribution-example]
- [Documentos de requisitos: `prd`][prd-example]
- [Diagramas y script de exportación: `drawio`][drawio-example]
- [Pruebas del navegador: `webapp-testing`][browser-example]

[previous-lesson]: ../4-custom-instructions/
[next-lesson]: ../6-mcp-playwright/
[skill-spec]: https://agentskills.io/specification
[contribution-example]: https://github.com/github/awesome-copilot/tree/main/skills/make-repo-contribution
[prd-example]: https://github.com/github/awesome-copilot/tree/main/skills/prd
[drawio-example]: https://github.com/github/awesome-copilot/tree/main/skills/drawio
[browser-example]: https://github.com/github/awesome-copilot/tree/main/skills/webapp-testing
