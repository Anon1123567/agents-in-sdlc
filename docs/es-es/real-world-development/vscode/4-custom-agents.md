---
title: "Ejercicio 4 - Agentes personalizados"
authors:
  - geektrainer
lastUpdated: 2026-06-30
---

## ¿Qué son los agentes personalizados?

Los [agentes personalizados][custom-agents-concept] de GitHub Copilot te permiten crear asistentes de IA especializados para determinadas tareas o áreas del flujo de trabajo de desarrollo. Al definir agentes mediante archivos Markdown en la carpeta `.github/agents` del repositorio, puedes proporcionar instrucciones, buenas prácticas, patrones de programación y conocimientos específicos que ayuden a Copilot a realizar ciertos tipos de trabajo con mayor eficacia. Los equipos pueden plasmar su experiencia en agentes reutilizables: uno de accesibilidad que aplique los requisitos de [WCAG][wcag], uno de seguridad que siga prácticas de programación segura o uno de pruebas que mantenga patrones coherentes.

Los agentes personalizados se definen en archivos Markdown en la carpeta `.github/agents` del proyecto o, de forma global, en `~/.copilot/agents`. Cada archivo tiene un frontmatter YAML que incluye al menos `name` y `description`, seguido de un prompt Markdown que define el comportamiento, la experiencia y las instrucciones del agente.

### Diferencias entre agentes personalizados y habilidades de agente

Hay cierto solapamiento entre los agentes personalizados y las [habilidades de agente][agent-skills-concept]. Ambos se definen principalmente mediante archivos Markdown que explican a una IA cómo realizar ciertas operaciones. La forma más sencilla de distinguirlos es esta: el **agente personalizado** es quien realiza el trabajo, y las **habilidades** son las herramientas.

Los agentes personalizados tienen su propia ventana de contexto y están diseñados para coordinar habilidades y otros agentes como parte de su trabajo. En este taller, el agente personalizado de accesibilidad revisará el sitio y lo actualizará de acuerdo con las directrices de accesibilidad. Para ello, podría recurrir a habilidades como un flujo de trabajo para pull requests o una que ejecute y gestione pruebas.

> [!NOTE]
> No hay una única forma correcta de crear un agente personalizado. Como ocurre con cualquier herramienta de IA, prueba distintas opciones e itera para encontrar las que funcionen en cada entorno y situación.

[custom-agents-concept]: https://docs.github.com/copilot/concepts/agents/cloud-agent/about-custom-agents
[agent-skills-concept]: https://docs.github.com/copilot/concepts/agents/about-agent-skills
[wcag]: https://www.w3.org/WAI/standards-guidelines/wcag/

## Escenario

Tailspin Toys se compromete a que todas las personas puedan usar su plataforma de financiación colectiva, independientemente de sus capacidades visuales o preferencias. Los comentarios recientes han indicado que a algunas personas les cuesta leer el tema oscuro actual porque el contraste entre el texto y el fondo es insuficiente. Para resolver este problema de accesibilidad, el equipo de diseño ha solicitado un modo de alto contraste que se pueda activar y desactivar.

Como la accesibilidad es fundamental, quieres implementar esta función lo antes posible. Para generar la funcionalidad, usarás un agente personalizado.

En este ejercicio:

- revisarás un agente personalizado de accesibilidad;
- usarás ese agente en Copilot Chat para implementar un modo de alto contraste.

## Revisar el agente personalizado de accesibilidad

Ya se ha creado un agente personalizado de accesibilidad. Vamos a revisar su contenido para entender cómo guiará a Copilot.

Vuelve a tu Codespace y abre un terminal. Crea una rama nueva a partir de `main` para el trabajo de accesibilidad (mantendrás separado el PR de filtrado del Ejercicio 3):

```bash
git checkout main
git pull
git checkout -b accessibility-vscode
```

1. Abre `.github/agents/accessibility.md`.
2. Fíjate en el frontmatter YAML con los campos `name` y `description`.

> [!CAUTION]
> El frontmatter con `name` y `description` es obligatorio para los agentes personalizados.

