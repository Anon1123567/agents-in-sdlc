---
title: "Ejercicio 5: Iterar sobre el trabajo de GitHub Copilot"
authors:
  - geektrainer
lastUpdated: 2026-06-30
---

| [← Lección anterior: Supervisar y gestionar agentes][previous-lesson] |
|:--|

## Revisar el trabajo

Durante este taller has trabajado con GitHub Copilot en varias tareas para mejorar la experiencia de usuario y añadir funcionalidades. Le has pedido a Copilot que añada documentación al código, cree una función de juegos relacionados para que el equipo de diseño pueda iterar sobre ella e implemente funciones de accesibilidad, como controles de modo de alto contraste y modo claro. Vamos a revisar los cambios de código y, si hace falta, proporcionar comentarios a Copilot para mejorar el trabajo.

### Escenario

Como ya hemos indicado varias veces, los fundamentos del diseño de software y DevOps no cambian por añadir IA generativa. Siempre debemos revisar el código generado y seguir nuestro proceso habitual de DevOps. Con eso en mente, revisaremos las propuestas de GitHub Copilot para crear la documentación, la función de juegos relacionados y las funciones de accesibilidad antes de habilitar la revisión para el resto del equipo.

## Seguridad y GitHub Copilot cloud agent

Como Copilot cloud agent realiza sus tareas de forma asíncrona y sin supervisión, se han establecido ciertas restricciones de seguridad para que el entorno siga siendo seguro. Entre ellas:

