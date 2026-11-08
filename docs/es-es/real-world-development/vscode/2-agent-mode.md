---
title: "Ejercicio 2 - Añadir funcionalidades con el modo de agente de Copilot"
authors:
  - geektrainer
lastUpdated: 2026-06-30
---

Incluso las actualizaciones más sencillas de una aplicación suelen requerir cambios en varios archivos y operaciones como ejecutar pruebas. Como desarrollador, normalmente tendrás que localizar los archivos necesarios, hacer los cambios, ejecutar las pruebas, depurar, averiguar qué archivo te has dejado y volver a actualizarlo. La lista continúa.

Aquí es donde entra en juego el modo de agente de Copilot.

El modo de agente está diseñado para trabajar de forma más autónoma en el IDE. Se comporta de manera parecida a un desarrollador: primero explora la estructura del proyecto, realiza las actualizaciones necesarias, ejecuta tareas como las pruebas y corrige automáticamente los errores que encuentra. Vamos a ver cómo usarlo para añadir nuevas funcionalidades al sitio.

> [!NOTE]
> Aunque los nombres se parecen, el modo de agente y el agente en la nube ofrecen experiencias distintas. El modo de agente trabaja en el IDE y permite ciclos de respuesta rápidos e interacción directa. Cloud agent está diseñado para trabajar de forma asíncrona como un miembro del equipo, e interactuar contigo mediante incidencias y pull requests.

En este ejercicio aprenderás:

- cómo el modo de agente de Copilot explora el proyecto, identifica los archivos pertinentes y coordina cambios entre ellos;
- cómo puede implementar nuevas funciones en la interfaz y la capa de datos;
- cómo revisar los cambios y las pruebas generados por Copilot antes de fusionarlos en el repositorio.

## Escenario

A medida que aumenta la lista de juegos, quieres permitir que los usuarios filtren por categoría y editorial. En el ejercicio anterior añadiste un helper de editoriales; ahora completarás el trabajo restante en la capa de datos, la interfaz y las pruebas con el modo de agente de Copilot.

## Ejecutar el sitio web de Tailspin Toys

Antes de hacer cambios, exploremos el sitio web de Tailspin Toys para entender sus funciones actuales.

El sitio web es una plataforma de financiación colectiva de juegos de mesa con temática de desarrollo. Permite mostrar juegos y sus detalles. Es una única aplicación Astro que genera páginas HTML estáticas durante la compilación. Las páginas consultan directamente una base de datos SQLite local mediante Drizzle ORM en su frontmatter; no hay una API de backend independiente ni un framework de interfaz en el cliente. Los helpers de acceso a datos reutilizables están en `src/lib/`, y la interactividad se añade con pequeños elementos `<script>` de Astro que usan las API DOM estándar.

### Iniciar el sitio web

Para facilitar el inicio del sitio, se ha proporcionado la tarea `npm run dev`, que pone en marcha un único servidor de desarrollo Astro. Para ejecutarlo en GitHub Codespaces:

1. Vuelve a tu Codespace. Después del ejercicio anterior, deberías estar en `main`.
2. Abre un terminal nuevo en el Codespace pulsando <kbd>Control</kbd>+<kbd>`</kbd> (Mac) o <kbd>Ctrl</kbd>+<kbd>`</kbd> (Windows/Linux).
3. Crea y cambia a una nueva rama para el trabajo de filtrado:

   ```bash
   git checkout main
   git pull
   git checkout -b filtering-vscode
   ```

4. Ejecuta el siguiente comando para iniciar el sitio web:

   ```bash
   npm run dev
   ```

   Cuando el servidor de desarrollo Astro esté listo, verás un mensaje parecido a este:

   ```bash
   🚀 Tailspin Toys is ready!
      Astro server: http://localhost:4321
      Press Ctrl-C to stop.
   ```

> [!NOTE]
> Si aparece un cuadro de diálogo para abrir una ventana del navegador en `http://localhost:4321`, ciérralo seleccionando **x**.

5. Abre el sitio web manteniendo pulsado <kbd>Command</kbd> (Mac) o <kbd>Ctrl</kbd> (Windows/Linux) y seleccionando la dirección del servidor Astro `http://localhost:4321` en el terminal.

> [!NOTE]
> En Codespaces, al seleccionar el enlace a la URL local en el terminal, se te redirigirá automáticamente a `https://<your-codespace-name>-4321.app.github.dev/`. Se trata de un túnel privado al Codespace, que ahora aloja el servidor web.

