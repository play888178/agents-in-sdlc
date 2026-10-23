---
title: "Ejercicio 1 - Instrucciones personalizadas (VS Code)"
authors:
  - geektrainer
lastUpdated: 2026-06-30
---

[← Lección anterior: Requisitos previos][previous-lesson] · [Siguiente lección: Modo de agente →][next-lesson]

El contexto es clave cuando se trabaja con IA generativa. Los [archivos de instrucciones][instruction-files] permiten compartir con Copilot directrices sobre cómo generar código.

En este ejercicio:

- explorarás cómo llegan a Copilot el contexto del proyecto, las directrices de desarrollo y los estándares de documentación mediante instrucciones del repositorio y archivos con ámbito de ruta.
- generarás el primer bloque de datos del filtrado (un helper de editoriales) con las instrucciones actuales.
- añadirás un estándar general a `.github/copilot-instructions.md`.
- repetirás el mismo prompt y observarás cómo el código generado adopta el nuevo estándar.
- confirmarás y fusionarás las instrucciones y el primer cambio de filtrado en `main` para usar la directriz actualizada en el siguiente ejercicio.

> [!CAUTION]
> El código generado puede diferir de algunos estándares. Copilot no es determinista. El objetivo es observar la tendencia del cambio después de actualizar las instrucciones, no obtener una salida idéntica carácter por carácter.

## Archivos de instrucciones

### Escenario

Tailspin Toys tiene estas directrices de desarrollo:

- La capa de datos siempre debe tener pruebas unitarias.
- La interfaz debe usar el modo oscuro y tener un aspecto moderno.
- El código debe incluir documentación en comentarios TSDoc.
- Cada archivo debe comenzar con un bloque de comentarios que describa su propósito.

Usarás archivos de instrucciones para proporcionar a Copilot el contexto necesario para seguir estas prácticas.

### Instrucciones personalizadas

Las instrucciones personalizadas proporcionan contexto y preferencias para que Copilot comprenda mejor el estilo y los requisitos del proyecto. Pueden definir convenciones de desarrollo, bibliotecas y comentarios preferidos. Se pueden aplicar a todo el repositorio o a determinados tipos de archivo.

- `.github/copilot-instructions.md` es un único archivo que se envía a Copilot en **todas** las solicitudes del repositorio. Contiene información general del proyecto, como su pila tecnológica, una descripción del trabajo, buenas prácticas y directrices globales.
- `.github/instructions/*.instructions.md` contiene directrices para tareas o tipos de archivo concretos, como TypeScript, Astro, componentes de interfaz o pruebas unitarias.

> [!NOTE]
> En el IDE, los archivos de instrucciones se usan para generar código en Copilot Chat, pero no para completar código ni para las sugerencias de siguiente edición.
>
> Copilot Chat, Copilot CLI y el agente en la nube usan las instrucciones del repositorio y los archivos `*.instructions.md` con frontmatter `applyTo` al generar código.
>
> Copilot también [admite archivos de instrucciones de otros estándares][custom-instructions-support], incluidos AGENTS.md y CLAUDE.md.

### Buenas prácticas para gestionar instrucciones

- Limita `copilot-instructions.md` a la orientación general del proyecto, su estructura y los estándares globales.
- Usa `*.instructions.md` para instrucciones sobre tipos de archivo o tareas concretas.
- Escribe directrices claras en lenguaje natural y proporciona ejemplos de código correcto e incorrecto.

No hay una única forma de crear archivos de instrucciones ni de usar IA; la experimentación te ayudará a saber qué funciona mejor.

> [!TIP]
> Un proyecto que usa GitHub Copilot debería tener un conjunto sólido de instrucciones. En este proyecto encontrarás archivos para [actualizaciones de la interfaz][ui-instructions] y [Astro][astro-instructions], entre otros.
>
> Copilot también puede generar instrucciones. Cada entorno lo presenta de forma distinta; por ejemplo, **Configure Chat → Generate Agent Instructions** en VS Code o `/init` en Copilot CLI.
>
> Para buscar plantillas, explora [awesome-copilot][awesome-copilot], un repositorio de archivos de instrucciones, agentes personalizados y otros recursos.

[ui-instructions]: https://github.com/github-samples/tailspin-toys/blob/main/.github/instructions/ui.instructions.md
[astro-instructions]: https://github.com/github-samples/tailspin-toys/blob/main/.github/instructions/astro.instructions.md
[awesome-copilot]: https://github.com/github/awesome-copilot
[custom-instructions-support]: https://docs.github.com/copilot/reference/custom-instructions-support