- Copilot solo tiene acceso de lectura al repositorio y acceso de escritura **únicamente** a la rama que usa para su trabajo.
- Cloud agent se ejecuta en GitHub Actions y crea un entorno independiente y efímero para trabajar.
- Los flujos de trabajo de GitHub Actions requieren la aprobación de una persona antes de ejecutarse.
- [El acceso a recursos externos está limitado de forma predeterminada](https://docs.github.com/copilot/how-tos/copilot-on-github/customize-copilot/customize-cloud-agent/customize-the-agent-firewall), incluidos los servidores MCP.

## Revisar la documentación generada

Empecemos por el primer pull request (PR) generado por GitHub Copilot cloud agent: el que añade documentación al código. Revisarás el trabajo con la interfaz habitual de PR de GitHub.com.

> [!NOTE]
> Al explorar el PR, puede que aparezca una advertencia que indica que el firewall ha bloqueado a GitHub Copilot. Es **normal**: Copilot tiene acceso limitado a recursos externos de forma predeterminada, incluidas las llamadas a servidores MCP externos. Si quieres, puedes [personalizar o deshabilitar el firewall de Copilot cloud agent][agent-firewall].

1. Vuelve al repositorio en github.com.
2. Selecciona **Pull Requests** para abrir la lista de pull requests.
3. Abre el pull request con un título parecido a **Add missing documentation** u otro título más descriptivo.

> [!NOTE]
> Si Copilot sigue trabajando, el pull request incluirá la etiqueta **[WIP]**. En ese caso, espera a que termine. Puede tardar unos minutos; puedes descansar o repasar lo que has aprendido.

4. Cuando el pull request esté listo, selecciona la pestaña **Files changed** y revisa los cambios.

    ![Pestaña Files changed](../../../_images/shared-pr-files-changed.png)

5. Explora el código actualizado, que incluirá los nuevos comentarios de documentación TSDoc y otros cambios. El contenido exacto dependerá de la solución.

    Mientras revisas los cambios, busca comentarios de documentación TSDoc, convenciones de TypeScript y encabezados de comentario. Proceden de los archivos de instrucciones personalizadas que revisaste al principio del Ejercicio 2.

6. Cuando hayas revisado las actualizaciones y te parezcan correctas, vuelve a la pestaña **Conversation** y desplázate hacia abajo.
7. Deberías ver un indicador de que hay flujos de trabajo pendientes de aprobación.
8. Si los hay, selecciona **Approve and run workflows**.

   ![Aprobar y ejecutar flujos de trabajo](../../../_images/shared-approve-workflows.png)

9. Deberías ver los flujos de trabajo en la cola de la sección de comprobaciones del pull request. Si todo va bien, las comprobaciones del proyecto deberían superarse en la aplicación Astro. La operación puede tardar unos minutos.

## Solicitar cambios a GitHub Copilot

Trabajar con Copilot en un pull request no es una interacción unidireccional. Puedes mencionarlo en comentarios del PR o en comentarios en línea del código, como harías con otras personas del equipo. Copilot verá esos comentarios e iniciará otra sesión para resolverlos. Como los resultados no son deterministas, no podemos recomendar un texto exacto. Algunas ideas de cambios que puedes solicitar:

- Añadir encabezados de comentario al principio de cada archivo de código con una breve descripción de su función.
- Añadir comentarios TSDoc a los archivos TypeScript y Astro.
- Crear un README que describa la estructura de la aplicación Astro.

1. Escribe un comentario para solicitar un cambio en la documentación generada y menciona a **@copilot**, como harías con cualquier otra persona. Puedes usar una de las ideas anteriores o pedir otro cambio relacionado con la documentación del código.
2. Selecciona **View Session** para ver el trabajo de Copilot. Observarás que empieza una nueva sesión para realizar los cambios.
3. Puedes seleccionar **Back to pull request** para volver al pull request.

    ![Volver al pull request](../../../_images/ex7-back-to-pr.png)

4. Cuando Copilot termine los cambios, debería aparecer una nueva confirmación en el pull request.
5. Selecciona la pestaña **Files changed** para revisar los cambios.

Puedes seguir iterando hasta que estés conforme. Cuando lo estés, puedes marcar el PR en borrador como listo para revisión y fusionarlo en la rama principal.

![Marcar el PR como listo para revisión](../../../_images/ex7-ready-for-review.png)

## Revisar la función de juegos relacionados

Volvamos al PR que Copilot ha creado para resolver la incidencia sobre la sección de juegos relacionados en la página de detalles.

1. Vuelve al repositorio en GitHub.com.
2. Selecciona la pestaña **Pull Requests**.
3. Selecciona el PR cuyo título se parezca a **Show related games on the game details page** u otro título más descriptivo.
4. Selecciona **Files changed** para revisar el código generado.
5. Cuando hayas revisado las actualizaciones y te parezcan correctas, vuelve a la pestaña **Conversation** y desplázate hacia abajo.
6. Deberías ver un indicador de que hay flujos de trabajo pendientes de aprobación.
7. Si los hay, selecciona **Approve and run workflows**.

   ![Aprobar y ejecutar flujos de trabajo](../../../_images/shared-approve-workflows.png)

8. Deberías ver los flujos de trabajo en la cola de la sección de comprobaciones del pull request. Si todo va bien, las comprobaciones del proyecto deberían superarse en la aplicación Astro. La operación puede tardar unos minutos.
9. **Opcional:** puedes cambiar a esta rama en tu Codespace y probar manualmente la función de juegos relacionados. Ve al Codespace, abre el terminal y ejecuta estos comandos (sustituye `<branch-name>` por el nombre de la rama creada por Copilot, por ejemplo, **copilot/fix-8**):

    ```bash
    git fetch origin
    git checkout <branch-name>
    ```

Copilot ha creado la función de juegos relacionados. Como antes, puedes trabajar de forma iterativa con Copilot cloud agent para solicitar cambios. Por ejemplo, podrías pedirle que modifique el número de juegos relacionados que se muestran o que añada encabezados de comentario y comentarios de documentación TSDoc (recuerda que asignaste esta tarea **antes** de actualizar las instrucciones personalizadas). Al igual que en los pasos anteriores, puedes escribir estos comentarios en la pestaña **Conversation**; Copilot los verá e iniciará una nueva sesión.

## Revisar las funciones de accesibilidad

Por último, revisaremos las funciones de accesibilidad implementadas con el agente personalizado de accesibilidad. Este PR debe incluir tanto el modo de alto contraste asignado en el Ejercicio 3 como el modo claro solicitado desde el centro de control en el Ejercicio 4.

1. Vuelve al repositorio en GitHub.com.
2. Selecciona la pestaña **Pull Requests**.
3. Selecciona el PR con un título parecido a **Add high contrast mode to website** u otro título más descriptivo.

> [!NOTE]
> Si Copilot sigue trabajando, el pull request incluirá la etiqueta **[WIP]**. En ese caso, espera a que termine. Puede tardar unos minutos.

4. Selecciona **Files changed** para revisar el código generado.
5. Revisa la implementación y presta especial atención a:
   - los componentes de interfaz para cambiar entre los modos;
   - el uso del almacenamiento local para conservar las preferencias;
   - los cambios de CSS o de estilos para los modos de alto contraste y claro;
   - los atributos de accesibilidad (etiquetas ARIA, navegación con teclado, etc.);
   - el código JavaScript o TypeScript que gestiona el cambio de modo.

6. Cuando hayas revisado las actualizaciones y te parezcan correctas, vuelve a la pestaña **Conversation** y desplázate hacia abajo.
7. Deberías ver un indicador de que hay flujos de trabajo pendientes de aprobación.
8. Si los hay, selecciona **Approve and run workflows**.

   ![Aprobar y ejecutar flujos de trabajo](../../../_images/shared-approve-workflows.png)

9. Deberías ver los flujos de trabajo en la cola de la sección de comprobaciones del pull request. Si todo va bien, las comprobaciones del proyecto deberían superarse en la aplicación Astro. La operación puede tardar unos minutos.
10. **Opcional:** puedes cambiar a esta rama en tu Codespace para probar manualmente las funciones de accesibilidad. Ve al Codespace, abre el terminal y ejecuta estos comandos (sustituye `<branch-name>` por el nombre de la rama creada por Copilot):

    ```bash
    git fetch origin
    git checkout <branch-name>
    ```

    Después, inicia la aplicación y prueba los controles de modo de alto contraste y modo claro en el navegador para confirmar que funcionan como esperas y que las preferencias se conservan al volver a cargar la página.

Observa cómo el agente personalizado de accesibilidad ha ayudado a Copilot a implementar estas funciones siguiendo las buenas prácticas de accesibilidad. Si detectas problemas o mejoras posibles, puedes mencionar a **@copilot** en un comentario y solicitar cambios, igual que con los PR anteriores.

## Ejercicio opcional: seguir delegando

Cloud agent funciona mejor cuando puedes asignarle incidencias reales del backlog y ocuparte de otra cosa. Para adquirir esa costumbre, crea algunas incidencias más en el repositorio y asígnaselas a Copilot. Algunas ideas:

- Crear un formulario para indicar interés en un juego en la página de detalles.
- Implementar la paginación en la página de lista de juegos.
- Añadir validación de entradas y gestión de errores a los helpers de acceso a datos.

## Resumen

Has completado el recorrido de Cloud agent. En estas lecciones:

- **Has inspeccionado los archivos de instrucciones personalizados del repositorio** para reconocer su efecto en el trabajo de cloud agent.
- **Has asignado incidencias a Copilot cloud agent** y has visto cómo prepara el entorno, planifica y trabaja de forma asíncrona.
- **Has creado y usado un agente personalizado** para accesibilidad y has añadido controles de modo de alto contraste y modo claro.
- **Has usado la página de agentes de Copilot como centro de control** para supervisar y dirigir la sesión de accesibilidad mientras trabajaba.
- **Has revisado los pull requests de cloud agent e iterado sobre ellos**, mencionando a `@copilot` para solicitar cambios y aprobando flujos de trabajo.

## Revisión y siguientes pasos

Has completado el recorrido de Cloud agent. Si quieres seguir explorando, los otros recorridos complementan lo que has practicado:

- 🖥️ **[Recorrido de VS Code](../../vscode/)**: explora el modo de agente de Copilot Chat y la integración de MCP desde el IDE.
- 💻 **[Recorrido de CLI](../../cli/)**: entrega dos cambios revisados desde el terminal con los modos Plan y Autopilot, habilidades de agente, un agente de QA personalizado, Playwright MCP, Agent Merge y un puente opcional `/delegate` con cloud agent.

En tu propio repositorio, prueba estas ideas:

- Asigna a cloud agent una incidencia de refactorización o cobertura de pruebas.
- Crea un agente personalizado para otro ámbito.
- Configura `copilot-setup-steps.yml` para otra pila tecnológica.

## Recursos

- [GitHub Copilot][github-copilot]
- [Acerca de los agentes de Copilot][copilot-agents]
- [Asignar incidencias de GitHub a Copilot][assign-issue]
- [Buenas prácticas para el flujo de configuración de Copilot cloud agent][cloud-agent-best-practices]
- [Configurar el firewall de Copilot cloud agent][agent-firewall]

---

| [← Lección anterior: Supervisar y gestionar agentes][previous-lesson] |
|:--|

[github-copilot]: https://github.com/features/copilot
[cloud-agent-overview]: https://docs.github.com/copilot/concepts/agents/cloud-agent/about-cloud-agent
[assign-issue]: https://docs.github.com/copilot/how-tos/use-copilot-agents/cloud-agent/start-copilot-sessions
[setup-workflow]: https://docs.github.com/copilot/how-tos/copilot-on-github/customize-copilot/customize-cloud-agent/customize-the-agent-environment
[copilot-agents]: https://docs.github.com/copilot/concepts/agents/cloud-agent/about-cloud-agent
[cloud-agent-best-practices]: https://docs.github.com/copilot/how-tos/copilot-on-github/customize-copilot/customize-cloud-agent/customize-the-agent-environment
[agent-firewall]: https://docs.github.com/copilot/how-tos/copilot-on-github/customize-copilot/customize-cloud-agent/customize-the-agent-firewall

[previous-lesson]: ../4-managing-agents/
