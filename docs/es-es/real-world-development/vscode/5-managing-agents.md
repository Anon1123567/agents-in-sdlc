---
title: "Ejercicio 5 - Supervisar y gestionar agentes"
authors:
  - geektrainer
lastUpdated: 2026-06-30
---

| [← Lección anterior: Agentes personalizados][previous-lesson] |
|:--|

Cuando pones GitHub Copilot en modo de agente, trabaja de forma autónoma: explora el código base, propone cambios, edita archivos y ejecuta comandos. Como trabaja en tu equipo en tiempo real, Copilot Chat de VS Code muestra cada llamada a una herramienta y cada edición de archivo a medida que se producen. Puedes revisar las diferencias en línea, aceptar o rechazar cambios concretos y orientar la conversación con prompts de seguimiento para ajustar o ampliar el trabajo sin salir del editor.

En este ejercicio:

- supervisarás en tiempo real el trabajo del agente de accesibilidad;
- revisarás y aceptarás las ediciones de archivos propuestas;
- dirigirás al agente durante la sesión para que añada un control de modo claro junto al de alto contraste.

## Escenario

El agente personalizado de accesibilidad que usaste en el [Ejercicio 4][previous-lesson] está implementando el modo de alto contraste. Mientras trabaja, el equipo de diseño solicita además un modo claro para quienes consideran que el tema predeterminado es demasiado oscuro. En vez de empezar otra conversación, orientarás la sesión actual para ampliar el trabajo.

## Supervisar el progreso de Copilot

La vista Copilot Chat muestra cada paso del agente: los archivos que explora, los comandos que ejecuta y las ediciones que propone. Cada edición aparece en línea con una diferencia y los controles **Keep** y **Undo**.

1. Vuelve a la vista Copilot Chat del Ejercicio 4.
2. Mientras trabaja el agente, recorre el chat para ver cada llamada a una herramienta. Verás lecturas y ediciones de archivos, además de los comandos del terminal que solicite ejecutar.
3. Cuando el agente proponga una edición, aparecerá una diferencia en línea:
    - Selecciona **Keep** para aceptar el cambio.
    - Selecciona **Undo** para deshacerlo y permitir que el agente vuelva a intentarlo.
4. También puedes abrir el indicador **Files changed**, en la parte superior del chat, para ver todos los archivos modificados por el agente durante la sesión.

> [!NOTE]
> Si una llamada a una herramienta solicita permiso (por ejemplo, para ejecutar un comando del terminal), el agente se detendrá y mostrará una solicitud **Allow**. Aprueba solo los comandos que te parezcan adecuados.

5. Espera a que el agente termine de implementar el modo de alto contraste. Deberías ver un mensaje final que resuma los cambios.

## Pedir al agente que añada el modo claro

Ahora que el modo de alto contraste está listo, ampliarás la misma conversación para añadir un control de modo claro. Como continuarás en la misma sesión de chat, el agente conservará el contexto del trabajo anterior, incluidos los componentes modificados, el enfoque de estilos y el patrón de almacenamiento local.

1. En el campo de texto de Copilot Chat, envía este prompt de seguimiento:

    ```plaintext
    ¡Buen trabajo! Ahora añade un control de modo claro junto al de alto contraste. El modo claro debe seguir el mismo patrón, guardarse en el almacenamiento local y estar disponible en la misma zona de la interfaz. El usuario debe poder activar el modo de alto contraste y el modo claro de forma independiente.
    ```

2. El agente continuará desde donde lo dejó, explorará los cambios anteriores y propondrá nuevas modificaciones para el modo claro.
3. Revisa cada nueva edición cuando aparezca, igual que hiciste con el modo de alto contraste. Usa **Keep** o **Undo** en cada diferencia.
4. Si el agente sigue un enfoque que no quieres —por ejemplo, intenta reorganizar los controles de una forma que no te convence—, puedes detenerlo con el botón **Stop** junto al campo de texto y enviarle una aclaración.

> [!TIP]
> Orientar una sesión en curso es más rápido que empezar de nuevo. Usa prompts de seguimiento para pedir ajustes («simplifica el CSS y usa variables CSS en lugar de duplicar selectores»), corregir el enfoque («no añadas un tercer control; integra el modo claro en el panel de configuración existente») o ampliar el alcance («añade también métodos abreviados de teclado para ambos controles»).

## Revisar los cambios

Antes de confirmar el trabajo, revisa los archivos modificados por el agente.

1. Abre la vista **Source Control** de VS Code.
2. Revisa la lista completa de archivos modificados. Deberías ver cambios en los componentes Astro, los estilos y las pruebas relacionadas.
3. Abre algunos archivos modificados y revisa las diferencias. Confirma que se sigan los principios de accesibilidad del agente personalizado: atributos ARIA, navegación con teclado, HTML semántico y persistencia en el almacenamiento local.
4. Cuando estés conforme, prepara y confirma los cambios desde el panel Source Control. Los publicarás en [la siguiente lección][next-lesson].

## Resumen y siguientes pasos

Has usado el modo de agente de Copilot Chat para supervisar y dirigir una sesión en curso. Has revisado las ediciones de cada archivo, aceptado o rechazado los cambios y enviado prompts de seguimiento para ampliar el trabajo sin perder el contexto.

En esta lección:

- has supervisado el trabajo de un agente en tiempo real;
- has aceptado y rechazado ediciones de archivos individuales;
- has orientado una sesión en curso con prompts de seguimiento.

En la [siguiente lección][next-lesson] publicarás el trabajo de accesibilidad como pull request y revisarás los cambios realizados por el agente local personalizado durante el taller.

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