## Explora los archivos de instrucciones del proyecto

Lee los archivos que incluye el repositorio antes de hacer cambios.

1. Vuelve al codespace y abre `.github/copilot-instructions.md`.
2. Revisa la descripción del proyecto y secciones como **Agent notes**, **Code standards**, **Scripts** y **Repository Structure**. En **Code standards**, fíjate en la guía anidada **GitHub Actions Workflows**.
3. Explora la carpeta `.github/instructions`, que contiene directrices para Astro, Drizzle, pruebas y otras áreas.
4. Abre `.github/instructions/unit-tests.instructions.md` y observa que su campo `applyTo` indica a qué archivos se aplican las instrucciones, como los archivos `**/*.test.ts`.
5. Revisa las instrucciones específicas para las pruebas unitarias.
6. Abre `.github/instructions/drizzle.instructions.md` y revisa sus vínculos a otros archivos de instrucciones y archivos del proyecto. Las rutas son relativas al archivo de instrucciones.

> [!NOTE]
> La sección **Code formatting requirements** de `copilot-instructions.md` describe los estándares, pero aún no exige documentación en el código. Añadirás reglas para comentarios TSDoc y encabezados de archivo.

## Crea una rama

Crea una rama para trabajar en los cambios.

1. Vuelve al codespace del ejercicio anterior.
2. Abre un terminal nuevo con <kbd>Control</kbd>+<kbd>`</kbd> (Mac) o <kbd>Ctrl</kbd>+<kbd>`</kbd> (Windows/Linux).
3. Crea una rama y cámbiate a ella:

   ```bash
   git checkout -b custom-instructions
   ```

## Usa Copilot Chat antes de actualizar las instrucciones

> [!TIP]
> **Abre Copilot Chat**
>
> Vuelve al codespace, abre el panel Copilot Chat y selecciona **New Chat** para empezar una conversación limpia. El modo y el modelo dependen del ejercicio; cada sección especifica lo necesario.

Primero enviarás un prompt con las instrucciones actuales. Después las actualizarás y volverás a enviar el mismo prompt.

1. Cierra las pestañas del editor de los ejercicios anteriores para limitar el contexto.
2. Abre `src/lib/publishers.ts` para mostrar dónde debe estar el helper.
3. En el selector de agentes de la vista Chat, selecciona **Agent**.

   ![Selector de agentes en la vista Chat](../../../_images/shared-chat-mode-selector.png)

4. Envía este prompt:

   ```plaintext
   Create or update src/lib/publishers.ts with a data-access helper that returns a list of all publishers with the name and id for each. Apply the file changes.
   ```

5. Copilot explorará el proyecto y aplicará cambios, posiblemente al helper y sus pruebas.
6. Observa que el helper propuesto es una función tipada que recibe un cliente `db` como primer argumento y devuelve un array tipado de editoriales. Esto procede de las convenciones de `.github/instructions/drizzle.instructions.md`, que se aplica a `src/lib/*.ts`.
7. Comprueba si al código propuesto le faltan comentarios TSDoc y un encabezado de archivo.

> [!CAUTION]
> Copilot podría añadir comentarios de documentación aunque no se lo pidas. No pasa nada: el objetivo es mejorar la coherencia después de actualizar las instrucciones.

## Añade un estándar al repositorio

Añade una directriz general para mejorar las sugerencias de código.

1. Vuelve a abrir `.github/copilot-instructions.md`.
2. Busca **Code formatting requirements**, cerca de la línea 27. Observa que todavía no incluye una regla de documentación en el código.
3. Añade estas líneas Markdown debajo de los estándares existentes:

   ```markdown
   - Every exported function should have a TSDoc comment describing its purpose, parameters, and return value.
   - Before imports or any code, add a comment block to the file that explains its purpose.
   ```

4. Guarda `copilot-instructions.md`.

> [!TIP]
> Las instrucciones globales del proyecto corresponden a `.github/copilot-instructions.md`; usa archivos `*.instructions.md` para lenguajes, tipos de archivo o tareas concretas.

## Repite el prompt y observa el cambio

Repite el prompt para que Copilot actualice el helper con la nueva directriz.

1. En Copilot Chat, selecciona **New Chat** para limpiar el contexto.
2. Vuelve a abrir `src/lib/publishers.ts`.
3. Envía el mismo prompt:

   ```plaintext
   Create or update src/lib/publishers.ts with a data-access helper that returns a list of all publishers with the name and id for each. Apply the file changes.
   ```

