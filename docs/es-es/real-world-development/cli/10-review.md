---
title: "Lección 10 - Resumen y siguientes pasos"
description: "Repasa el flujo de trabajo de Copilot CLI, los dos pull requests, las personalizaciones reutilizables y otros recursos."
authors:
  - geektrainer
lastUpdated: 2026-09-29
---

Has usado GitHub Copilot CLI en un flujo de trabajo continuo de Tailspin Toys. En concreto:

- has preparado un Codespace, instalado Copilot CLI, explorado el proyecto y encontrado la incidencia de filtrado que ya estaba creada.
- has añadido valoraciones por estrellas, revisado el resultado en un navegador redirigido y fusionado manualmente tu primer pull request (PR).
- has empezado con la incidencia de filtrado, has definido el enfoque en el modo Plan, has creado la funcionalidad en el modo Autopilot y la has revisado en el modo Interactive.
- has guiado al agente con instrucciones personalizadas, has personalizado la habilidad existente `quality-checks` y la has usado para ejecutar pruebas unitarias, lint y comprobaciones de tipos.
- has añadido el servidor Model Context Protocol (MCP) de Playwright y lo has usado para explorar el filtrado en un navegador real.
- has creado y seleccionado un agente personalizado de control de calidad (QA) para evaluar los requisitos, la cobertura, los resultados de la habilidad y las pruebas en el navegador.
- has revisado el cambio completo del filtrado y autorizado Agent Merge para ese PR.
- has explorado los comandos de barra para gestionar el contexto, los modelos, el uso compartido y la delegación opcional en cloud.

## Lo que has entregado

El taller incluye dos hitos de PR, cada uno en su propia rama creada a partir de `main` actualizada:

1. **Valoraciones por estrellas:** muestra `starRating` y un estado explícito de falta de valoración en las tarjetas de juegos.
2. **Flujo de trabajo de filtrado y calidad:** implementa el filtrado, actualiza las instrucciones y las aplica a la funcionalidad, personaliza el informe de `quality-checks`, crea un perfil de QA e incluye las pruebas asociadas.

Desde la planificación del filtrado hasta la apertura de su PR, has trabajado en la misma conversación y rama. Hemos agrupado ese trabajo en un único PR para agilizar el taller.

## Diferentes tipos de verificación

Has comprobado el código de varias formas: con pruebas automatizadas, mediante tu propia inspección en el navegador y con la exploración del navegador de Copilot a través de MCP. La habilidad `quality-checks` ha ejecutado pruebas unitarias, lint y comprobaciones de tipos, y ha incluido los resultados en el nuevo formato. El agente de QA ha reunido esos resultados con una revisión de los requisitos y la cobertura de pruebas antes del PR.

Las pruebas nuevas deben cerrar carencias reales; puede ser correcto que una ejecución de QA no requiera añadir pruebas. Revisa el código y las pruebas antes de autorizar la fusión, y actualiza las pruebas afectadas después de hacer cambios.

## Buenas prácticas

El contexto y las herramientas que proporcionas a Copilot influyen en su trabajo. En este taller has actualizado instrucciones, personalizado una habilidad, creado un perfil de QA y configurado un servidor MCP. Reutiliza estas personalizaciones en distintas conversaciones y ajústalas a las necesidades del equipo. Las instrucciones establecen estándares, las habilidades describen tareas repetibles, los agentes personalizados definen funciones especializadas y los servidores MCP conectan herramientas externas. Revisa los cambios y los resultados de las herramientas, no solo el resumen del agente.

Adapta el **modo y el modelo** a la tarea. Usa **Plan** para pensar en un enfoque antes de construir, **Interactive** para mantenerte al tanto de los cambios acotados y **Autopilot** para las tareas bien definidas. Elige un modelo más rápido para las ediciones rutinarias y uno más avanzado para el trabajo complejo.

El contexto sigue siendo tan importante como la infraestructura. Describir con claridad *qué* quieres crear, *por qué* y *cómo* influye de forma significativa en el resultado.

## Más opciones para explorar

Has completado el flujo de trabajo principal. Hay otras funciones de CLI que te pueden resultar útiles:

- `/review` para pedir al agente de revisión de código que analice los cambios.
- `/rubber-duck` para explicar un problema y obtener otra perspectiva.
- `/fleet` para coordinar subtareas independientes en paralelo.
- `/worktree` para aislar una tarea distinta.
- `/delegate` para enviar una tarea a Copilot cloud agent.

## Siguientes pasos

La mejor forma de mejorar con cualquier herramienta es seguir usándola. Úsala para código de producción, proyectos personales o esa pequeña aplicación que tienes en mente desde hace años, pero nunca has llegado a crear. Comparte lo que aprendas con el equipo y aprende de sus experiencias. Y, como siempre, explora la documentación.

Si quieres ampliar Tailspin Toys desde el terminal, continúa con la serie opcional [Foundry Backer Concierge][foundry]. Para comparar otros entornos, explora el [taller de VS Code][vscode], el [taller de la aplicación GitHub Copilot][app] o el [taller de Copilot cloud agent][cloud].

## Recursos

- [Acerca de GitHub Copilot CLI][about-cli]
- [Referencia de comandos de Copilot CLI][cli-reference]
- [Personalizar Copilot CLI][customize-cli]
- [Gestionar pull requests con Copilot CLI][manage-prs]

[previous-lesson]: ../9-cli-power-tools/
[foundry]: ../8-foundry-agent/
[vscode]: ../../vscode/
[app]: ../../app/
[cloud]: ../../cloud/
[about-cli]: https://docs.github.com/copilot/concepts/agents/about-copilot-cli
[cli-reference]: https://docs.github.com/copilot/reference/copilot-cli-reference/cli-command-reference
[customize-cli]: https://docs.github.com/copilot/how-tos/copilot-cli/customize-copilot
[manage-prs]: https://docs.github.com/copilot/how-tos/copilot-cli/use-copilot-cli/manage-pull-requests
