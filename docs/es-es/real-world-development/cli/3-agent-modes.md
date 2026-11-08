---
title: "Lección 3 - Modos de agente: Plan y Autopilot"
description: "Usa Plan para acordar un enfoque, Autopilot para crear el filtrado a partir de una incidencia e Interactive para revisar el resultado."
authors:
  - geektrainer
lastUpdated: 2026-09-18
---

Empezamos añadiendo una pequeña función al proyecto, pero los cambios más amplios necesitan un proceso más robusto. Por suerte, GitHub Copilot CLI está diseñado para adaptarse al flujo de trabajo de las organizaciones y ayudar a crear las cosas adecuadas de la forma adecuada. Esta es la primera de varias lecciones en las que seguirás un proceso habitual de desarrollo con agentes: partirás de una incidencia para crear una función, confirmarás que el código es válido y que la función se comporta como esperas y, finalmente, la fusionarás correctamente en el proyecto.

> [!NOTE]
> Usarás la misma conversación y rama durante todo el flujo de trabajo de esta funcionalidad. Normalmente tendrías ramas o PR distintos para cada tipo de archivo, pero usaremos un atajo para centrarnos en los conceptos principales.

En esta lección:

- iniciarás una conversación de Copilot a partir de una incidencia de GitHub;
- definirás los requisitos en el modo Plan;
- implementarás la funcionalidad con el modo Autopilot;
- revisarás el código;
- validarás manualmente la funcionalidad en un navegador redirigido.

Mientras sigues trabajando en esta funcionalidad, actualizarás las instrucciones del repositorio, personalizarás la habilidad existente `quality-checks`, añadirás validación con MCP, crearás un agente de QA y abrirás el PR de la funcionalidad.

## Escenario

El catálogo de Tailspin Toys crece y quienes visitan el sitio necesitan filtrar los juegos por categoría y editorial. La incidencia del backlog describe la funcionalidad, pero antes de programar hay que acordar algunos detalles, como la combinación de categorías. Usarás el modo Plan para resolver esas decisiones y, después, autorizarás una implementación acotada con Autopilot.

## Contexto

Introducir agentes de programación con IA en el flujo de desarrollo no cambia sus fundamentos. De hecho, ¡los hace aún más importantes! La mayoría de los desarrolladores siguen un proceso parecido a este:

1. Empezar con una incidencia registrada que describa el trabajo.
2. Crear un plan de implementación.
3. Crear y revisar el código.
4. Ejecutar las pruebas para validar el código.
5. Validar manualmente la nueva funcionalidad.
6. Crear un pull request (PR).
7. Revisar el código y, cuando se superen las comprobaciones de integración continua, fusionarlo.

> [!NOTE]
> Los detalles concretos dependen del equipo y de la organización, pero la mayoría de los flujos de trabajo siguen una variación de este proceso.

Al seguir este enfoque, confirmas que el código generado por IA cumple los requisitos y pasa por el mismo proceso de revisión que el código escrito manualmente.

## Modos de conversación

El **modo de conversación** controla la autonomía del agente. Pulsa <kbd>Shift</kbd>+<kbd>Tab</kbd> para cambiar de modo:

- **Interactive:** tú y el agente trabajáis juntos. El agente propone cambios y espera tu respuesta para continuar.
- **Plan:** el agente crea primero un plan y no puede editar los archivos del proyecto.
- **Autopilot:** el agente trabaja de forma autónoma: escribe código, ejecuta pruebas e itera hasta completar la tarea.

Empieza en el modo Plan, revisa el plan y, después, usa Autopilot para implementarlo.

## Empezar con la incidencia

Antes de trabajar en el filtrado, vuelve a tu Codespace y asegúrate de que el repositorio y el terminal estén listos.

1. Vuelve a tu Codespace. Si está detenido, reinícialo antes de continuar.
2. Confirma que el PR de la valoración por estrellas se ha fusionado.
3. Si el terminal no está abierto, pulsa <kbd>Control</kbd>+<kbd>`</kbd> (Mac) o <kbd>Ctrl</kbd>+<kbd>`</kbd> (Windows/Linux).
4. Actualiza `main` y crea una rama para el trabajo de filtrado:

   ```bash
   git checkout main
   git pull --ff-only
   git checkout -b game-filters-cli
   ```

5. Inicia Copilot CLI:

   ```bash
   copilot --yolo
   ```

6. Pulsa <kbd>Tab</kbd> dos veces para abrir la pestaña **Issues**.
7. Pulsa <kbd>A</kbd> para mostrar todas las incidencias.
8. Usa las teclas de dirección para seleccionar la incidencia **Allow users to filter games by category and publisher**.
9. Pulsa <kbd>C</kbd> para añadir la incidencia al prompt y volver a la pestaña **Session**.

