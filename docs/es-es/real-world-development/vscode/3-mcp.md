---
title: "Ejercicio 3 - Probar la funcionalidad con el servidor MCP de Playwright"
authors:
  - geektrainer
lastUpdated: 2026-06-30
---

[← Lección anterior: Modo de agente][previous-lesson] · [Siguiente lección: Agentes personalizados →][next-lesson]

Acabas de crear la funcionalidad de filtrado con el modo de agente. Antes de abrir un pull request, debes confirmar que funciona en el navegador. En lugar de probar la aplicación manualmente, conectarás el **servidor MCP de Playwright** y pedirás a Copilot que use un navegador real para probar la funcionalidad. Después, publicarás la rama y abrirás el PR.

En este ejercicio:

- aprenderás qué es Model Context Protocol (MCP) y cómo los servidores MCP amplían Copilot con nuevas herramientas;
- añadirás el servidor MCP de Playwright al espacio de trabajo;
- pedirás a Copilot que lo use para probar manualmente la funcionalidad de filtrado en un navegador;
- publicarás la rama y abrirás un pull request para el trabajo de filtrado.

## ¿Qué es Model Context Protocol (MCP)?

El modo de agente es mucho más potente cuando puede acceder a recursos externos al editor. MCP permite que Copilot lo haga: es un estándar para que el agente se comunique con herramientas y servicios externos.

![Diagrama que muestra el funcionamiento del modo de agente y cómo interactúa con el contexto, el LLM y las herramientas, incluidas las que proporcionan los servidores MCP y las extensiones de VS Code](../../../_images/ex1-mcp-diagram.png)

