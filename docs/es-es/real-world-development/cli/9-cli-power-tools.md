---
title: "Lección 9 - Comandos de barra en GitHub Copilot CLI"
description: "Explora comandos de barra para gestionar el contexto, seleccionar modelos, compartir sesiones y delegar tareas opcionalmente en cloud agent."
authors:
  - geektrainer
lastUpdated: 2026-09-18
---

Como cualquier buena herramienta de CLI, GitHub Copilot CLI incluye numerosos comandos de barra para interactuar con ella. Estos comandos permiten acceder a funciones avanzadas, consultar información interna y cambiar la configuración. Ya has usado comandos como `/diff`, `/mcp`, `/skills`, `/agent` y `/pr`. Vamos a explorar algunos otros que pueden ser útiles.

En esta lección:

- usarás `/context` y `/compact` para explorar cómo gestiona Copilot el contexto de la conversación;
- usarás `/model` para explorar los modelos disponibles;
- aprenderás cómo `/share` puede exportar o compartir una sesión;
- explorarás comandos opcionales para trabajar en paralelo, usar worktrees y delegar tareas en cloud agent.

## Escenario

Has completado el flujo de trabajo principal de CLI. Ahora veremos otras funciones: gestionar el contexto, cambiar de modelo, compartir sesiones y, de forma opcional, delegar trabajo a [Copilot cloud agent][about-cloud-agent].

## Explorar el contexto de Copilot CLI

En tareas grandes o complejas, puede que alcances el límite de la ventana de contexto del modelo. Copilot CLI compacta automáticamente la conversación cuando es necesario. También puedes inspeccionarla o compactarla con comandos de barra.

1. Vuelve a tu Codespace e inicia Copilot CLI desde la raíz del repositorio si aún no está abierto.
2. Escribe:

   ```plaintext
   /context
   ```

3. Observa el modelo, el uso actual de tokens y la distribución del contexto entre las instrucciones del sistema, las herramientas, los mensajes y el espacio disponible.
4. Compacta la conversación:

   ```plaintext
   /compact
   ```

5. Vuelve a escribir `/context` y compara el resultado. Si la conversación ya es breve, puede que no observes un cambio importante.

> [!NOTE]
> Copilot CLI compacta automáticamente el contexto cuando la ventana se llena. Usa `/compact` para elegir cuándo hacerlo. Usa `/clear` o `/new` cuando vayas a cambiar a una tarea distinta y quieras empezar una conversación nueva.

## Elegir un modelo

Los modelos tienen distintas ventajas, y cada desarrollador puede tener sus propias preferencias. Copilot CLI permite enumerar y seleccionar el modelo que quieras usar.

1. Escribe:

   ```plaintext
   /model
   ```

2. Explora los modelos y la información de uso disponibles.
3. Mantén el modelo actual, selecciona otro o pulsa <kbd>Esc</kbd> para cerrar la lista.

## Compartir una sesión

Colaborar y compartir lo aprendido ayuda a todo el equipo a mejorar el uso de las herramientas de IA. El comando `/share` puede exportar una sesión a un archivo Markdown o HTML, crear un enlace para compartir o publicar un GitHub gist.

1. Escribe `/help` y revisa las opciones de `/share` disponibles en tu versión.
2. Si quieres compartir la sesión, elige el destino adecuado. Por ejemplo, `/share file` crea una exportación Markdown local.
3. Revisa el contenido exportado antes de enviarlo o publicarlo. Las exportaciones de sesiones pueden incluir prompts, respuestas y detalles del proyecto.

Publicar un enlace o un gist es opcional. No publiques contenido del repositorio o de una conversación que el equipo no tenga previsto compartir.

## Opcional: ampliar o delegar el trabajo

Has completado el taller principal. Copilot CLI también ofrece comandos para tareas más grandes:

- `/fleet` puede dividir una tarea en subtareas independientes, asignarlas a subagentes y ejecutarlas en paralelo.
- `/worktree` puede crear un worktree de Git aislado para otra tarea.
- `/delegate` puede enviar una tarea a Copilot cloud agent, que trabaja de forma asíncrona y puede abrir un pull request.

Estos comandos son opcionales, ya que pueden crear worktrees adicionales o trabajo remoto. Antes de probarlos, inicia una tarea nueva con un alcance claro y revisa el resultado con el flujo de trabajo habitual. Si quieres profundizar en el trabajo de agentes asíncronos, continúa con el [taller de cloud agent][cloud-workshop].

## Resumen y siguientes pasos

Los comandos de barra de Copilot CLI permiten configurarlo, compartir sesiones y consultar lo que ocurre entre bastidores. En esta lección:

- has usado `/context` y `/compact` para explorar cómo gestiona Copilot el contexto de la conversación;
- has usado `/model` para explorar los modelos disponibles;
- has aprendido cómo `/share` puede exportar o compartir una sesión;
- has explorado comandos opcionales para trabajar en paralelo, usar worktrees y delegar tareas en cloud agent.

Hay más comandos de barra y funciones de Copilot CLI que puedes explorar. Vamos a cerrar el recorrido [repasando lo que hemos aprendido][next-lesson] y algunos pasos para seguir aprendiendo.

## Recursos

- [Referencia de comandos de Copilot CLI][cli-reference]
- [Gestión del contexto en Copilot CLI][context-management]
- [Acerca de Copilot cloud agent][about-cloud-agent]

[previous-lesson]: ../8-create-pull-request/
[next-lesson]: ../10-review/
[cli-reference]: https://docs.github.com/copilot/reference/copilot-cli-reference/cli-command-reference
[context-management]: https://docs.github.com/copilot/concepts/agents/copilot-cli/context-management
[about-cloud-agent]: https://docs.github.com/copilot/concepts/agents/cloud-agent/about-cloud-agent
[cloud-workshop]: ../../cloud/
