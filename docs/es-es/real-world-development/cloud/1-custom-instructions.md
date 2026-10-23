---
title: "Ejercicio 1 - Instrucciones personalizadas (agente en la nube)"
authors:
  - geektrainer
lastUpdated: 2026-06-30
---

[← Lección anterior: Requisitos previos][previous-lesson] · [Siguiente lección: Agente de Copilot en la nube →][next-lesson]

El contexto es esencial al trabajar con IA generativa. Si una tarea debe hacerse de una manera concreta o Copilot necesita información de fondo, asegúrate de que pueda acceder a ese contexto. Los [archivos de instrucciones][instruction-files] proporcionan esa orientación para que Copilot entienda no solo *qué* debe hacer, sino también *cómo*.

En este ejercicio:

- explorarás cómo llegan a Copilot el contexto del proyecto, las directrices de desarrollo y los estándares de documentación mediante instrucciones personalizadas del repositorio y archivos con ámbito de ruta.
- añadirás un estándar general del repositorio a `.github/copilot-instructions.md`.

> [!NOTE]
> A diferencia de los entornos de VS Code y CLI, aquí no ejecutarás un prompt comparativo «antes y después»: el agente de Copilot en la nube trabaja de forma asíncrona en incidencias de GitHub, así que es más difícil mostrar el efecto en tiempo real. Más adelante observarás la influencia del archivo de instrucciones al revisar las pull requests del agente.

## Archivos de instrucciones

### Escenario

Tailspin Toys tiene directrices y requisitos de desarrollo:

- La capa de datos siempre debe tener pruebas unitarias.
- La interfaz debe usar el modo oscuro y tener un aspecto moderno.
- El código debe incluir documentación en comentarios TSDoc.
- Cada archivo debe comenzar con un bloque de comentarios que describa su propósito.

Los archivos de instrucciones ayudan a Copilot a realizar tareas de acuerdo con estas prácticas.

### Instrucciones personalizadas

Las instrucciones personalizadas proporcionan contexto y preferencias a Copilot para que comprenda mejor tu estilo de desarrollo y tus requisitos. Puedes especificar convenciones, bibliotecas y comentarios preferidos. Las instrucciones pueden aplicarse a todo el repositorio o a tipos de archivo concretos.

- `.github/copilot-instructions.md` es un único archivo enviado a Copilot en **todas** las solicitudes del repositorio. Debe contener información general del proyecto: tecnologías, descripción, buenas prácticas y directrices globales.
- `.github/instructions/*.instructions.md` contiene directrices para tareas o tipos de archivo concretos, como TypeScript, Astro, componentes de interfaz o pruebas unitarias.

> [!NOTE]
> En el IDE, los archivos de instrucciones se usan para generar código en Copilot Chat, pero no para completar código ni para las sugerencias de siguiente edición.
>
> Copilot Chat, Copilot CLI y el agente en la nube usan instrucciones del repositorio y archivos `*.instructions.md` con frontmatter `applyTo` al generar código.
>
> Copilot también [admite archivos de instrucciones de otros estándares][custom-instructions-support], incluidos AGENTS.md y CLAUDE.md.

### Buenas prácticas para gestionar instrucciones

Los ejemplos del proyecto muestran un enfoque representativo:

- Limita `copilot-instructions.md` a la orientación general del proyecto, su estructura y los estándares globales.
- Usa `*.instructions.md` para instrucciones sobre tipos de archivo o tareas concretas.
- Escribe directrices claras en lenguaje natural y añade ejemplos de código correcto e incorrecto.

No hay una única forma de crear archivos de instrucciones ni de usar IA. La experimentación te ayudará a determinar qué funciona mejor para el proyecto.

> [!TIP]
> Los proyectos que usan GitHub Copilot deberían contar con un conjunto sólido de archivos de instrucciones. En este proyecto hay instrucciones para [actualizaciones de la interfaz][ui-instructions], [Astro][astro-instructions] y otras tareas.
>
> Copilot también puede generar archivos de instrucciones. Cada entorno lo presenta de forma distinta, por ejemplo **Configure Chat → Generate Agent Instructions** en VS Code o `/init` en Copilot CLI.
>
> Para buscar plantillas, explora [awesome-copilot][awesome-copilot], un repositorio de instrucciones, agentes personalizados y otros recursos.

[ui-instructions]: https://github.com/github-samples/tailspin-toys/blob/main/.github/instructions/ui.instructions.md
[astro-instructions]: https://github.com/github-samples/tailspin-toys/blob/main/.github/instructions/astro.instructions.md
[awesome-copilot]: https://github.com/github/awesome-copilot
[custom-instructions-support]: https://docs.github.com/copilot/reference/custom-instructions-support

