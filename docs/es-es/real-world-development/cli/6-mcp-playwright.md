---
title: "Lección 6 - Validar la funcionalidad con Playwright MCP"
description: "Inspecciona o configura Playwright MCP en Copilot CLI y úsalo para explorar el filtrado en un navegador."
authors:
  - geektrainer
lastUpdated: 2026-09-18
---

Como ya hemos indicado, programar no consiste solo en escribir código. También hay que trabajar con datos y servicios externos, y permitir que Copilot use automatizaciones adicionales. Aquí entran en juego los servidores MCP. Permiten que Copilot vaya más allá de las funciones integradas en la CLI y acceda a más herramientas y servicios.

En esta lección:

- aprenderás qué es Model Context Protocol (MCP) y cómo lo usa Copilot CLI;
- añadirás el servidor MCP de Playwright si aún no está disponible;
- pedirás al agente que controle un navegador y explore la función de filtrado.

## Escenario

Las pruebas unitarias y de extremo a extremo son importantes, pero para validar cambios en la interfaz hay que interactuar con ella. Quieres que Copilot pueda usar el sitio web como lo haría una persona y automatizar aún más el proceso de cambios, para tener más confianza en que las actualizaciones funcionan como esperas.

## ¿Qué es Model Context Protocol (MCP)?

[Model Context Protocol (MCP)][mcp-blog-post] permite que los agentes de IA se comuniquen con herramientas y servicios externos en tiempo real. Así, pueden acceder a información actualizada y realizar acciones en tu nombre.

Para acceder a estas herramientas y recursos, se usa un servidor MCP que actúa como puente entre el agente de IA y las herramientas y servicios externos. Cada servidor MCP proporciona un conjunto distinto de herramientas y recursos al agente.

Algunos servidores MCP populares:

- **[GitHub MCP Server][github-mcp]:** proporciona acceso a API para gestionar repositorios, incidencias y pull requests de GitHub.
- **[Playwright MCP Server][playwright-mcp-server]:** proporciona funciones de automatización del navegador con Playwright.

Hay muchos otros servidores MCP disponibles. GitHub aloja un [registro MCP][mcp-registry] para facilitar la búsqueda y la participación en el ecosistema.

> [!CAUTION]
> Trata los servidores MCP como cualquier otra dependencia del proyecto. Antes de usar uno, revisa su código fuente, comprueba quién lo publica y considera las implicaciones de seguridad.

## Añadir el servidor MCP de Playwright

Si el servidor MCP de Playwright no está disponible, añádelo para que Copilot pueda interactuar con el sitio web como lo haría una persona.

> [!NOTE]
> Si Playwright MCP ya está configurado en Copilot CLI, omite estos pasos y continúa con la sección siguiente.

1. Vuelve a tu Codespace.
2. En Copilot CLI, abre el cuadro de diálogo para añadir un servidor MCP escribiendo:

   ```plaintext
   /mcp add
   ```

3. Escribe `playwright` como nombre y, después, pulsa <kbd>Tab</kbd>.
4. Confirma que el tipo de servidor sea **STDIO** pulsando <kbd>Enter</kbd> y, después, pulsa <kbd>Tab</kbd>.
5. Pega lo siguiente en el cuadro de diálogo **Command**:

   ```plaintext
   npx -y @playwright/mcp@latest --headless --no-sandbox
   ```

6. Pulsa <kbd>Control</kbd>+<kbd>S</kbd> (Mac) o <kbd>Ctrl</kbd>+<kbd>S</kbd> (Windows/Linux) para guardar el servidor MCP.
7. Pulsa <kbd>Esc</kbd> para salir del cuadro de diálogo de MCP.

## Pedir a Copilot que explore la función con Playwright

Antes has confirmado manualmente que la funcionalidad se comporta como esperas. Ahora vamos a usar el servidor MCP de Playwright disponible en Copilot para repetir la prueba.

1. Envía este prompt para pedir a Copilot que use el servidor MCP de Playwright y valide la funcionalidad:

   ```plaintext
   Inicia la aplicación y usa Playwright MCP para comprobar el filtrado según la incidencia y el plan. Dime qué funciona y qué no, sin hacer cambios. Detén el servidor que has iniciado cuando termines.
   ```

> [!NOTE]
> En realidad, no hace falta indicar a Copilot que use el servidor MCP: normalmente lo descubrirá por sí solo. Sin embargo, como sabes qué herramienta debe usar, es buena idea ayudarle a elegirla. Así los resultados serán más coherentes y ahorrarás algunos tokens.

2. Observa cómo Copilot enumera las distintas operaciones que realiza en el navegador para comprobar que la funcionalidad funciona.
3. Lee el informe y confirma que todo se comporte como esperas.

Copilot iniciará el servidor, usará Playwright para interactuar con el sitio web, detendrá el servidor y te mostrará un informe.

## Resumen y siguientes pasos

Enhorabuena. Has usado el servidor MCP de Playwright para explorar la funcionalidad en un navegador real desde Copilot CLI. En concreto:

- has aprendido qué es Model Context Protocol (MCP) y cómo lo usa Copilot CLI;
- has añadido el servidor MCP de Playwright si todavía no estaba disponible;
- has pedido al agente que controle un navegador y explore la función de filtrado.

A continuación, [crearás un agente personalizado de QA][next-lesson] que combinará la habilidad y las herramientas del navegador en una función especializada.

## Recursos

- [Qué es MCP y por qué todo el mundo habla de ello][mcp-blog-post]
- [Servidor MCP de Microsoft Playwright][playwright-mcp-server]
- [Añadir servidores MCP a Copilot CLI][add-mcp]

[previous-lesson]: ../5-agent-skills/
[next-lesson]: ../7-qa-agent/
[mcp-blog-post]: https://github.blog/ai-and-ml/llms/what-the-heck-is-mcp-and-why-is-everyone-talking-about-it/
[playwright-mcp-server]: https://github.com/microsoft/playwright-mcp
[github-mcp]: https://github.com/github/github-mcp-server
[mcp-registry]: https://github.com/mcp
[add-mcp]: https://docs.github.com/copilot/how-tos/copilot-cli/customize-copilot/add-mcp-servers