Observa que el prompt ahora empieza por `#7` (o un número parecido). El carácter `#` permite incluir en el contexto una incidencia o un pull request (PR) de GitHub.

## Planificar la funcionalidad de filtrado

La planificación te permite definir cómo se implementará una función o se realizará una tarea antes de delegársela a Copilot. Para cualquier trabajo complejo, conviene dedicar un tiempo a planificarlo. Vamos a cambiar al modo Plan y pedir a Copilot que cree el plan.

1. Pulsa <kbd>Shift</kbd>+<kbd>Tab</kbd> para cambiar al modo Plan. Confirma que el indicador debajo del prompt muestre **Plan**.
2. Después de la referencia a la incidencia que has añadido en el paso anterior, escribe el siguiente prompt:

   ```plaintext
   Crea un plan para implementar esta función.
   ```

Copilot explorará el proyecto y determinará el mejor enfoque para elaborar el plan.

3. Puede que Copilot te haga preguntas sobre el funcionamiento del filtrado. Respóndelas según tus preferencias. No hay respuestas incorrectas.
4. Cuando termine el plan, pulsa <kbd>Control</kbd>+<kbd>E</kbd> (Mac) o <kbd>Ctrl</kbd>+<kbd>E</kbd> (Windows/Linux) para ampliarlo.
5. Desplázate por el plan y revísalo.
6. Pide a Copilot que modifique cualquier parte del plan que no se ajuste a tus decisiones.

## Aprobar Autopilot

Ya has escrito y revisado el plan. Es el momento de implementarlo. Deja que Copilot trabaje en la tarea con Autopilot.

Autopilot permite que Copilot itere hasta considerar que ha completado el trabajo.

1. Selecciona **Accept plan and build on autopilot (recommended)** o la opción equivalente de tu versión.
2. Confirma que el indicador debajo del prompt muestre **Autopilot**.
3. Observa cómo Copilot sigue el plan, genera código y ejecuta pruebas.

> [!NOTE]
> Al aprobar el plan, la implementación puede empezar inmediatamente, así que revísalo antes. Si Copilot indica que faltan dependencias o que hay un conflicto de puertos, resuelve el problema de configuración antes de dar por completas las comprobaciones.

## Revisar y verificar la implementación

Como cualquier otro código, hay que revisar el código generado antes de fusionarlo. Vamos a revisar el código y ejecutar el sitio para confirmar que todo funciona.

1. Pulsa <kbd>Shift</kbd>+<kbd>Tab</kbd> para entrar en el modo Interactive. Confirma que el indicador ya no muestre **Plan** ni **Autopilot**.
2. Escribe `/diff` y revisa la implementación del filtrado y las pruebas.
3. Compara el resultado con la incidencia y las decisiones tomadas durante la planificación.
4. Cuando hayas revisado el código, pulsa <kbd>Esc</kbd> para salir de la vista de diferencias.
5. Revisa los resultados de las comprobaciones del proyecto y pide a Copilot que resuelva cualquier error.

## Explorar la nueva funcionalidad

El código parece correcto, pero ¿se ejecuta? Iniciemos la aplicación como antes y abramos el sitio a través del puerto redirigido de Codespaces.

1. Pide a Copilot que inicie la aplicación:

   ```plaintext
   Inicia la aplicación para que pueda probar en el navegador la funcionalidad de filtrado. Dime la URL y deja el servidor en marcha.
   ```

2. Cuando Codespaces indique que el puerto `4321` está disponible, selecciona **Open in Browser**.
3. Prueba los filtros de categoría y editorial, así como las combinaciones que acordaste en el plan.
4. Confirma que el comportamiento de restablecimiento y de resultados vacíos coincida con la incidencia y con tus decisiones.
5. Vuelve a tu Codespace y pide a Copilot que detenga el servidor de desarrollo que ha iniciado.

## Resumen y siguientes pasos

Has usado distintos modos de conversación para crear y revisar una funcionalidad. En esta lección:

- has iniciado una conversación de Copilot a partir de una incidencia de GitHub;
- has definido los requisitos en el modo Plan;
- has implementado la funcionalidad con el modo Autopilot;
- has revisado el código;
- has validado manualmente la función en un navegador redirigido.

A continuación, veremos cómo generar código de acuerdo con las prácticas documentadas al [usar instrucciones personalizadas][next-lesson].

## Recursos

- [Autopilot en GitHub Copilot CLI][autopilot]
- [Referencia de comandos de Copilot CLI][cli-reference]

[autopilot]: https://docs.github.com/copilot/concepts/agents/copilot-cli/autopilot
[cli-reference]: https://docs.github.com/copilot/reference/copilot-cli-reference/cli-command-reference
[previous-lesson]: ../2-add-star-rating/
[next-lesson]: ../4-custom-instructions/
