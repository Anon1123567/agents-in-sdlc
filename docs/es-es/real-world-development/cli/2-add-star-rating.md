---
title: "Lección 2 - Añadir valoraciones por estrellas: un cambio rápido"
description: "Usa Copilot CLI para hacer un pequeño cambio en las tarjetas de juegos, revísalo en un navegador redirigido y fusiónalo como tu primer pull request."
authors:
  - geektrainer
lastUpdated: 2026-09-18
---

Ya has instalado Copilot CLI y probado una conversación. Ahora harás tu primer cambio en el proyecto. Será sencillo: los datos de los juegos ya incluyen una valoración por estrellas, pero las tarjetas de la página de inicio aún no la muestran. Pedirás al agente que la muestre, revisarás el cambio y lo fusionarás como tu primer pull request.

En esta lección:

- iniciarás una conversación de Copilot centrada en una rama de funcionalidad;
- pedirás al agente que haga un pequeño cambio en el proyecto;
- revisarás el cambio con `/diff`;
- ejecutarás la aplicación para comprobarlo en un navegador redirigido;
- abrirás y fusionarás tu primer pull request.

## Escenario

Cada juego de Tailspin Toys puede tener una valoración por estrellas, que ya aparece en la página de detalles. Sin embargo, las tarjetas de la página de inicio solo muestran el título, la categoría, la editorial y la descripción. Para empezar, pedirás al agente que muestre en cada tarjeta la valoración existente. Es un cambio pequeño y autónomo, ideal para tu primera sesión.

## Anatomía de una conversación

Una **conversación** es el espacio donde trabajas con Copilot CLI en una tarea. A diferencia de la aplicación Copilot, una conversación normal de CLI usa el repositorio y la rama de Git que están seleccionados en el terminal, en lugar de crear un worktree dedicado. Las conversaciones guardadas te permiten volver a la misma conversación más adelante, mientras que los archivos y la rama siguen siendo el estado normal de Git en el disco.

En una conversación verás tres cosas: tus prompts y las respuestas del agente, la actividad de las herramientas mientras explora y edita archivos, y los cambios que puedes inspeccionar con `/diff`.

## Iniciar una conversación y solicitar un cambio

Empecemos una conversación nueva para implementar la funcionalidad.

1. Vuelve a tu Codespace.
2. Si el terminal no sigue abierto, pulsa <kbd>Control</kbd>+<kbd>`</kbd> (Mac) o <kbd>Ctrl</kbd>+<kbd>`</kbd> (Windows/Linux).
3. Si Copilot CLI aún no está abierto, inicia Copilot con el siguiente comando:

   ```bash
   copilot --yolo
   ```

4. Inicia una sesión nueva con el comando de barra `/new` y pulsa <kbd>Enter</kbd>.
5. Envía este prompt para solicitar el cambio:

   ```plaintext
   Muestra la valoración `starRating` de cada juego sobre 5 en las tarjetas de la página de lista. Si la valoración es null, muestra "No rating yet". Mantén el diseño actual de las tarjetas, añade pruebas y ejecuta las comprobaciones pertinentes.
   ```

Copilot explorará el proyecto, localizará los archivos que muestran los detalles de los juegos y creará el código necesario. Ya has añadido una funcionalidad con Copilot CLI.

## Revisar la diferencia

Hay que revisar todos los cambios generados por IA antes de fusionarlos, incluso los pequeños. Vamos a inspeccionarlos en Copilot CLI.

1. Escribe `/diff` y revisa todos los archivos modificados.
2. Confirma que la tarjeta muestra la valoración numérica cuando existe y `No rating yet` cuando `starRating` es `null`.
3. Confirma que las pruebas cubren ambos casos.
4. Revisa los resultados de las comprobaciones ejecutadas por Copilot y pídele que corrija los errores.
5. Cuando termines la revisión, pulsa <kbd>Esc</kbd> para salir de la vista de diferencias.

> [!NOTE]
> Como Copilot, al igual que las demás herramientas de IA generativa, es probabilístico y no determinista, el código concreto puede variar. Revisa el comportamiento, no esperes una implementación exacta.

## Comprobar los cambios

No basta con leer el código y dar por sentado que funciona. Pide a Copilot que inicie el sitio web para poder revisar la interfaz actualizada en el navegador redirigido por Codespaces.

1. Pide a Copilot que inicie la aplicación:

   ```plaintext
   Inicia la aplicación para que pueda revisar en el navegador el cambio de la valoración por estrellas. Dime la URL y deja el servidor en marcha.
   ```

2. Cuando Codespaces indique que el puerto `4321` está disponible, selecciona **Open in Browser**.
3. Confirma que las tarjetas muestran las valoraciones sobre cinco.
4. Vuelve a Copilot y pídele que detenga el servidor que ha iniciado:

   ```plaintext
   Detén el servidor de desarrollo que has iniciado.
   ```

## Abrir y fusionar tu primer pull request

Ya has creado la funcionalidad. Vamos a abrir un pull request (PR) para fusionar el nuevo código en el proyecto.

1. Pide al agente predeterminado que confirme el cambio:

   ```plaintext
   Confirma los cambios revisados de la valoración por estrellas con un mensaje de confirmación adecuado.
   ```

2. Escribe `/pr create`. Copilot CLI enviará la confirmación existente al crear el PR y mostrará la URL.
3. Abre el PR manteniendo pulsado <kbd>Command</kbd> (Mac) o <kbd>Ctrl</kbd> (Windows/Linux) y seleccionando la URL que muestra Copilot CLI.
4. Revisa los archivos modificados y las comprobaciones.
5. Cuando esté listo, selecciona **Merge pull request** y confirma la fusión.
6. Vuelve a tu Codespace y sal de Copilot CLI con `/exit`.
7. Actualiza tu `main` local:

   ```bash
   git checkout main
   git pull
   ```

## Resumen y siguientes pasos

Enhorabuena. Has publicado tu primer cambio con GitHub Copilot CLI. En concreto:

- has iniciado una conversación centrada en una rama de funcionalidad;
- has pedido al agente que haga un pequeño cambio en las tarjetas;
- has revisado el cambio con `/diff`;
- has ejecutado la aplicación y confirmado la valoración por estrellas en un navegador redirigido;
- has abierto y fusionado tu primer pull request.

A continuación, [empezarás con la incidencia de filtrado y usarás los modos Plan y Autopilot][next-lesson] para crear una funcionalidad más amplia.

## Recursos

- [Acerca de GitHub Copilot CLI][about-copilot-cli]
- [Referencia de comandos de Copilot CLI][cli-reference]

[previous-lesson]: ../1-install-copilot-cli/
[next-lesson]: ../3-agent-modes/
[about-copilot-cli]: https://docs.github.com/copilot/concepts/agents/about-copilot-cli
[cli-reference]: https://docs.github.com/copilot/reference/copilot-cli-reference/cli-command-reference