## Explora los archivos de instrucciones personalizadas del proyecto

Lee los archivos que incluye este repositorio en la interfaz web de GitHub o en un codespace.

1. Abre `.github/copilot-instructions.md`.
2. Revisa la descripción del proyecto y secciones como **Agent notes**, **Code standards**, **Scripts** y **Repository Structure**. En **Code standards**, fíjate en las directrices anidadas **GitHub Actions Workflows**.
3. Abre la carpeta `.github/instructions` y explora sus instrucciones para Astro, la capa de datos Drizzle, las pruebas y otras áreas.
4. Abre `.github/instructions/unit-tests.instructions.md`. El campo `applyTo` define un glob relativo a la raíz del repositorio; por ejemplo, incluye archivos de prueba TypeScript que coincidan con `**/*.test.ts`.
5. Revisa las instrucciones específicas para crear pruebas unitarias.
6. Abre `.github/instructions/drizzle.instructions.md` y revisa al final los vínculos a otros archivos de instrucciones y a archivos del proyecto. Sus rutas son relativas al archivo de instrucciones, no a la raíz del repositorio.

> [!NOTE]
> La sección **Code formatting requirements** de `copilot-instructions.md` describe los estándares de desarrollo, pero aún no exige documentación en el código. Añadirás reglas para comentarios TSDoc y encabezados de archivo.

## Añade un estándar al repositorio

Añadirás una regla general para que el agente en la nube la use al trabajar en incidencias posteriores. Antes, crea una rama.

1. En el terminal del codespace, crea una rama y cámbiate a ella:

   ```bash
   git checkout -b custom-instructions-cloud
   ```

2. Vuelve a abrir `.github/copilot-instructions.md`.
3. Busca **Code formatting requirements**, cerca de la línea 27. Comprueba que todavía no hay directrices sobre documentación en el código.
4. Añade estas líneas Markdown debajo de los estándares existentes:

   ```markdown
   - Every exported function should have a TSDoc comment describing its purpose, parameters, and return value.
   - Before imports or any code, add a comment block to the file that explains its purpose.
   ```

5. Guarda `copilot-instructions.md`.

> [!TIP]
> Los archivos de instrucciones pueden aplicarse en todo el repositorio (`.github/copilot-instructions.md`) o a lenguajes, tipos de archivo y tareas concretos mediante archivos `*.instructions.md`. La regla de comentarios de documentación corresponde al archivo general del repositorio.

## Confirma, envía y fusiona la actualización de instrucciones

El agente en la nube lee las instrucciones de la rama a la que se dirige la incidencia. La siguiente tarea partirá de `main`, así que la actualización debe llegar a `main`.

1. Prepara y confirma el cambio:

   ```bash
   git add .github/copilot-instructions.md
   git commit -m "Add doc comment and file-header standards to copilot instructions"
   ```

2. Envía la rama:

   ```bash
   git push -u origin custom-instructions-cloud
   ```

3. En github.com, abre una pull request desde `custom-instructions-cloud` a `main` y fusiónala. El agente podrá leer estas instrucciones al trabajar en la siguiente incidencia.

> [!TIP]
> Si prefieres trabajar directamente en `main`, puedes omitir la rama y confirmar los cambios allí. La rama permite practicar un flujo más parecido al de un proyecto real.

## Resumen y siguientes pasos

Has explorado cómo Copilot recoge contexto de los archivos de instrucciones y has añadido un estándar general a `.github/copilot-instructions.md`. Observarás su efecto en las pull requests del agente en la nube.

A continuación, [asignarás tu primera incidencia al agente de Copilot en la nube][next-lesson].

## Recursos

- [Archivos de instrucciones para personalizar GitHub Copilot][instruction-files]
- [Buenas prácticas para crear instrucciones personalizadas][instructions-best-practices]
- [Cinco consejos para mejorar las instrucciones personalizadas de Copilot][copilot-instructions-five-tips]
- [Awesome Copilot: instrucciones y otros recursos][awesome-copilot]

[previous-lesson]: ../0-prerequisites/
[next-lesson]: ../2-cloud-agent/
[instruction-files]: https://docs.github.com/copilot/customizing-copilot/about-customizing-github-copilot-chat-responses
[instructions-best-practices]: https://docs.github.com/enterprise-cloud@latest/copilot/using-github-copilot/coding-agent/best-practices-for-using-copilot-to-work-on-tasks#adding-custom-instructions-to-your-repository
[copilot-instructions-five-tips]: https://github.blog/ai-and-ml/github-copilot/5-tips-for-writing-better-custom-instructions-for-copilot/
