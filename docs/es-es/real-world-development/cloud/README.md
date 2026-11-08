---
slug: es-es/real-world-development/cloud
title: "GitHub Copilot cloud agent"
authors:
  - geektrainer
lastUpdated: 2026-06-30
---

**[GitHub Copilot cloud agent](https://docs.github.com/copilot/concepts/agents/cloud-agent/about-cloud-agent)** permite que GitHub Copilot trabaje de forma asíncrona en la nube. Asignas el trabajo en GitHub y el agente en la nube se encarga de él en segundo plano: explora el repositorio, realiza cambios y abre un pull request mientras tú puedes dedicarte a otras tareas.

En estos ejercicios añadirás instrucciones personalizadas que seguirá el agente en la nube y, después, le asignarás una incidencia de GitHub para que implemente el trabajo. Revisarás y usarás agentes personalizados para definir su enfoque, supervisarás y orientarás las sesiones desde el panel de agentes y terminarás revisando sus pull requests e iterando sobre los resultados.

## Ejercicios

| Ejercicio | Tema | Descripción |
|----------|-------|-------------|
| [0. Requisitos previos][ex0] | Configuración | Crea tu repositorio y Codespace |
| [1. Instrucciones personalizadas][ex1] | Contexto | Añade instrucciones personalizadas para el agente en la nube |
| [2. Cloud agent][ex2] | Agente asíncrono | Asigna incidencias a GitHub Copilot cloud agent |
| [3. Agentes personalizados][ex3] | Agentes especializados | Revisa y usa agentes personalizados |
| [4. Supervisar y gestionar agentes][ex4] | Supervisión | Supervisa y dirige las sesiones del agente |
| [5. Iterar][ex5] | Revisión | Revisa los PR, itera sobre el trabajo de Copilot y elige los siguientes pasos |

## Requisitos previos

Antes de participar en este taller, asegúrate de tener:

- [ ] Una cuenta de GitHub con un plan activo **Copilot Student, Pro, Pro+, Business o Enterprise**
- [ ] Copilot cloud agent habilitado en tu cuenta u organización

> [!TIP]
> ¿No tienes un plan de pago? Los estudiantes verificados pueden obtener GitHub Copilot gratis a través de [GitHub Education][callout-student-plan-education]. El plan **Copilot Student** incluye el agente, MCP, la revisión de código y las funciones de Copilot CLI que se usan en este taller, así que podrás completar todos los recorridos con él.

[callout-student-plan-education]: https://github.com/education/students

> [!NOTE]
> Cloud agent está disponible en los planes **Copilot Student, Pro, Pro+, Business y Enterprise**: en todos excepto Copilot Free. En Copilot Business o Enterprise, un administrador debe habilitarlo para la organización.

> [!NOTE]
> Este recorrido no incluye MCP. Para aprender a usar servidores MCP con Copilot, consulta el [recorrido de CLI][mcp-cli] o el [recorrido de VS Code][mcp-vscode].

## Empezar

**[Empieza con el Ejercicio 0: Requisitos previos →][ex0]**

[ex0]: 0-prerequisites/
[ex1]: 1-custom-instructions/
[ex2]: 2-cloud-agent/
[ex3]: 3-custom-agents/
[ex4]: 4-managing-agents/
[ex5]: 5-iterating/
[mcp-cli]: ../cli/
[mcp-vscode]: ../vscode/