### Explorar el sitio web

Con el sitio web en marcha, puedes explorar sus funciones. Entre otras cosas, incluye:

- **Página de inicio:** muestra una lista de juegos de mesa con sus títulos, imágenes y descripciones.
- **Página de detalles del juego:** al seleccionar un juego, se abre una página con más información, como el título, la descripción, la editorial y la categoría.

## Explorar el backlog con Copilot

> [!TIP]
> **Abrir Copilot Chat**
>
> Antes de empezar los ejercicios siguientes, vuelve a tu Codespace, abre el panel de Copilot Chat y selecciona **New Chat** para iniciar una conversación nueva. El modo y el modelo varían según el ejercicio; cada paso lo indicará cuando sea pertinente.

La implementación inicial del sitio funciona, pero queremos añadir capacidades. Empecemos por revisar el backlog. Al crear el proyecto a partir de la plantilla, se generó automáticamente un backlog de incidencias. Pide a GitHub Copilot que te muestre esas incidencias.

1. Selecciona **Agent** en el selector de agentes de la vista Chat. El agente planifica e implementa cambios en varios archivos de forma autónoma, ejecuta comandos del terminal e invoca herramientas.

   ![Captura del selector de agente en la vista Chat.](../../../_images/shared-chat-mode-selector.png)

2. Selecciona **Claude Sonnet 4.5** en la lista de modelos disponibles.

> [!CAUTION]
> Los autores de este taller no recomiendan un modelo concreto. Durante su desarrollo usamos Claude Sonnet 4.5 y lo incluimos en estas instrucciones para intentar ofrecer una experiencia coherente. Sin embargo, como los modelos de lenguaje grandes son probabilísticos, es posible que recibas sugerencias distintas de las indicadas. Es normal y esperable.

> [!NOTE]
> Debido al carácter probabilístico de los modelos de lenguaje grandes, Copilot puede usar un comando MCP distinto, pero debería poder completar la tarea.

3. Pide a Copilot que muestre las incidencias del backlog con este prompt:

   ```plaintext
   Muéstrame el backlog de incidencias de mi repositorio de GitHub. Ayúdame a priorizarlas en función de cuáles serán más útiles para el usuario.
   ```

4. Selecciona **Continue** para ejecutar el comando que enumera las incidencias.
5. Revisa la lista generada.

Observa que Copilot también ha priorizado las incidencias según las que considera más útiles para el usuario.

## Revisar los archivos de instrucciones

