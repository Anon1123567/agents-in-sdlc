---
title: "Ejercicio 4 - Supervisar y gestionar agentes"
authors:
  - geektrainer
lastUpdated: 2026-06-30
---

En los dos ejercicios anteriores has pedido a Copilot cloud agent que se ocupe de tres tareas independientes para mejorar la experiencia de usuario y añadir funcionalidades. Aunque el agente está diseñado para trabajar de forma asíncrona y autónoma, también es importante poder supervisar esas tareas.

Hay varias herramientas para gestionar las tareas asignadas a cloud agent, entre ellas, la [página de agentes][agents-page] de GitHub.com. Desde este centro de control puedes ver todas las tareas de agentes que tienen pull requests (PR) abiertos. Puedes explorar las operaciones realizadas e incluso dirigir una sesión que esté en curso.

En esta lección:

- explorarás la página de agentes para supervisar las tareas de cloud agent;
- dirigirás una sesión en curso para pedir una funcionalidad adicional.

## Escenario

Después de asignar al agente la tarea de crear el modo de alto contraste, el equipo decidió que sería buen momento para añadir también un modo claro. Como ya se estaba trabajando en actualizar los estilos del sitio y añadir controles, parecía lógico incluir esta funcionalidad. Quieres orientar el trabajo del agente para que añada el modo claro además del modo de alto contraste.

## Revisar las tareas de Copilot cloud agent

Vamos a comprobar el estado de todas las tareas asignadas a Copilot cloud agent.

1. Ve a la [página de agentes de Copilot](https://github.com/copilot/agents).
2. Observa la lista de tareas en el panel principal y en el panel izquierdo. Deberías ver las tareas que has asignado a Copilot, entre ellas:
    - actualizar la documentación del código;
    - generar API para modificar juegos;
    - añadir un modo de alto contraste al sitio web.
3. Selecciona una de las tareas en curso. Revisa las operaciones que ha realizado Copilot, por ejemplo:
    - obtener el código del repositorio;
    - crear el entorno de trabajo de Copilot;
    - configurar servidores MCP;
    - realizar distintos pasos para completar la tarea.

> [!NOTE]
> Los pasos exactos que aparecen dependen del estado del trabajo de Copilot y del enfoque que haya seguido.

4. Observa también el panel de pull requests que aparece a la derecha. Te permite ver el PR y los archivos modificados para seguir el trabajo con más detalle.

## Dirigir el trabajo del agente en la nube

Ya has visto las tareas activas. Ahora vamos a pedir a Copilot que añada el modo claro mientras trabaja en el modo de alto contraste.

1. Selecciona la sesión relacionada con el modo de alto contraste. El título exacto dependerá del nombre que Copilot haya usado y del estado del trabajo.

    ![Sesión de accesibilidad en el centro de control](../../../_images/ex6-accessibility-session.png)

2. Observa la sesión durante unos minutos, hasta que indique que ha terminado de configurar el entorno y ha empezado a trabajar. Verás mensajes similares a los siguientes.
3. En el cuadro de diálogo **Steer active session while Copilot is working**, escribe este prompt:

    ```plaintext
    Mientras trabajamos en el modo de alto contraste, vamos a añadir también un modo claro. Debe haber un control para este modo, de forma que los usuarios puedan seleccionar el modo de visualización que prefieran.
    ```

    ![Captura de la tarea de cloud agent en la página de agentes, con el cuadro de diálogo para dirigir una sesión activa mientras Copilot trabaja resaltado](../../../_images/ex6-steer-cloud-agent-task.png)

4. Pulsa <kbd>Enter</kbd> para enviar el prompt.
5. Observa cómo Copilot reconoce la solicitud y la incorpora a su trabajo.

## Dejar que Copilot trabaje

Como antes, Copilot empezará a trabajar en la tarea actualizada. Incorporará la nueva solicitud al terminar el paso en el que estuviera trabajando cuando enviaste el mensaje.

Esto también tardará varios minutos. Puedes aprovechar el tiempo para reflexionar sobre todo lo que has aprendido y explorado.

## Resumen y siguientes pasos

En esta lección has explorado la página de agentes de Copilot, el centro de control para supervisar y orientar las tareas de GitHub Copilot cloud agent. Puedes hacer un seguimiento de las tareas activas y completadas, revisar el trabajo en curso y redirigir las sesiones para ajustar su alcance o añadir indicaciones.

Has explorado estos conceptos:

- usar el centro de control y la página de agentes para supervisar las tareas de cloud agent;
- redirigir una sesión en curso para solicitar una funcionalidad adicional.

Mientras Copilot termina de trabajar en las funciones de accesibilidad, puedes [iterar sobre los pull requests creados por Copilot][next-lesson].

## Recursos

- [Página de agentes de Copilot][agents-page]
- [Acerca de los agentes personalizados][custom-agents]

---

| [← Lección anterior: Agentes personalizados][previous-lesson] | [Siguiente lección: Iterar sobre el trabajo de Copilot →][next-lesson] |
|:--|--:|

[previous-lesson]: ../3-custom-agents/
[next-lesson]: ../5-iterating/
[agents-page]: https://github.blog/changelog/2025-07-03-agents-page-for-copilot-coding-agent-in-public-preview
[custom-agents]: https://docs.github.com/copilot/concepts/agents/cloud-agent/about-custom-agents
