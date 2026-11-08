---
title: "Ejercicio 2 - GitHub Copilot cloud agent"
authors:
  - geektrainer
lastUpdated: 2026-06-30
---

| [← Lección anterior: Instrucciones personalizadas][previous-lesson] |
|:--|

Es probable que muy pocas organizaciones —si es que hay alguna— no tengan problemas con la deuda técnica. Puede tratarse de problemas de seguridad sin resolver, código heredado que necesita actualizarse o solicitudes de funcionalidades que llevan tiempo en el backlog porque no ha habido tiempo para implementarlas. GitHub Copilot cloud agent está diseñado para tareas como actualizar código y añadir funcionalidades de forma autónoma. Cuando termina, crea un PR en borrador para que lo revise un desarrollador. Así, puedes delegar tareas tediosas, acelerar el desarrollo y dedicarte a cuestiones de mayor alcance.

Con Copilot cloud agent explorarás cómo:

- personalizar el entorno de generación de código;
- garantizar que las operaciones se realicen de forma segura;
- definir incidencias con un alcance claro;
- asignar incidencias a Copilot.

## Escenarios

Tailspin Toys tiene deuda técnica que quiere resolver. Los contratistas que crearon la primera versión del sitio dejaron la documentación en un estado poco adecuado: verás que no hay ninguna. Como primer paso, quieren añadir comentarios de documentación TSDoc a todas las funciones exportadas de la aplicación.

Además, el equipo de diseño quiere mejorar la exploración de juegos. Quiere que la página de detalles de cada juego muestre juegos relacionados —otros títulos de la misma categoría— para que la gente pueda seguir explorando. Todavía no necesita una implementación pulida: solo algo que pueda usar para las pruebas de aceptación de la experiencia. Este trabajo es un bloqueo, aunque por ahora hay incidencias de mayor prioridad.

Estos son ejemplos de tareas que pueden quedar relegadas y que conviene asignar a Copilot cloud agent. El agente puede trabajar en ellas de forma asíncrona mientras tú te ocupas de otras tareas. Después, podrás volver a revisar su trabajo y confirmar que todo está como esperabas.

## Presentación de GitHub Copilot cloud agent