3. Revisa las secciones siguientes, que describen:
   - las responsabilidades principales al generar código para un sitio web accesible;
   - las buenas prácticas de accesibilidad;
   - ejemplos de código HTML, CSS y JavaScript;
   - errores y problemas frecuentes.

## Usar el agente personalizado en Copilot Chat

VS Code muestra en el selector de agentes situado en la parte inferior de la vista Copilot Chat todos los agentes personalizados definidos en `.github/agents`. Puedes seleccionar uno para adaptar la sesión de chat a sus instrucciones y herramientas.

> [!TIP]
> **Abrir Copilot Chat**
>
> Antes de empezar los ejercicios siguientes, vuelve a tu Codespace, abre el panel de Copilot Chat y selecciona **New Chat** para iniciar una conversación nueva. El modo y el modelo varían según el ejercicio; cada paso lo indicará cuando sea pertinente.

1. Selecciona **Agent** en el selector de agentes de la vista Chat, si aún no está seleccionado.

   ![Captura del selector de agente en la vista Chat.](../../../_images/shared-chat-mode-selector.png)

2. Selecciona el selector de agentes en la parte inferior de la vista Chat. Muestra el agente activo, que de forma predeterminada es **default**.
3. Selecciona **Accessibility agent** en la lista de agentes disponibles.
4. Envía este prompt al agente de accesibilidad:

    ```plaintext
    Añade un modo de alto contraste al sitio. Debe haber un control para que el usuario pueda activar el modo y la selección debe persistir entre recargas de página usando el almacenamiento local del navegador.
    ```

5. Copilot Chat empezará a trabajar: explorará el código base, propondrá cambios y editará los archivos del proyecto. Cada edición aparecerá en el chat con la ruta del archivo y una diferencia que podrás revisar.
6. A medida que se apliquen las ediciones, se actualizará el indicador **Files changed** para mostrar los archivos modificados por el agente.

> [!NOTE]
> El proceso puede tardar unos minutos. Copilot hará cambios reales en el repositorio y editará archivos existentes, como los componentes Astro, el CSS y las pruebas relacionadas.

En el ejercicio siguiente revisarás y dirigirás el trabajo en curso.

## Resumen y siguientes pasos

En esta lección has explorado los [agentes personalizados][custom-agents] de GitHub Copilot, asistentes de IA especializados en determinadas tareas y áreas. Puedes plasmar la experiencia y los estándares del equipo en agentes reutilizables que guíen a Copilot.

Has explorado estos conceptos:

- cómo se definen los agentes personalizados;
- cómo usar un agente personalizado en el modo de agente de Copilot Chat.

A continuación, [supervisarás y dirigirás el trabajo del agente][next-lesson], revisarás los cambios mientras se realizan y añadirás un control de modo claro a la misma sesión.

## Recursos

- [Acerca de los agentes personalizados][custom-agents]
- [Crear agentes personalizados en el IDE][creating-custom-agents-ide]
- [Agentes personalizados en VS Code][custom-agents-vscode]
- [Configuración de agentes personalizados][custom-agents-config]
- [Agentes personalizados de awesome-copilot][awesome-copilot-agents]

---

| [← Lección anterior: Probar la funcionalidad con el servidor MCP de Playwright][previous-lesson] | [Siguiente lección: Supervisar y gestionar agentes →][next-lesson] |
|:--|--:|

[previous-lesson]: ../3-mcp/
[next-lesson]: ../5-managing-agents/
[custom-agents]: https://docs.github.com/copilot/concepts/agents/cloud-agent/about-custom-agents
[creating-custom-agents-ide]: https://docs.github.com/copilot/how-tos/use-copilot-agents/cloud-agent/create-custom-agents-in-your-ide
[custom-agents-vscode]: https://code.visualstudio.com/docs/copilot/customization/custom-agents
[custom-agents-config]: https://docs.github.com/copilot/reference/custom-agents-configuration
[awesome-copilot-agents]: https://github.com/github/awesome-copilot/tree/main/agents
