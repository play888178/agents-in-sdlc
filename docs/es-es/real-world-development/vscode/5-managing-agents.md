---
title: "Ejercicio 5 - Supervisar y gestionar agentes"
authors:
  - geektrainer
lastUpdated: 2026-06-30
---

| [← Lección anterior: Agentes personalizados][previous-lesson] |
|:--|

En modo de agente, GitHub Copilot trabaja de forma autónoma: explora el código, propone cambios, edita archivos y ejecuta comandos. Copilot Chat en VS Code muestra en tiempo real las llamadas a herramientas y las ediciones. Puedes revisar las diferencias en el editor, aceptar o rechazar cada cambio y dirigir la conversación con prompts de seguimiento.

En este ejercicio:

- supervisarás en tiempo real el trabajo del agente de accesibilidad.
- revisarás y aceptarás las ediciones propuestas.
- pedirás al agente que añada un modo claro junto al modo de alto contraste.

## Escenario

El agente personalizado de accesibilidad del [ejercicio 4][previous-lesson] está implementando el modo de alto contraste. Mientras trabaja, el equipo de diseño solicita también un modo claro para las personas a quienes el tema predeterminado les resulta demasiado oscuro. Dirigirás la sesión existente para ampliar el trabajo sin empezarlo desde cero.

## Supervisa el progreso de Copilot

La vista Copilot Chat muestra cada paso del agente: archivos que explora, comandos que ejecuta y cambios que propone. Las ediciones aparecen en el chat con las opciones **Keep** y **Undo**.

1. Vuelve a la vista Copilot Chat del ejercicio 4.
2. Desplázate por el chat para observar cada llamada a herramienta, lectura, edición y comando de terminal.
3. Cuando el agente proponga una edición, revisa sus diferencias y selecciona **Keep** para aceptarla o **Undo** para descartarla y permitir otro intento.
4. Abre el indicador **Files changed** en la parte superior del chat para ver los archivos modificados durante la sesión.

> [!NOTE]
> Si una llamada a herramienta requiere permiso, por ejemplo para ejecutar un comando, el agente se detendrá y mostrará una solicitud **Allow**. Aprueba solo los comandos que te parezcan adecuados.

5. Espera a que el agente termine el modo de alto contraste. Debería mostrar un mensaje final con el resumen de los cambios.

## Pide al agente que añada el modo claro

Extenderás la misma conversación para añadir el modo claro. El agente conservará el contexto de su trabajo anterior, incluidos los componentes, los estilos y el patrón de almacenamiento local.

1. Envía este prompt de seguimiento en Copilot Chat:

   ```plaintext
   Nice work! Now add a light-mode toggle alongside the high-contrast toggle. Light mode should follow the same pattern — persist via local storage and be accessible from the same area of the UI. The user should be able to enable high-contrast and light mode independently.
   ```

2. Revisa cada edición nueva con **Keep** o **Undo**.
3. Si el agente toma una dirección que no quieres —por ejemplo, intenta reorganizar los controles—, detenlo con **Stop** junto al campo de chat y envía una aclaración.

> [!TIP]
> Dirigir una sesión en curso es más rápido que empezar de cero. Puedes pedir ajustes de estilo, corregir el enfoque o ampliar el alcance mediante prompts de seguimiento.

## Revisa el conjunto de cambios

Antes de confirmar el trabajo, revisa todo lo que haya modificado el agente.

1. Abre la vista **Source Control** de VS Code.
2. Revisa la lista completa de archivos modificados. Deberías ver componentes Astro, estilos y pruebas relacionadas.
3. Abre algunos archivos modificados y revisa las diferencias. Comprueba los atributos ARIA, la navegación con teclado, el HTML semántico y la persistencia en almacenamiento local.
4. Cuando estés conforme, prepara y confirma los cambios desde el panel Source Control. Los publicarás en la [siguiente lección][next-lesson].

## Resumen y siguientes pasos

Has usado el modo de agente de Copilot Chat para supervisar y dirigir una sesión en curso: revisaste cada edición, aceptaste o rechazaste los cambios y enviaste prompts de seguimiento para ampliar el trabajo sin perder el contexto.

En esta lección:

- supervisaste el trabajo del agente en tiempo real.
- aceptaste y rechazaste ediciones individuales.
- dirigiste una sesión en curso mediante prompts de seguimiento.

A continuación, publicarás el trabajo de accesibilidad en una pull request y revisarás los cambios del agente personalizado local.

## Recursos

- [Copilot Chat en VS Code][copilot-chat-vscode]
- [Usar el modo de agente][agent-mode]

---

| [← Lección anterior: Agentes personalizados][previous-lesson] | [Siguiente lección: Iterar sobre el trabajo de Copilot →][next-lesson] |
|:--|--:|

[previous-lesson]: ../4-custom-agents/
[next-lesson]: ../6-iterating/
[copilot-chat-vscode]: https://code.visualstudio.com/docs/copilot/chat/copilot-chat
[agent-mode]: https://code.visualstudio.com/docs/copilot/chat/chat-agent-mode
