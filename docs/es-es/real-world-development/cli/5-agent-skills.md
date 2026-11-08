---
title: "Lección 5 - Personalizar y usar una habilidad quality-checks"
description: "Explora la habilidad existente quality-checks, personaliza el formato de su informe y úsala para validar el filtrado."
authors:
  - geektrainer
lastUpdated: 2026-09-29
---

Programar no consiste solo en escribir código. Ya hemos podido comprobar manualmente que el código funciona y hemos usado archivos de instrucciones para que siga nuestros estándares. Pero ¿qué pasa con las pruebas, lint y los demás aspectos de la integración continua (CI)?

Las **habilidades de agente** son adecuadas para estas tareas. Ayudan a Copilot a entender cómo debe ejecutar operaciones como estas.

En esta lección:

- explorarás la habilidad existente `quality-checks`;
- personalizarás el formato de sus resultados;
- volverás a cargar la habilidad y la ejecutarás.

## Escenario

Tailspin Toys usa la habilidad `quality-checks` para ejecutar pruebas unitarias, lint y comprobaciones de tipos. El equipo quiere mejorar el informe para que los resultados sean más fáciles de leer.

## Instrucciones, scripts y recursos

Las habilidades de agente reúnen instrucciones reutilizables para tareas, scripts ejecutables y recursos de apoyo que un agente carga cuando los necesita. En esencia, una habilidad es una carpeta con su nombre y un archivo Markdown llamado `SKILL.md`. El archivo Markdown incluye un frontmatter con el nombre y la descripción de la habilidad, una descripción general de su función y directrices sobre cuándo usarla. La carpeta también puede incluir subcarpetas con scripts y otros recursos que la habilidad necesite.

> [!NOTE]
> Una habilidad no necesita archivos ni carpetas adicionales. La habilidad `quality-checks` de Tailspin Toys solo tiene un archivo `SKILL.md`, porque usa los comandos existentes del proyecto.

Las habilidades pueden estar en la carpeta `.github/skills` del proyecto para compartirse y reutilizarse en el equipo, o en la carpeta de habilidades del usuario, `~/.copilot/skills`.

## Explorar la habilidad

Vamos a explorar la habilidad `quality-checks` que creó el equipo de Tailspin Toys para ejecutar pruebas unitarias, lint y comprobaciones de tipos.

1. Vuelve a tu Codespace. En el editor de Codespaces, abre `.github/skills/quality-checks/SKILL.md`.
2. Lee el `name` y la `description` de la parte superior. La descripción ayuda a Copilot a saber cuándo debe usar la habilidad.
3. Lee las instrucciones y observa cómo guían a Copilot para ejecutar las pruebas y lint.
4. Observa que la habilidad todavía no tiene una sección **Results output formatting**.

## Ejecutar la habilidad antes de cambiarla

Las habilidades se pueden invocar directamente desde Copilot CLI o mediante lenguaje natural. Pide a Copilot que ejecute las tres comprobaciones de la habilidad.

1. Vuelve a la conversación de filtrado en modo Interactive.
2. Envía este prompt:

   ```plaintext
   Ejecuta la habilidad quality-checks para las pruebas unitarias, lint y las comprobaciones de tipos.
   ```

3. Observa el informe final.

## Personalizar el informe

Queremos un informe mejor que indique qué se ha ejecutado, si ha terminado correctamente y qué resultados han devuelto las herramientas. Vamos a actualizar la habilidad para generarlo.

1. Vuelve a `.github/skills/quality-checks/SKILL.md`.
2. Añade al final del archivo esta sección:

   ```markdown
   ## Results output formatting

   Upon completion, report each command that ran and whether it passed, failed, or was blocked. Include test counts, durations, errors, warnings, and other metrics only when the tool reports them. Identify the next action for any failure or blocker, and never describe a skipped or incomplete check as passed.
   ```

3. El archivo se guarda automáticamente.

## Ejecutar la habilidad actualizada

Ya has hecho el cambio. Vamos a probarlo. Copilot CLI puede volver a cargar las habilidades modificadas sin reiniciar la conversación.

1. Escribe:

   ```plaintext
   /skills reload
   ```

2. Envía exactamente el mismo prompt de antes:

   ```plaintext
   Ejecuta la habilidad quality-checks para las pruebas unitarias, lint y las comprobaciones de tipos.
   ```

3. Observa el informe final y compáralo con el primero.

## Resumen y siguientes pasos

Has personalizado y usado una habilidad de agente existente. En esta lección:

- has explorado la habilidad `quality-checks` para las pruebas unitarias, lint y las comprobaciones de tipos;
- has personalizado el formato de sus resultados;
- has vuelto a cargar y ejecutado la habilidad.

Este cambio acompañará al filtrado en el PR de la funcionalidad. A continuación, permitirás que Copilot interactúe directamente con el sitio [mediante el servidor MCP de Playwright][next-lesson].

## Más ejemplos de habilidades

Estos ejemplos de la comunidad sirven de referencia, no son tareas adicionales:

- [Especificación de Agent Skills][skill-spec]
- [Flujo de trabajo de contribución: `make-repo-contribution`][contribution-example]
- [Documentos de requisitos: `prd`][prd-example]
- [Diagramas y script de exportación incluido: `drawio`][drawio-example]
- [Pruebas del navegador: `webapp-testing`][browser-example]

[previous-lesson]: ../4-custom-instructions/
[next-lesson]: ../6-mcp-playwright/
[skill-spec]: https://agentskills.io/specification
[contribution-example]: https://github.com/github/awesome-copilot/tree/main/skills/make-repo-contribution
[prd-example]: https://github.com/github/awesome-copilot/tree/main/skills/prd
[drawio-example]: https://github.com/github/awesome-copilot/tree/main/skills/drawio
[browser-example]: https://github.com/github/awesome-copilot/tree/main/skills/webapp-testing
