---
title: "Lección 7 - Crear y usar un agente de QA"
description: "Crea un agente personalizado de QA que combine los requisitos de la incidencia, la habilidad quality-checks y Playwright MCP."
authors:
  - geektrainer
lastUpdated: 2026-09-18
---

Has usado la habilidad `quality-checks` para ejecutar comprobaciones automatizadas y Playwright MCP para observar la experiencia de filtrado en un navegador. Ahora reunirás esas funciones en un agente personalizado con un proceso de control de calidad (QA) bien definido.

En esta lección:

- explorarás cómo un agente personalizado trabaja con instrucciones, habilidades y herramientas MCP;
- crearás y revisarás un perfil reutilizable de control de calidad;
- seleccionarás el agente de QA y revisarás sus conclusiones respecto a la incidencia del filtrado.

## Escenario

Antes de abrir un pull request (PR), Tailspin Toys quiere revisar de forma coherente los requisitos, la calidad del código, las comprobaciones automatizadas, la cobertura de pruebas y el comportamiento en el navegador. Un agente personalizado puede coordinar el proceso de QA y generar un informe reutilizable.

## ¿Qué es un agente personalizado?

Un agente personalizado es una versión especializada de Copilot definida en un perfil Markdown. El perfil describe su finalidad, sus instrucciones y las herramientas disponibles. En este taller, definirás la función de QA en `.github/agents/qa.agent.md` y la seleccionarás en Copilot CLI.

Las personalizaciones que has usado tienen funciones distintas. Las instrucciones del repositorio describen los estándares del equipo. La habilidad `quality-checks` reúne comprobaciones repetibles. Playwright MCP proporciona herramientas para el navegador. El perfil de QA indica a Copilot cómo usar esas capacidades para evaluar los requisitos y comunicar los resultados. No sustituye a las demás personalizaciones ni requiere otra conversación.

## Crear el perfil de QA

Antes de abrir el PR de la funcionalidad, pedirás a Copilot que cree un perfil de QA reutilizable. El perfil definirá las comprobaciones y los límites que debe respetar.

1. Vuelve a tu Codespace y confirma que la conversación de filtrado esté en modo Interactive.
2. Pide al agente predeterminado que cree el agente personalizado:

   ```plaintext
   Crea un agente personalizado llamado QA en .github/agents/qa.agent.md. Debe comprobar las funcionalidades respecto a sus incidencias y los requisitos acordados, seguir las instrucciones del repositorio, ejecutar la habilidad quality-checks, usar Playwright MCP para verificar el comportamiento y añadir pruebas si falta cobertura.

   Debe indicar cada requisito como superado, fallido o bloqueado, con pruebas que respalden el resultado. Debe preguntar antes de cambiar el código de implementación y no debe confirmar cambios ni abrir pull requests.

   De momento, crea solo el perfil para que pueda revisarlo.
   ```

## Revisar el perfil

Antes de usar el nuevo agente, revisa su perfil para confirmar que Copilot ha incluido el flujo de trabajo de QA y sus límites. Así evitarás que un agente incompleto o demasiado amplio modifique la funcionalidad cuando solo quieres verificarla.

1. Escribe `/diff` y abre `.github/agents/qa.agent.md`.
2. Lee el frontmatter. `description` es obligatorio; `name` es opcional, pero incluirlo da al agente un nombre claro en la interfaz.
3. Lee las instrucciones del perfil y confirma que QA empieza por los requisitos, sigue las instrucciones del repositorio, ejecuta la habilidad `quality-checks` y usa Playwright MCP.
4. Confirma que el agente aporte pruebas para sus conclusiones, pregunte antes de modificar el código de implementación y no confirme cambios ni abra pull requests.
5. Si el perfil generado omite alguna de estas responsabilidades o restricciones, pide al agente predeterminado que lo revise antes de continuar.

## Ejecutar QA para la incidencia

Copilot CLI carga los agentes del proyecto al iniciar una conversación. Después de crear el perfil, retoma la conversación de filtrado y selecciona QA para que pueda usar la incidencia y las decisiones de planificación que ya forman parte del contexto.

1. Habilita el agente con este prompt:

   ```plaintext
   /agent QA
   ```

> [!NOTE]
> Como acabas de crear el agente, es posible que no aparezca en la lista de agentes disponibles. El comando anterior lo activará igualmente.

2. Envía este prompt para pedir al agente de QA que revise la funcionalidad:

   ```plaintext
   Revisa la funcionalidad de filtrado respecto a la incidencia y las decisiones de nuestro plan. ¿Está lista para abrir un PR?
   ```

3. El agente de QA empezará a trabajar.
4. Cuando termine, lee el informe.

## Resumen y siguientes pasos

Has añadido a tu flujo de trabajo una función especializada reutilizable y has revisado sus resultados. En esta lección:

- has explorado cómo un agente personalizado trabaja con instrucciones, habilidades y herramientas MCP;
- has creado y revisado un perfil reutilizable de control de calidad;
- has seleccionado el agente de QA y revisado sus conclusiones respecto a la incidencia del filtrado.

Ya tienes la implementación, la actualización de la habilidad, el perfil de QA, las pruebas y el informe de verificación listos para revisión. A continuación, [los reunirás en un PR de la funcionalidad y usarás Agent Merge][next-lesson].

## Recursos

- [Crear agentes personalizados para Copilot CLI][create-agents]
- [Configuración de agentes personalizados][agent-config]

[previous-lesson]: ../6-mcp-playwright/
[next-lesson]: ../8-create-pull-request/
[create-agents]: https://docs.github.com/copilot/how-tos/copilot-cli/customize-copilot/create-custom-agents-for-cli
[agent-config]: https://docs.github.com/copilot/reference/custom-agents-configuration