4. Comprueba que el archivo propuesto empieza con un bloque de comentarios parecido a este:

   ```typescript
   /**
    * Helpers de acceso a datos de editoriales para la plataforma de financiación colectiva de Tailspin Toys.
    * Proporciona funciones para recuperar información de editoriales de la base de datos.
    */
   ```

5. Comprueba que la función incluye un comentario TSDoc parecido a este:

   ```typescript
   /**
    * Devuelve una lista de todas las editoriales con su id y su nombre.
    *
    * @param db - El cliente de base de datos de Drizzle.
    * @returns Una promesa que se resuelve en un array de objetos de editoriales.
    */
   ```

Copilot ya sigue un nuevo estándar del proyecto y ha aplicado la directriz al código que usarás en el siguiente ejercicio.

## Confirma las instrucciones y envía la rama

Confirma la actualización de instrucciones y los archivos que Copilot haya modificado para el helper.

1. Abre un terminal con <kbd>Control</kbd>+<kbd>`</kbd> (Mac) o <kbd>Ctrl</kbd>+<kbd>`</kbd> (Windows/Linux).
2. Comprueba los cambios:

   ```bash
   git status
   ```

3. Prepara y confirma la actualización y el helper:

   ```bash
   git add .github/copilot-instructions.md src/lib/publishers.ts
   # If git status shows additional supporting updates (for example tests), add those files too.
   git commit -m "Add doc comment standards and publishers helper foundation"
   ```

4. Envía la rama:

   ```bash
   git push -u origin custom-instructions
   ```

## Crea y fusiona una pull request

Con la rama publicada, crearás una pull request vinculada a la incidencia sobre las instrucciones del backlog. Pide a Copilot que busque la incidencia, cree una PR que la cierre y la fusione.

1. Abre Copilot Chat en el codespace.
2. Pulsa <kbd>Control</kbd>+<kbd>Command</kbd>+<kbd>I</kbd> (Mac) o <kbd>Ctrl</kbd>+<kbd>Alt</kbd>+<kbd>I</kbd> (Windows/Linux) para abrir la vista Copilot Chat y confirma que está seleccionado **Agent**.
3. Envía este prompt:

   ```plaintext
   Find the issue related to updating the instructions file. Create a new PR from the current branch, highlight that the PR closes that issue, and include that we also added the publishers helper foundation for the upcoming filtering work.
   ```

4. Cuando Copilot solicite permiso para realizar acciones de GitHub, revisa el comando y selecciona **Allow** si corresponde.
5. Cuando se cree la PR, pídele que la fusione y actualice tu rama local:

   ```plaintext
   Merge the PR into main. Then return to main locally, and pull the latest code so we are up to date.
   ```

6. Revisa las solicitudes de permiso para acciones de GitHub y comandos de shell y selecciona **Allow** cuando corresponda.

Has creado y fusionado una pull request con ayuda de GitHub Copilot.

## Resumen y siguientes pasos

Has explorado cómo Copilot obtiene contexto de los archivos de instrucciones y has usado Copilot Chat en VS Code para:

- enviar un prompt de generación de código con las instrucciones existentes.
- añadir un estándar general a `.github/copilot-instructions.md`.
- repetir el prompt y observar cómo el código adopta el estándar.
- confirmar las instrucciones y el helper en `main` para que el siguiente ejercicio pueda usarlos.

A continuación, usarás esas instrucciones en el [modo de agente][next-lesson] para añadir una funcionalidad al proyecto.

## Recursos

- [Archivos de instrucciones para personalizar GitHub Copilot][instruction-files]
- [Buenas prácticas para crear instrucciones personalizadas][instructions-best-practices]
- [Cinco consejos para mejorar las instrucciones personalizadas de Copilot][copilot-instructions-five-tips]
- [Instrucciones personales para GitHub Copilot][personal-instructions]
- [Awesome Copilot: instrucciones y otros recursos][awesome-copilot]

[previous-lesson]: ../0-prerequisites/
[next-lesson]: ../2-agent-mode/
[instruction-files]: https://code.visualstudio.com/docs/copilot/copilot-customization
[instructions-best-practices]: https://docs.github.com/enterprise-cloud@latest/copilot/using-github-copilot/coding-agent/best-practices-for-using-copilot-to-work-on-tasks#adding-custom-instructions-to-your-repository
[personal-instructions]: https://docs.github.com/copilot/customizing-copilot/adding-personal-custom-instructions-for-github-copilot
[copilot-instructions-five-tips]: https://github.blog/ai-and-ml/github-copilot/5-tips-for-writing-better-custom-instructions-for-copilot/
