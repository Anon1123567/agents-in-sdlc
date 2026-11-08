---
title: "Lección 8 - Crear y fusionar el PR de la funcionalidad"
description: "Revisa conjuntamente el filtrado, las instrucciones, la actualización de la habilidad, el perfil de QA y las pruebas; después, crea un PR y usa Agent Merge."
authors:
  - geektrainer
lastUpdated: 2026-09-18
---

La implementación del filtrado, las actualizaciones de instrucciones, la actualización de la habilidad, el perfil de control de calidad (QA) y las pruebas están guardados en una misma rama. Es el momento de revisarlos conjuntamente y abrir un pull request. Fusionaste tú mismo el pull request (PR) de la valoración por estrellas; esta vez dejarás que **Agent Merge** gestione el proceso.

> [!NOTE]
> Normalmente, dividiríamos la funcionalidad, las actualizaciones de instrucciones, la actualización de la habilidad y el agente de QA en varios PR. Para agilizar el taller, has mantenido todo el flujo de trabajo de filtrado y calidad en una conversación y una rama, y todo ese trabajo se incluirá en este PR.

En esta lección:

- aprenderás qué es Agent Merge y cómo automatiza el ciclo de vida de la fusión.
- inspeccionarás todos los cambios de la funcionalidad y las pruebas de verificación.
- crearás el PR del filtrado.
- habilitarás Agent Merge solo después de la revisión y confirmarás que el PR se haya fusionado.

## Escenario

Durante el flujo de trabajo del filtrado, has usado Copilot para planificar, implementar y verificar una funcionalidad. Tailspin Toys quiere ahora automatizar el trabajo restante del PR, pero mantener bajo el control del desarrollador la autorización para fusionarlo.

## Presentación de Agent Merge

**Agent Merge** automatiza el trabajo restante necesario para integrar un pull request. Al habilitarlo, Copilot resuelve los bloqueos del PR —corrige comprobaciones de integración continua (CI) fallidas, responde a comentarios de revisión y vuelve a basar la rama cuando es necesario— y, después, habilita la fusión automática de GitHub cuando el repositorio lo permite.

Hasta ahora has seleccionado tú **Merge pull request**. Agent Merge puede encargarse de esa responsabilidad. Revisa el trabajo y decide si está listo antes de habilitar Agent Merge.

## Usar Agent Merge para gestionar el PR

Ya has creado todo el código. Vamos a revisarlo, crear el PR y dejar que Agent Merge gestione el resto del proceso.

1. Vuelve a tu Codespace.
2. Abre el diálogo del agente escribiendo `/agent`.
3. Selecciona **Default** en la lista de opciones y pulsa <kbd>Enter</kbd>.
4. Crea un PR con el comando `/pr create`.
5. Activa Agent Merge con `/pr agentmerge`.
6. Copilot supervisará el proceso de integración continua del PR. Cuando todo termine correctamente, lo fusionará.
7. Comprueba que Copilot muestre un mensaje similar a «El PR #14 se ha fusionado correctamente mediante squash».

> [!IMPORTANT]
> Agent Merge no omite las aprobaciones obligatorias, la protección de ramas, las colas de fusión, la configuración del repositorio ni los permisos que falten. Si se bloquea, lee el motivo indicado y completa manualmente la fusión revisada cuando el repositorio lo permita.

## Resumen y siguientes pasos

Has automatizado varias partes del proceso de desarrollo: la generación de código, las pruebas y la validación del código, y ahora el proceso del pull request. En concreto:

- has aprendido qué es Agent Merge y cómo automatiza el ciclo de vida de la fusión.
- has inspeccionado todos los cambios de la funcionalidad y las pruebas de verificación.
- has creado el PR del filtrado.
- has habilitado Agent Merge solo después de la revisión y has confirmado que el PR se ha fusionado.

A continuación, [explorarás otros comandos de barra útiles de Copilot CLI][next-lesson] para gestionar el contexto, los modelos, el uso compartido y la delegación opcional en cloud.

## Recursos

- [Gestionar pull requests con Copilot CLI][manage-prs]
- [Referencia de comandos de Copilot CLI][cli-reference]

[previous-lesson]: ../7-qa-agent/
[next-lesson]: ../9-cli-power-tools/
[manage-prs]: https://docs.github.com/copilot/how-tos/copilot-cli/use-copilot-cli/manage-pull-requests
[cli-reference]: https://docs.github.com/copilot/reference/copilot-cli-reference/cli-command-reference
