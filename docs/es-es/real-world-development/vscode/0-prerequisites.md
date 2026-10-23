---
title: "Ejercicio 0: Requisitos previos"
authors:
  - geektrainer
lastUpdated: 2026-06-30
---

Antes de empezar los ejercicios de VS Code, prepara el entorno. Crearás tu propia copia del repositorio Tailspin Toys, iniciarás un [codespace][codespaces] y confirmarás que GitHub Copilot Chat funciona en el editor.

## Configurar el repositorio del laboratorio

Crearás una instancia a partir de la [plantilla][template-repository]. La nueva instancia contendrá los archivos necesarios para el laboratorio.

1. En una ventana nueva del navegador, abre la [plantilla de Tailspin Toys][tailspin-template].
2. Selecciona **Use this template** en la página del repositorio del laboratorio y, después, **Create a new repository**.

   ![Botón Use this template](../../../_images/ex0-use-template.png)

3. Si participas en un evento dirigido por GitHub o Microsoft, sigue las instrucciones de los mentores. De lo contrario, crea el repositorio en una organización donde tengas acceso a GitHub Copilot.

   ![Configuración de la plantilla del repositorio](../../../_images/ex0-repository-settings.png)

4. Anota la ruta del repositorio que has creado (**organization-or-user-name/repository-name**); la necesitarás más adelante.

> [!NOTE]
> **El backlog está preparado**
>
> Al crear el repositorio a partir de la plantilla, se crea automáticamente un backlog de incidencias de GitHub. Trabajarás con ellas durante el taller; no tienes que crear incidencias.

## Crear un codespace

Usarás un codespace para completar los ejercicios. [GitHub Codespaces][codespaces] es un entorno de desarrollo en la nube que permite escribir, ejecutar y depurar código desde el navegador. Proporciona un IDE completo con compatibilidad para distintos lenguajes, extensiones y herramientas.

1. Abre el repositorio que acabas de crear.
2. Selecciona el botón verde **Code**.

   ![Botón Code](../../../_images/ex0-code-button.png)

3. Selecciona la pestaña **Codespaces** y, después, el botón **+** para crear un Codespace.

   ![Crear un codespace](../../../_images/ex0-create-codespace.png)

La creación del codespace puede tardar varios minutos. Es más rápido que instalar manualmente todos los servicios; mientras esperas, puedes explorar otras funciones de GitHub Copilot.

> [!CAUTION]
> Volverás al codespace en un ejercicio posterior. Por ahora, déjalo abierto en una pestaña del navegador.

> [!NOTE]
> El taller está diseñado para ejecutarse en un codespace o en un [contenedor de desarrollo][dev-containers] local. Ambos entornos incluyen los requisitos previos necesarios. Para trabajar localmente, abre el repositorio clonado en VS Code y selecciona **Reopen in Container** cuando se te solicite; VS Code creará el mismo contenedor de desarrollo que usa el codespace.

[codespaces]: https://github.com/features/codespaces
[dev-containers]: https://code.visualstudio.com/docs/devcontainers/containers

## Usa GitHub Copilot Chat y el modo de agente

Para usar el modo de agente de GitHub Copilot Chat, necesitas tener instalada la extensión GitHub Copilot Chat en el IDE. Debería estar instalada si usas GitHub Codespaces.

> [!TIP]
> Si no tienes la extensión, [instálala desde Visual Studio Code Marketplace][copilot-chat-extension]. También puedes abrir la vista Extensions en Visual Studio Code, buscar **GitHub Copilot Chat** y seleccionar **Install**.

Después de instalar la extensión, quizá tengas que autenticarte con tu cuenta de GitHub.

1. Vuelve al codespace.
2. Si no ves Copilot Chat en el lado derecho del editor, selecciona el icono **Copilot Chat** en la parte superior del codespace.
3. Escribe un mensaje como «Hello world» en la ventana de Copilot Chat y pulsa <kbd>Enter</kbd>.
4. Si no tienes una sesión autenticada, sigue las indicaciones para iniciar sesión en tu cuenta de GitHub.

   ![Ejemplo del mensaje de autenticación de Copilot Chat](../../../_images/ex1-copilot-authentication.png)

5. Tras autenticarte, se abrirá la ventana de Copilot Chat.

## Resumen

Has creado una copia del repositorio del laboratorio e iniciado la creación del codespace que usarás para escribir código.

## Siguiente paso

A continuación, pondrás Copilot a trabajar. Continúa con el [ejercicio 1: instrucciones personalizadas][next-lesson], donde le enseñarás las convenciones del proyecto.

## Recursos

- [Información general sobre GitHub Codespaces][codespaces]
- [Crear un repositorio a partir de una plantilla][template-repository]
- [Introducción a Codespaces][codespaces-quickstart]

[tailspin-template]: https://github.com/github-samples/tailspin-toys
[template-repository]: https://docs.github.com/repositories/creating-and-managing-repositories/creating-a-template-repository
[codespaces-quickstart]: https://docs.github.com/codespaces/getting-started/quickstart
[copilot-chat-extension]: https://marketplace.visualstudio.com/items?itemName=GitHub.copilot-chat
[next-lesson]: ../1-custom-instructions/