[GitHub Copilot cloud agent](https://docs.github.com/copilot/concepts/agents/cloud-agent/about-cloud-agent#overview-of-copilot-cloud-agent-formerly-copilot-coding-agent) puede realizar tareas en segundo plano de forma similar a un desarrollador. Puedes interactuar con él de varias formas, por ejemplo, [asignándole una incidencia de GitHub](https://docs.github.com/copilot/how-tos/use-copilot-agents/cloud-agent/start-copilot-sessions). Cuando se la asignas, Copilot crea un pull request en borrador para hacer un seguimiento del trabajo, configura un entorno y empieza a trabajar en la tarea. Puedes consultar la sesión de Copilot mientras trabaja o después de que termine. Cuando la solución propuesta esté lista para que la revises, te mencionará en el pull request.

## La importancia de unas instrucciones con un alcance claro

GitHub Copilot no hace magia: no puedes dejar de pensar. Incluso las operaciones que parecen sencillas se complican al examinarlas con detalle, así que [define cuidadosamente el alcance de las tareas para Copilot cloud agent][cloud-agent-best-practices]. Considéralo un programador colaborativo basado en IA: trabaja por etapas, aprende, experimenta y adapta el enfoque. Los fundamentos del desarrollo de software no cambian por añadir IA generativa.

## Instrucciones personalizadas de este repositorio

En ejercicios anteriores has conocido las instrucciones personalizadas y cómo guían a Copilot. Si ya has completado la práctica sobre instrucciones personalizadas, habrás visto `.github/copilot-instructions.md` en acción. Si no, este es un buen momento para echarle un vistazo. Antes de asignar trabajo a Copilot cloud agent, lee los archivos de instrucciones que ya incluye este repositorio para que después puedas reconocer su efecto.

Abre los siguientes archivos en la interfaz web de GitHub o en un Codespace si ya tienes uno en marcha:

- `.github/copilot-instructions.md`: revisa la sección **Code standards**, en especial las convenciones de TypeScript y los requisitos de comentarios de documentación TSDoc.
- `.github/instructions/unit-tests.instructions.md`: fíjate en el frontmatter `applyTo`, que aplica estas instrucciones a los archivos `**/*.test.ts`.

Cuando asignes al agente la incidencia `Code lacks documentation` en la sección siguiente, observa si el pull request resultante incluye comentarios TSDoc, respeta las convenciones de TypeScript y añade encabezados de comentario a los archivos. Estas características proceden de los archivos de instrucciones que acabas de revisar. Volveremos a señalarlas más adelante, al revisar los PR.

## Configurar el entorno de desarrollo de Copilot cloud agent

Crear código, independientemente de quién lo haga, suele requerir un entorno específico y la ejecución de algunos scripts de configuración. Lo mismo ocurre al asignar tareas a Copilot, que trabaja de forma parecida a un ingeniero de software.

Cloud agent usa [GitHub Actions][github-actions] como entorno de trabajo. Puedes personalizarlo creando un [flujo de configuración especial][setup-workflow] en `.github/workflows/copilot-setup-steps.yml`, que se ejecuta antes de que el agente empiece a trabajar. Así puede acceder a las herramientas y dependencias de desarrollo necesarias. El flujo ya está configurado para facilitar el taller y que puedas aprender sobre este aspecto. Instala Node.js, las dependencias del proyecto y los binarios del navegador, y migra y carga la base de datos SQLite de la aplicación Astro:

```yaml
name: "Copilot Setup Steps"

# Permite probar los pasos de configuración desde la pestaña "Actions" del repositorio
on: workflow_dispatch

env:
  ASTRO_TELEMETRY_DISABLED: "1"

jobs:
  copilot-setup-steps:
    runs-on: ubuntu-latest
    # Configura los permisos mínimos necesarios para los pasos. Copilot recibe su propio token para sus operaciones.
    permissions:
      # Si los pasos de configuración clonan el repositorio, por ejemplo, para instalar dependencias, necesitan el permiso `contents: read`. Si no se clona el repositorio en estos pasos, Copilot lo hará automáticamente al terminar.
      contents: read
    steps:
      - name: Checkout code
        uses: actions/checkout@v5

      # Configuración de frontend y aplicación: Node.js (la aplicación es Astro + Drizzle/libSQL)
      - name: Set up Node.js
        uses: actions/setup-node@v6
        with:
          node-version: "lts/*"
          cache: "npm"
          cache-dependency-path: "./package-lock.json"

      - name: Install JavaScript dependencies
        run: npm ci

      - name: Install Playwright browsers
        run: npx playwright install --with-deps chromium

      # Migra y carga la base de datos SQLite local para que las compilaciones y las pruebas tengan datos.
      - name: Set up the database
        run: npm run db:setup
```

El archivo se parece a cualquier otro flujo de trabajo de GitHub, pero tiene algunos aspectos importantes:

- Contiene un único trabajo llamado `copilot-setup-steps`. Se ejecuta en GitHub Actions antes de que Copilot empiece a trabajar en el pull request.
- Observa el activador `workflow_dispatch`, que permite ejecutar manualmente el flujo de trabajo desde la pestaña **Actions** del repositorio. Esto sirve para probar que funciona, sin tener que esperar a que Copilot lo ejecute.

## Añadir documentación

Todo el mundo conoce la importancia de la documentación, pero la mayoría de los proyectos tienen información obsoleta o carecen de ella. Este tipo de deuda técnica suele quedar desatendida, reduce la productividad y dificulta el mantenimiento del código o la incorporación de nuevos desarrolladores. Copilot destaca en la creación de documentación, por lo que esta es una buena incidencia para asignarle. Trabajará en segundo plano para generar la documentación necesaria. En un ejercicio futuro volverás para revisar el resultado.

1. En una nueva pestaña del navegador, ve al repositorio en github.com.
2. Selecciona la pestaña **Issues**.
3. Selecciona **New issue** para abrir el cuadro de diálogo de incidencias.
4. Selecciona **Blank issue** para crear una incidencia en blanco.
5. Escribe `Code lacks documentation` en el campo **Title**.
6. Escribe el siguiente texto en **Description**:

    ```plaintext
    Nuestra organización requiere que las funciones y los métodos incluyan comentarios de documentación TSDoc cuando aporten claridad. Por desgracia, algunas actualizaciones recientes no han seguido este estándar. Tenemos que actualizar el código existente para incluir comentarios de documentación cuando ayuden a entender el comportamiento.
    ```

7. Selecciona **Create** para crear la incidencia.
8. En el lado derecho, selecciona **Assign to Copilot** para abrir el cuadro de diálogo de asignación.

  ![Asignar una incidencia a Copilot](../../../_images/shared-assign-copilot.png)

9. Selecciona **Assign**.
10. Selecciona la pestaña **Pull Requests**.
11. Abre el nuevo pull request (PR), que tendrá un título parecido a `[WIP]: Code lacks documentation`. Si aún no aparece, espera un momento y actualiza la página.
12. Al cabo de unos minutos, Copilot debería haber creado una lista de tareas.

> [!NOTE]
> La lista de tareas de Copilot puede tardar varios minutos en aparecer en el PR. Copilot está preparando el entorno (ejecutando el flujo de trabajo anterior), analizando el proyecto y determinando el mejor enfoque.

13. Revisa la lista y las tareas que se propone completar.
14. Desplázate por la cronología del pull request. Deberías ver una actualización que indica que Copilot ha empezado a trabajar en la incidencia.
15. Selecciona el botón **View session**.

  ![Vista de la sesión de Copilot](../../../_images/ex4-view-session.png)

> [!CAUTION]
> Es posible que debas actualizar la ventana para ver el indicador actualizado.

16. Observa que puedes recorrer la sesión en directo y ver cómo Copilot resuelve el problema: explora el código y comprende su estado, se detiene a considerar el plan adecuado y crea código.

Es probable que tarde varios minutos. Uno de los objetivos principales de Copilot cloud agent es realizar tareas de forma asíncrona para que tú puedas ocuparte de otras cosas. Aprovecharemos esa función asignándole otra tarea y, mientras tanto, nos dedicaremos a añadir funcionalidades a la aplicación.

## Añadir una sección de juegos relacionados a la página de detalles

Como se ha indicado, una de las ventajas de GitHub Copilot cloud agent es que permite dividir el trabajo. Mientras el agente se ocupa de una tarea, tú puedes centrarte en otra. Puede que añadir una sección de juegos relacionados para el equipo de diseño no lleve mucho tiempo, pero ese tiempo se puede aprovechar para otras tareas. Vamos a asignársela al agente.

1. Vuelve al repositorio en github.com.
2. Selecciona la pestaña **Issues**.
3. Selecciona **New issue** para abrir el cuadro de diálogo de incidencias.
4. Selecciona **Blank issue** para usar la plantilla en blanco.
5. Escribe `Show related games on the game details page` en **Title**.
6. Escribe lo siguiente en **Description**:

    ```markdown
    Queremos ayudar a la gente a descubrir más juegos mostrando juegos relacionados en la página de detalles de cada juego. El equipo de diseño quiere explorar la experiencia y realizar pruebas de aceptación. Estos son los requisitos:

   - Añadir un helper de acceso a datos en `src/lib/` que devuelva otros juegos de la misma categoría que un juego, sin incluir ese juego
   - Mostrar una sección "Related games" en la página de detalles del juego que use el helper
   - Gestionar el caso en que un juego no tenga juegos relacionados
   - Crear pruebas unitarias para el nuevo helper
   - Confirmar que todas las pruebas se superan antes de crear el PR
    ```

7. Selecciona **Create** para crear la incidencia.
8. En el lado derecho, selecciona **Assign to Copilot** para abrir el cuadro de diálogo de asignación.

  ![Asignar una incidencia a Copilot](../../../_images/shared-assign-copilot.png)

9. Selecciona **Assign**.

Al poco tiempo, verás un 👀 en el primer comentario de la incidencia. Indica que Copilot ya está trabajando.

![Copilot usa el emoji de ojos para indicar que está trabajando en la incidencia](../../../_images/ex4-issue-eyes-emoji.png)

Copilot ya está trabajando en la segunda solicitud. El agente funciona de forma similar a un ingeniero de software, así que no hace falta supervisarlo continuamente. Puedes revisar el resultado cuando termine. Ahora vamos a crear y usar agentes personalizados.

## Resumen y siguientes pasos

En esta lección has explorado [GitHub Copilot cloud agent][copilot-agents]. Puedes asignarle incidencias para que trabaje en ellas de forma asíncrona. Copilot puede ayudarte con la deuda técnica, crear funcionalidades o migrar código de un framework a otro.

Has explorado estos conceptos:

- personalizar el entorno de generación de código;
- garantizar que las operaciones se realicen de forma segura;
- definir incidencias con un alcance claro;
- asignar incidencias a Copilot.

Mientras el agente en la nube trabaja, puedes empezar a crear y usar agentes personalizados. [Copilot cloud agent también puede usar servidores MCP][cloud-agent-mcp] y puede acceder a las instrucciones personalizadas que has explorado en módulos anteriores.

## Recursos

- [Acerca de Copilot cloud agent][copilot-agents]
- [Asignar incidencias de GitHub a Copilot][assign-issue]
- [Buenas prácticas para el flujo de configuración de Copilot cloud agent][cloud-agent-best-practices]

| [Siguiente lección: Agentes personalizados →][next-lesson] |
|--:|

[previous-lesson]: ../1-custom-instructions/
[next-lesson]: ../3-custom-agents/
[cloud-agent-mcp]: https://docs.github.com/copilot/how-tos/copilot-on-github/customize-copilot/customize-cloud-agent/extend-cloud-agent-with-mcp
[assign-issue]: https://docs.github.com/copilot/how-tos/use-copilot-agents/cloud-agent/start-copilot-sessions
[setup-workflow]: https://docs.github.com/copilot/how-tos/copilot-on-github/customize-copilot/customize-cloud-agent/customize-the-agent-environment
[copilot-agents]: https://docs.github.com/copilot/concepts/agents/cloud-agent/about-cloud-agent
[cloud-agent-best-practices]: https://docs.github.com/copilot/how-tos/copilot-on-github/customize-copilot/customize-cloud-agent/customize-the-agent-environment
[github-actions]: https://docs.github.com/actions