Antes de pedir al agente que genere código, revisa el archivo de instrucciones que proporcionará contexto a Copilot. Usarás el archivo de [interfaz de usuario (UI)](https://github.com/github-samples/tailspin-toys/blob/main/.github/instructions/ui.instructions.md), que explica cómo añadir funciones al sitio web.

1. En el Codespace, ve a `.github/instructions/ui.instructions.md`.
2. Observa las directrices generales para añadir funcionalidad. Incluyen:
   - una descripción general de la arquitectura;
   - principios de diseño de componentes, facilidad de prueba y accesibilidad;
   - enlaces a archivos de instrucciones específicos para varios tipos de archivo, como Astro, Tailwind CSS y Drizzle.

> [!TIP]
> Los archivos de instrucciones pueden hacer referencia a otros archivos de instrucciones y a archivos del proyecto. Las rutas son relativas a la ubicación del archivo de instrucciones. Esto permite reutilizar las directrices, dividir conjuntos grandes en otros más manejables y mostrar a Copilot ejemplos y plantillas.

## Implementar el filtrado

Para completar el filtrado, tendrás que realizar al menos tres actualizaciones independientes:

- añadir o ajustar la lógica restante del filtrado en la capa de datos (`src/lib/`);
- añadir o actualizar las pruebas de comportamiento del filtrado;
- actualizar la página de lista de juegos para incluir la interfaz de filtrado.

Además, las pruebas deben ejecutarse y superarse antes de fusionar los cambios. El modo de agente de Copilot puede encargarse de estas tareas.

1. Puedes continuar la conversación actual con Copilot o seleccionar **New Chat** para iniciar otra.
2. Selecciona **Add Context**, **Instructions** y **ui** como archivo de instrucciones.

   ![Captura de cómo seleccionar el archivo de instrucciones de interfaz](../../../_images/ex3-select-instructions-file.png)

3. Confirma que **Agent** siga seleccionado en el selector de agentes de la vista Chat.

   ![Captura del selector de agente en la vista Chat.](../../../_images/shared-chat-mode-selector.png)

4. Confirma que **Claude Sonnet 4.5** siga seleccionado como modelo.
5. Pide a Copilot que implemente la funcionalidad según la incidencia relacionada del backlog:

   ```plaintext
   Actualiza el sitio para incluir filtros por editorial y categoría de acuerdo con los requisitos de la incidencia relacionada del backlog de GitHub. Ya existe un helper de editoriales del ejercicio anterior; consérvalo y ajústalo si hace falta para completar el trabajo restante de la capa de datos, la interfaz y las pruebas. Confirma que todas las pruebas se superan antes de terminar. El servidor ya está en marcha, así que no hace falta iniciarlo.
   ```

6. Observa cómo Copilot explora el proyecto y localiza los archivos pertinentes. Debería encontrar tanto los helpers de la capa de datos y la interfaz como las pruebas. Después, modificará los archivos y ejecutará las pruebas.

   ![Captura de Copilot explorando archivos del proyecto](../../../_images/ex3-agent-mode-explores.png)

> [!NOTE]
> Copilot realizará varias tareas, como explorar el proyecto, modificar archivos y ejecutar pruebas. El proceso puede tardar unos minutos según la complejidad de la tarea y del código. Durante ese tiempo, es posible que aparezcan los botones **Keep** y **Undo** en el editor. Cuando termine, tendrás la opción de conservar o deshacer todos los cambios; no tienes que seleccionar ninguna mientras el trabajo siga en curso.

7. Cuando Copilot te lo solicite, selecciona **Continue** para ejecutar las pruebas.

   ![Captura de un diálogo de Copilot Chat que pide confirmar si quieres ejecutar las pruebas](../../../_images/ex3-agent-mode-run-tests.png)

8. Es posible que haya pausas e incluso que algunas pruebas fallen durante el proceso. No pasa nada. Copilot alterna entre generar código y ejecutar pruebas hasta completar la tarea y dejar de detectar errores.

   ![Captura de una sesión de Copilot Agent Mode terminada](../../../_images/ex3-agent-mode-proposed-changes.png)

9. Explora el código generado para detectar posibles problemas.

> [!CAUTION]
> Recuerda que siempre es importante revisar el código generado por Copilot o cualquier otra herramienta de IA.

10. Vuelve al navegador con el sitio en marcha y explora la nueva funcionalidad.
11. Cuando hayas confirmado que todo funciona y revisado el código, selecciona **Keep** en la ventana de Copilot Chat.

## Resumen y siguientes pasos

Enhorabuena. En este ejercicio has aprendido a usar el modo de agente de GitHub Copilot para añadir funciones al sitio de Tailspin Toys. Has visto cómo:

- el modo de agente de GitHub Copilot puede implementar funciones en la interfaz y la capa de datos;
- Copilot puede explorar el proyecto, identificar los archivos pertinentes y coordinar cambios;
- revisar los cambios y las pruebas generados por Copilot antes de fusionarlos en el repositorio.

Ahora vamos a [probar la funcionalidad con el servidor MCP de Playwright][next-lesson] y abrir un pull request.

### Ejercicio de exploración adicional: implementar paginación

A medida que aumente la lista de juegos, será necesario habilitar la paginación. Aplica lo aprendido en este ejercicio y pide a Copilot que añada paginación al sitio. Al diseñar el código, ten en cuenta lo siguiente:

- sigue las buenas prácticas existentes, incluidos los archivos de instrucciones;
- decide cómo quieres implementar la paginación: por ejemplo, si permitirás que el usuario elija el tamaño de página o si usarás un valor fijo;
- proporciona a Copilot las directrices necesarias para crear la implementación que necesitas;
- puede que tengas que iterar con GitHub Copilot y pedir cambios o proporcionar contexto. Es lo normal al trabajar con Copilot.

## Recursos

- [Copilot: modos Ask, Edit y Agent, y cuándo usarlos][choose-mode]
- [Modo de agente en VS Code][vs-code-agent-mode]

[previous-lesson]: ../1-custom-instructions/
[next-lesson]: ../3-mcp/
[choose-mode]: https://github.blog/ai-and-ml/github-copilot/copilot-ask-edit-and-agent-modes-what-they-do-and-when-to-use-them/
[vs-code-agent-mode]: https://code.visualstudio.com/docs/copilot/chat/chat-agent-mode