[Model Context Protocol (MCP)](https://github.blog/ai-and-ml/llms/what-the-heck-is-mcp-and-why-is-everyone-talking-about-it/) permite que los agentes de IA se comuniquen con herramientas y servicios externos en tiempo real. Así pueden acceder a información actualizada mediante recursos y realizar acciones en tu nombre mediante herramientas.

Para acceder a estas herramientas y recursos, se usa un servidor MCP que actúa como puente entre el agente de IA y las herramientas y servicios externos. El servidor MCP gestiona la comunicación entre el agente y las herramientas externas, como las API existentes o herramientas locales como los paquetes de NPM. Cada servidor MCP proporciona un conjunto distinto de herramientas y recursos al agente.

Algunos servidores MCP populares:

- **[GitHub MCP Server](https://github.com/github/github-mcp-server):** proporciona acceso a un conjunto de API para gestionar repositorios de GitHub. Permite al agente realizar acciones como crear repositorios, actualizar los existentes y gestionar incidencias y pull requests.
- **[Playwright MCP Server](https://github.com/microsoft/playwright-mcp):** proporciona funciones de automatización del navegador con Playwright. Permite al agente realizar acciones como navegar por páginas web, rellenar formularios y seleccionar botones.

Hay muchos otros servidores MCP que proporcionan acceso a distintas herramientas y recursos. GitHub aloja un [registro MCP](https://github.com/mcp) para facilitar la búsqueda y la participación en el ecosistema.

> [!CAUTION]
> Por motivos de seguridad, trata los servidores MCP como cualquier otra dependencia del proyecto. Antes de usar uno, revisa su código fuente, comprueba quién lo publica y considera las implicaciones de seguridad. Usa únicamente servidores MCP de confianza y ten cuidado al conceder acceso a recursos u operaciones sensibles.

## Revisar la configuración de MCP

El archivo `.vscode/mcp.json` configura los servidores MCP disponibles en este espacio de trabajo de VS Code.

1. Abre `.vscode/mcp.json` en tu Codespace.
2. Deberías ver que ya hay un servidor `github` configurado:

    ```json
    {
      "servers": {
        "github": {
          "type": "http",
          "url": "https://api.githubcopilot.com/mcp/"
        }
      }
    }
    ```

La entrada `github` forma parte de la plantilla del proyecto. Por eso Copilot pudo leer el backlog de incidencias en el ejercicio anterior. Usa el [servidor MCP remoto de GitHub][remote-github-mcp-server], así que no hay que instalar nada localmente: VS Code se conecta por HTTP y te autenticas en GitHub la primera vez que Copilot usa una de sus herramientas.

## Añadir el servidor MCP de Playwright

Ahora añadirás un segundo servidor. El [servidor MCP de Playwright][playwright-mcp-server] proporciona a Copilot un navegador que puede controlar, justo lo que necesitas para probar la funcionalidad.

1. En `.vscode/mcp.json`, añade una entrada `playwright` junto a `github` para que el archivo quede así:

    ```json
    {
      "servers": {
        "github": {
          "type": "http",
          "url": "https://api.githubcopilot.com/mcp/"
        },
        "playwright": {
          "command": "npx",
          "args": ["@playwright/mcp@latest", "--headless"]
        }
      }
    }
    ```

2. Guarda el archivo.

A diferencia del servidor remoto `github`, `playwright` es un servidor **local**. VS Code lo inicia en tu equipo con `npx @playwright/mcp@latest`. La opción `--headless` indica a Playwright que ejecute el navegador sin mostrar una ventana, necesario en Codespaces, donde no hay un escritorio que pueda mostrarla.

> [!NOTE]
> El proyecto Tailspin Toys ya usa Playwright para las pruebas de extremo a extremo, así que normalmente el navegador que necesita Playwright ya está instalado. Si Copilot indica más adelante que falta un navegador, pídele que ejecute `npx playwright install chromium` y vuelva a intentarlo.

## Iniciar y confiar en el servidor

VS Code inicia los servidores MCP **cuando se necesitan**: la primera vez que Copilot requiere las herramientas de un servidor, VS Code lo inicia y te pregunta si confías en él.

1. En `.vscode/mcp.json`, aparecerá una acción **Start** sobre la entrada `playwright`. Selecciónala para iniciar el servidor y confirmar que la configuración es correcta.
2. Si VS Code te pregunta si confías en el servidor, revisa la configuración y confirma que confías en él para permitir que se inicie.

Iniciarlo manualmente es opcional. Si no lo haces, VS Code lo iniciará —y te pedirá que confirmes que confías en él— la primera vez que Copilot necesite sus herramientas en el paso siguiente.

## Probar la funcionalidad de filtrado

El [servidor MCP de Playwright][playwright-mcp-server] proporciona a Copilot un navegador real que puede controlar. En lugar de que tú pruebes la aplicación manualmente, el agente puede abrir una página, navegar por ella, aplicar filtros y leer los resultados. Después, resumirá lo que ha observado. Es una forma rápida de confirmar que una función se comporta como esperas, sin salir de la conversación.

El servidor MCP de Playwright se basa en el [árbol de accesibilidad][playwright-mcp-server] de la página, no en capturas de pantalla. Así, el agente trabaja con elementos estructurados y etiquetados (botones, enlaces y elementos de lista), como lo hace la tecnología de asistencia. Por tanto, una prueba funcional rápida también ofrece una comprobación básica de accesibilidad.

Con el servidor conectado y la aplicación en marcha, pide a Copilot que pruebe la funcionalidad que acabas de crear:

```text
Usa el servidor MCP de Playwright para abrir un navegador con la aplicación en marcha en http://localhost:4321 y comprobar la nueva funcionalidad de filtrado de juegos:

1. Ve a la página de juegos y anota cuántos aparecen.
2. Aplica un filtro de categoría y confirma que la lista solo muestra los juegos de esa categoría.
3. Borra el filtro y, después, aplica uno de editorial y confirma que la lista se actualiza para mostrar esa editorial.
4. Combina un filtro de categoría y uno de editorial y confirma que los resultados cumplen ambos.

Describe lo que observes en cada paso e indica cualquier comportamiento inesperado.
```

Copilot abrirá un navegador mediante el servidor MCP de Playwright, realizará cada paso y comunicará los resultados. Compara el resumen con los criterios de aceptación de la incidencia. Si algo no funciona como esperas, haz preguntas de seguimiento o pídele que corrija el código antes de abrir un pull request.

> [!NOTE]
> La aplicación debe estar en marcha en `http://localhost:4321` para poder hacer esta prueba. Si has detenido el servidor de desarrollo, vuelve a iniciarlo antes de enviar el prompt. La primera vez que Copilot use el servidor MCP de Playwright, puede que tenga que descargar un navegador. Si indica que falta uno, pídele que ejecute `npx playwright install chromium` y vuelva a intentarlo.

[playwright-mcp-server]: https://github.com/microsoft/playwright-mcp

## Publicar la rama y crear un pull request

Ahora que has confirmado que la función funciona, puedes abrir un pull request (PR) para que el equipo la revise. Primero publicarás la rama `filtering-vscode`.

1. Abre el panel **Source Control** del Codespace y revisa los cambios de Copilot.
2. Prepara los cambios seleccionando el icono **+**.
3. Genera un mensaje de confirmación con el botón **Sparkle**.

   ![Captura del panel Source Control con los cambios realizados](../../../_images/ex3-source-control-changes.png)

4. Selecciona **Publish** para enviar la rama al repositorio.

## Crear el pull request

Hay varias formas de crear un pull request: por ejemplo, en github.com o con la interfaz de línea de comandos (CLI) de GitHub. Como ya estás trabajando con GitHub Copilot, pídele que cree el PR. Puede encontrar la incidencia correspondiente y vincularla al pull request.

1. Ve al panel de Copilot Chat y selecciona **New Chat** para iniciar una sesión nueva.
2. Confirma que **Agent** esté seleccionado en el selector de agentes para que Copilot pueda usar las herramientas de GitHub.
3. Pide a Copilot que cree un PR:

   ```plaintext
   Busca en el repositorio la incidencia relacionada con el filtrado por categoría y editorial. Crea un pull request para la rama actual y relaciónalo con la incidencia correspondiente.
   ```

4. Cuando sea necesario, selecciona **Continue** para permitir que Copilot obtenga información y realice operaciones. La primera vez que use una herramienta de GitHub, es posible que tengas que autenticarte; sigue las instrucciones para hacerlo.
5. Observa cómo Copilot busca entre las incidencias, encuentra la correcta y crea el PR.
6. Selecciona el enlace que genera Copilot para revisar el pull request, pero **no lo fusiones todavía**.

## Resumen y siguientes pasos

Enhorabuena. En este ejercicio:

- has aprendido qué es Model Context Protocol (MCP) y cómo los servidores MCP amplían Copilot con nuevas herramientas;
- has añadido el servidor MCP de Playwright al espacio de trabajo;
- lo has usado para probar manualmente la funcionalidad de filtrado en un navegador antes de publicarla;
- has publicado la rama y abierto un pull request para el trabajo de filtrado.

Has usado un servidor MCP para *probar* una función, pero MCP es solo una de las formas de ampliar Copilot. A continuación, vamos a [crear un agente personalizado][next-lesson] para agilizar tareas concretas, como las revisiones de accesibilidad.

## Recursos

- [Qué es MCP y por qué todo el mundo habla de ello][mcp-blog-post]
- [Servidor MCP de Microsoft Playwright][playwright-mcp-server]
- [Servidor MCP de GitHub][github-mcp-server]
- [Registro MCP de GitHub][mcp-registry]
- [Servidores MCP en VS Code][vscode-mcp-config]

[previous-lesson]: ../2-agent-mode/
[next-lesson]: ../4-custom-agents/
[mcp-blog-post]: https://github.blog/ai-and-ml/llms/what-the-heck-is-mcp-and-why-is-everyone-talking-about-it/
[github-mcp-server]: https://github.com/github/github-mcp-server
[mcp-registry]: https://github.com/mcp
[remote-github-mcp-server]: https://github.blog/changelog/2025-06-12-remote-github-mcp-server-is-now-available-in-public-preview/
[vscode-mcp-config]: https://code.visualstudio.com/docs/agents/reference/mcp-configuration
