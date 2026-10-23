---
title: "Lección 9 - Comandos de barra en GitHub Copilot CLI"
description: "Explora comandos de barra para gestionar el contexto, elegir modelos, compartir sesiones y delegar tareas opcionalmente en la nube."
authors:
  - geektrainer
lastUpdated: 2026-09-18
---

Como cualquier buena herramienta de CLI, GitHub Copilot CLI incluye comandos de barra para interactuar con ella. Estos comandos permiten acceder a funciones avanzadas, información interna y opciones de configuración. Ya has usado `/diff`, `/mcp`, `/skills`, `/agent` y `/pr`. Ahora explorarás otros comandos útiles.

En esta lección:

- usarás `/context` y `/compact` para explorar cómo gestiona Copilot el contexto de la conversación.
- usarás `/model` para explorar los modelos disponibles.
- aprenderás a exportar o compartir sesiones con `/share`.
- explorarás comandos opcionales para trabajar en paralelo, usar árboles de trabajo y delegar tareas en cloud agent.

## Escenario

Ya has terminado el flujo principal de CLI. Ahora explorarás otras capacidades: gestionar el contexto, cambiar de modelo, compartir sesiones y delegar opcionalmente trabajo en [Copilot cloud agent][about-cloud-agent].

## Explora el contexto de Copilot CLI

En tareas complejas o extensas puedes acercarte al límite de la ventana de contexto del modelo. Copilot CLI compacta automáticamente la conversación cuando hace falta; también puedes inspeccionar o compactar el contexto mediante comandos de barra.

1. Vuelve a tu Codespace e inicia Copilot CLI desde la raíz del repositorio, si aún no está abierto.
2. Introduce:

   ```plaintext
   /context
   ```

3. Fíjate en el modelo, el uso actual de tokens y cómo se distribuye el contexto entre instrucciones del sistema, herramientas, mensajes y espacio libre.
4. Compacta la conversación:

   ```plaintext
   /compact
   ```

5. Vuelve a introducir `/context` y compara el resultado. Si la conversación ya era breve, puede que apenas haya cambiado.

> [!NOTE]
> Copilot CLI compacta el contexto automáticamente cuando se llena la ventana. Usa `/compact` para elegir cuándo hacerlo. Usa `/clear` o `/new` al pasar a una tarea no relacionada y empezar una conversación nueva.

## Elige un modelo

Los modelos tienen puntos fuertes distintos y cada desarrollador puede tener preferencias diferentes. Copilot CLI permite consultar y seleccionar un modelo.

1. Introduce:

   ```plaintext
   /model
   ```

2. Explora los modelos disponibles y la información de uso.
3. Mantén el modelo actual, selecciona otro o pulsa <kbd>Esc</kbd> para cerrar la lista.

## Comparte una sesión

Compartir lo aprendido ayuda al equipo a mejorar el uso de herramientas de IA. El comando `/share` puede exportar una sesión a un archivo Markdown o HTML, crear un vínculo para compartirla o publicar un GitHub gist.

1. Introduce `/help` y revisa las opciones de `/share` en tu versión instalada.
2. Si quieres compartir la sesión, elige un destino adecuado; por ejemplo, `/share file` para exportarla localmente como Markdown.
3. Revisa el contenido exportado antes de enviarlo o publicarlo. Las exportaciones pueden incluir prompts, respuestas y detalles del proyecto.

Publicar un vínculo o gist es opcional. No publiques contenido del repositorio ni de la conversación que el equipo no haya autorizado compartir.

## Opcional: ampliar el trabajo o delegarlo

El flujo principal del taller ha terminado. Copilot CLI también incluye comandos para tareas más amplias:

- `/fleet` puede repartir subtareas independientes entre subagentes y ejecutarlas en paralelo.
- `/worktree` puede crear un árbol de trabajo de Git aislado para otra tarea.
- `/delegate` puede enviar una tarea a Copilot cloud agent, que trabaja de forma asíncrona y puede abrir una pull request.

Estos comandos son opcionales porque pueden crear árboles de trabajo adicionales o iniciar trabajo remoto. Antes de probarlos, empieza una tarea nueva y bien delimitada, y revisa el resultado con tu proceso habitual. Para profundizar en el trabajo asíncrono con agentes, continúa con el [taller de cloud agent][cloud-workshop].

## Resumen y siguientes pasos

Los comandos de barra de Copilot CLI te permiten configurarlo, compartir sesiones y consultar su actividad interna. En esta lección:

- usaste `/context` y `/compact` para explorar cómo gestiona Copilot el contexto.
- usaste `/model` para explorar los modelos disponibles.
- aprendiste a exportar o compartir una sesión con `/share`.
- exploraste comandos opcionales para trabajar en paralelo, usar árboles de trabajo y delegar en cloud agent.

Hay más comandos de barra y funciones de Copilot CLI por explorar. Para terminar, [repasarás lo aprendido][next-lesson] y verás cómo seguir aprendiendo.

## Recursos

- [Referencia de comandos de Copilot CLI][cli-reference]
- [Gestión del contexto en Copilot CLI][context-management]
- [Acerca de Copilot cloud agent][about-cloud-agent]

[previous-lesson]: ../8-create-pull-request/
[next-lesson]: ../10-review/
[cli-reference]: https://docs.github.com/copilot/reference/copilot-cli-reference/cli-command-reference
[context-management]: https://docs.github.com/copilot/concepts/agents/copilot-cli/context-management
[about-cloud-agent]: https://docs.github.com/copilot/concepts/agents/cloud-agent/about-cloud-agent
[cloud-workshop]: ../../cloud/
