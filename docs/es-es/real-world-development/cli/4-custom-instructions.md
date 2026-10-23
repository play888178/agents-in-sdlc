---
title: "Lección 4 - Orientar Copilot con instrucciones personalizadas"
description: "Explora las instrucciones del repositorio, añade un estándar de documentación y aplícalo al código de filtrado."
authors:
  - geektrainer
lastUpdated: 2026-09-18
---

El contexto es clave al trabajar con IA generativa. Si una tarea debe hacerse de una forma concreta, asegúrate de que Copilot tenga esa orientación. Los [archivos de instrucciones][instruction-files] describen no solo *qué* código quieres, sino también *cómo* debe estructurarse. Ahora que has creado el filtrado, explorarás las instrucciones que usó Copilot, añadirás un estándar de documentación y lo aplicarás al código.

En esta lección:

- explorarás cómo llegan al agente las instrucciones del repositorio y los archivos de instrucciones con ámbito de ruta.
- actualizarás el archivo de instrucciones para que se sigan los estándares de desarrollo.
- observarás el efecto de los archivos de instrucciones en el código.

## Escenario

Tailspin Toys tiene directrices y requisitos para las prácticas de desarrollo:

- Los comentarios deben explicar la intención y las decisiones no evidentes, no repetir el código.
- Las funciones exportadas de `db/` y `src/lib/` deben documentar con TSDoc/JSDoc su propósito, los parámetros y los valores devueltos, incluido el argumento `db` inyectable cuando exista.
- Los componentes Astro reutilizables deben documentar sus contratos `Props`; los comentarios deben mantenerse actualizados cuando cambie el código relacionado.
- Se deben conservar las directrices existentes de formato y lint.

Con los archivos de instrucciones proporcionarás a Copilot el contexto adecuado para que las tareas sigan estas prácticas.

## Archivos de instrucciones

Las instrucciones personalizadas aportan a Copilot contexto y preferencias para que comprenda mejor tu estilo de desarrollo y tus requisitos. Puedes indicar convenciones de código, bibliotecas y los comentarios que quieres incluir. Las instrucciones pueden aplicarse a todo el repositorio o a tipos concretos de archivos.

Hay dos tipos de archivos de instrucciones:

- `.github/copilot-instructions.md`, un único archivo que se envía a Copilot en **todas** las solicitudes del repositorio y que contiene contexto general del proyecto.
- `.github/instructions/*.instructions.md`, archivos que ofrecen directrices para lenguajes, tipos de archivo o tareas concretas.

> [!NOTE]
> La compatibilidad con otros formatos de instrucciones depende del entorno. Consulta la [referencia sobre compatibilidad de instrucciones personalizadas][custom-instructions-support] antes de depender de un formato concreto.

## Explora los archivos de instrucciones del proyecto

El proyecto de ejemplo ya incluye un conjunto de archivos. Revísalos antes de hacer cambios para poder observar su efecto.

1. Vuelve a tu Codespace.
2. En el editor de Codespaces, abre `.github/copilot-instructions.md`.
3. Explora la descripción del proyecto y sus directrices de desarrollo, que se aplican a todas las interacciones con Copilot en este repositorio.
4. Abre la carpeta `.github/instructions` y explora sus archivos, incluidos los de Astro, la capa de datos Drizzle y las pruebas.
5. Abre `.github/instructions/unit-tests.instructions.md` y fíjate en el campo `applyTo`, que indica a qué archivos se aplican las instrucciones.
6. Abre `.github/instructions/drizzle.instructions.md` y revisa sus enlaces a otros archivos de instrucciones y a archivos del proyecto. Así puedes dividir instrucciones extensas en archivos reutilizables y proporcionar ejemplos.

## Actualiza las instrucciones para reflejar las prácticas del equipo

Añade a `copilot-instructions.md` una directriz sobre documentación TSDoc en TypeScript nuevo.

1. Localiza la sección **Code formatting guidance**.
2. Añade este último elemento a la lista:

   ```markdown
   - All new TypeScript should contain TSDocs comments for documentation purposes.
   ```

El archivo se guarda automáticamente.

## Aplica las instrucciones actualizadas

Copilot CLI carga las instrucciones del repositorio al iniciar una conversación. Reanuda la conversación de filtrado para que Copilot recoja la directriz sin perder el contexto de la funcionalidad.

1. Si la conversación de filtrado sigue abierta, introduce `/exit`. Copilot CLI guarda las conversaciones automáticamente.
2. Desde la raíz del repositorio y en la rama `game-filters-cli`, reanuda la conversación guardada:

   ```bash
   copilot --resume
   ```

   Si se te solicita, selecciona la conversación de filtrado y confirma que está activa la rama `game-filters-cli`.
3. Pide a Copilot que aplique las instrucciones:

   ```plaintext
   We just updated our instructions and code guidance. Can you please update the code you generated to match that guidance?
   ```

4. Introduce `/diff` y revisa los archivos TypeScript modificados. Confirma que los comentarios TSDoc describen el código correctamente.

## Resumen y siguientes pasos

Has explorado cómo Copilot CLI recoge el contexto de los archivos de instrucciones y has aplicado un nuevo estándar a la funcionalidad. En concreto:

- exploraste cómo llegan al agente las instrucciones del repositorio y los archivos de instrucciones con ámbito de ruta.
- actualizaste el archivo de instrucciones.
- observaste el efecto de esas instrucciones en el código.

A continuación, [personalizarás y ejecutarás la habilidad reutilizable quality-checks][next-lesson] para aplicar de forma coherente lint y pruebas.

## Recursos

- [Añadir instrucciones personalizadas para Copilot CLI][instruction-files]
- [Compatibilidad con instrucciones personalizadas][custom-instructions-support]
- [Buenas prácticas para crear instrucciones personalizadas][instructions-best-practices]
- [Awesome Copilot: instrucciones y otros recursos][awesome-copilot]

[instruction-files]: https://docs.github.com/copilot/how-tos/copilot-cli/customize-copilot/add-custom-instructions
[instructions-best-practices]: https://docs.github.com/copilot/concepts/prompting/response-customization#writing-effective-custom-instructions
[awesome-copilot]: https://awesome-copilot.github.com/
[custom-instructions-support]: https://docs.github.com/copilot/reference/custom-instructions-support
[previous-lesson]: ../3-agent-modes/
[next-lesson]: ../5-agent-skills/
