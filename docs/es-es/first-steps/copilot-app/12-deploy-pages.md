---
title: "Lección 12 - Desplegar el sitio con GitHub Pages"
description: "Usa la aplicación GitHub Copilot para configurar un flujo de trabajo de Pages, revisar su pull request y comprobar que Space Quiz está disponible."
authors:
  - jamesmontemagno
lastUpdated: 2026-10-12
---

Publica Space Quiz para que cualquiera pueda usarlo con GitHub Pages. Copilot puede preparar el flujo de trabajo y guiarte durante el despliegue, pero tú eres responsable de revisar los cambios y confirmar que el sitio está disponible.

En esta lección:

- pedirás a Copilot que configure un despliegue en GitHub Pages.
- revisarás el flujo de trabajo y el pull request antes de fusionarlo.
- seguirás el despliegue y comprobarás que el sitio publicado funciona.

## Escenario

Space Quiz ya está en un repositorio de GitHub, pero la gente necesita una forma sencilla de probarlo. Publicar el sitio, que consta de un solo archivo, con GitHub Pages permite acceder a él mediante una URL pública sin añadir un servidor ni una dependencia de runtime.

## Pedir a Copilot que despliegue el sitio

En la sesión de la aplicación Copilot para `space-quiz`, envía este prompt:

```plaintext
Habilita GitHub Pages para este repositorio. Crea un flujo de trabajo de GitHub Actions que compile el sitio web si es necesario y lo despliegue en GitHub Pages. Añade un README que describa la aplicación y explique cómo ejecutarla localmente, si aún no existe. Crea un pull request con estos cambios y fusiónalo cuando se superen las comprobaciones obligatorias. Supervisa el despliegue hasta que termine correctamente y, después, indícame la URL definitiva del sitio web publicado. Si no tienes permiso para habilitar Pages o fusionar, dime exactamente qué debo aprobar o hacer; no indiques que ha terminado correctamente hasta que el sitio esté disponible.
```

![Diagrama en tres fases: un pull request de space-quiz fusionado; una ejecución correcta de GitHub Actions llamada Deploy website, con los pasos Build website, Upload artifact y Deploy to Pages; y Space Quiz disponible en yourname.github.io/space-quiz/.](../../../_images/first-steps-app-pages-deploy.svg)

> [!WARNING]
> Este prompt pide al agente que cree y fusione un pull request y cambie la configuración de Pages del repositorio. Revisa la diferencia, los permisos de despliegue y las solicitudes de aprobación de GitHub antes de continuar.

## Revisar el flujo de trabajo y comprobar el despliegue

1. Confirma que el README solo se añade si el repositorio todavía no tiene uno.
2. Revisa los cambios del pull request, incluidos los pasos de compilación y despliegue del flujo de trabajo.
3. Fusiona el pull request solo cuando se superen las comprobaciones obligatorias.
4. Sigue la ejecución de despliegue de GitHub Actions hasta que termine correctamente.
5. Abre la URL indicada y confirma que Space Quiz se carga.

Si el agente no puede habilitar Pages ni fusionar el pull request, sigue las instrucciones específicas que te proporcione. No des la tarea por terminada hasta que el despliegue finalice correctamente y el sitio esté disponible.

## Resumen y siguientes pasos

Space Quiz ya está disponible como sitio web y has comprobado el despliegue en lugar de confiar solo en el informe del agente. Continúa con la [Lección 13: Revisión y siguientes pasos][next-lesson].

[next-lesson]: ../13-review/
