---
title: "Ejercicio 1 - Instrucciones personalizadas (Cloud agent)"
authors:
  - geektrainer
lastUpdated: 2026-06-30
---

[← Lección anterior: Requisitos previos][previous-lesson] · [Siguiente lección: GitHub Copilot cloud agent →][next-lesson]

El contexto es fundamental cuando se trabaja con IA generativa. Si una tarea debe hacerse de una forma concreta o Copilot necesita información de fondo, asegúrate de que tenga acceso a ese contexto. Los [archivos de instrucciones][instruction-files] proporcionan esas directrices para que Copilot entienda no solo *qué* quieres que haga, sino también *cómo*.

En este ejercicio:

- explorarás cómo el contexto específico del proyecto, las directrices de programación y los estándares de documentación llegan a Copilot mediante las instrucciones personalizadas del repositorio y los archivos de instrucciones aplicables a determinadas rutas;
- añadirás un nuevo estándar general del repositorio a `.github/copilot-instructions.md`.

> [!NOTE]
> A diferencia de los recorridos de VS Code y CLI, aquí no usarás un prompt «antes y después». Copilot cloud agent trabaja de forma asíncrona con incidencias de GitHub, por lo que es más difícil demostrar en tiempo real su impacto con una comparación directa. Más adelante, al revisar los pull requests que genere el agente, verás la influencia del archivo de instrucciones.

## Archivos de instrucciones

### Escenario

Como cualquier buen equipo de desarrollo, Tailspin Toys tiene directrices y requisitos para sus prácticas de desarrollo. Entre ellos se incluyen:

- La capa de datos siempre necesita pruebas unitarias.
- La interfaz debe estar en modo oscuro y tener un aspecto moderno.
- Debe añadirse documentación al código en forma de comentarios de documentación TSDoc.
- Debe añadirse un bloque de comentarios al inicio de cada archivo para describir su función.

Con los archivos de instrucciones, te asegurarás de que Copilot disponga de la información adecuada para realizar las tareas de acuerdo con estas prácticas.

### Instrucciones personalizadas

Las instrucciones personalizadas permiten proporcionar contexto y preferencias a Copilot para que entienda mejor tus convenciones y requisitos de desarrollo. Esta función puede ayudarte a obtener sugerencias y fragmentos de código más pertinentes. Puedes especificar las convenciones de código, las bibliotecas y los tipos de comentarios que prefieres. También puedes crear instrucciones para todo el repositorio o para tipos de archivo concretos y aportar contexto específico para cada tarea.

Hay dos tipos de archivos de instrucciones:

- `.github/copilot-instructions.md`, un único archivo que se envía a Copilot en **todas** las solicitudes del repositorio. Debe contener información a nivel del proyecto: contexto relevante para la mayoría de las solicitudes de chat o CLI enviadas a Copilot. Puede describir la pila tecnológica, el proyecto, las buenas prácticas y otras directrices generales.
- Los archivos `.github/instructions/*.instructions.md`, que se crean para tareas o tipos de archivo específicos. Puedes usarlos para definir directrices para lenguajes (como TypeScript o Astro) o tareas (como crear un componente de interfaz o un conjunto de pruebas unitarias).

> [!NOTE]
> En el IDE, los archivos de instrucciones solo se usan para generar código en Copilot Chat, no para completar código ni para sugerir la siguiente edición.
>
> Copilot Chat, Copilot CLI y Copilot cloud agent usan tanto las instrucciones del repositorio como los archivos `*.instructions.md` (con frontmatter `applyTo`) al generar código.
>
> Además, Copilot [admite archivos de instrucciones con otros estándares][custom-instructions-support], como AGENTS.md y CLAUDE.md.

### Buenas prácticas para gestionar archivos de instrucciones

Una conversación completa sobre cómo crear archivos de instrucciones queda fuera del alcance del taller. Sin embargo, los ejemplos del proyecto muestran un enfoque representativo. A grandes rasgos:

- Mantén `copilot-instructions.md` centrado en directrices del proyecto, como su descripción, estructura y estándares generales de programación.
- Usa archivos `*.instructions.md` para tareas o tipos de archivo específicos, como las pruebas unitarias, los componentes Astro o la capa de datos.
- Usa lenguaje natural y directrices claras. Incluye ejemplos de cómo debería ser el código y de cómo no debería serlo.

No hay una única forma de crear archivos de instrucciones, igual que no hay una única forma de usar IA. La experimentación te ayudará a descubrir qué funciona mejor en cada proyecto.

> [!TIP]
> Todo proyecto que use GitHub Copilot debería tener una colección sólida de archivos de instrucciones. Al explorar los de este proyecto, verás que hay archivos para distintos tipos de tareas, incluidas las [actualizaciones de interfaz][ui-instructions] y el desarrollo con [Astro][astro-instructions].
>
> Copilot también puede ayudarte a generar archivos de instrucciones. Cada entorno ofrece esta opción de una forma distinta (por ejemplo, **Configure Chat → Generate Agent Instructions** en VS Code o `/init` en Copilot CLI); la lección correspondiente lo indicará cuando sea pertinente.
>
> Si buscas plantillas o un punto de partida, explora [awesome-copilot][awesome-copilot], un repositorio con archivos de instrucciones, agentes personalizados y otros recursos.

[ui-instructions]: https://github.com/github-samples/tailspin-toys/blob/main/.github/instructions/ui.instructions.md
[astro-instructions]: https://github.com/github-samples/tailspin-toys/blob/main/.github/instructions/astro.instructions.md
[awesome-copilot]: https://github.com/github/awesome-copilot
[custom-instructions-support]: https://docs.github.com/copilot/reference/custom-instructions-support

## Explorar los archivos de instrucciones personalizadas del proyecto

Dedica un momento a leer los archivos de instrucciones incluidos en este repositorio: hay un `copilot-instructions.md` principal y varios `*.instructions.md` para distintas tareas. Ábrelos en tu editor o en la interfaz web de GitHub.

1. Abre `.github/copilot-instructions.md`.
2. Explora el archivo y fíjate en la breve descripción del proyecto y en secciones como **Agent notes**, **Code standards**, **Scripts** y **Repository Structure**. En **Code standards**, fíjate en las directrices anidadas de **GitHub Actions Workflows**. Se aplican a cualquier interacción con Copilot.
3. Abre la carpeta `.github/instructions` y échale un vistazo. Verás instrucciones para archivos Astro, la capa de datos Drizzle, pruebas y otras tareas.
4. Abre `.github/instructions/unit-tests.instructions.md`. Fíjate en el campo `applyTo` de la parte superior: define un glob (relativo a la raíz del repositorio) que determina a qué archivos se aplican las instrucciones. En este caso, coincide con cualquier archivo de pruebas TypeScript (por ejemplo, los que coincidan con `**/*.test.ts`).
5. Observa las instrucciones específicas para crear pruebas unitarias en este proyecto.
6. Por último, abre `.github/instructions/drizzle.instructions.md` y desplázate hasta el final. Fíjate en los enlaces a otros archivos de instrucciones, como `unit-tests.instructions.md`, y a archivos existentes del proyecto. Así puedes dividir conjuntos de instrucciones grandes en archivos más pequeños y reutilizables, y proporcionar a Copilot ejemplos que seguir al generar código. (Las rutas indicadas son relativas al archivo de instrucciones, no a la raíz del repositorio).

> [!NOTE]
> La sección **Code formatting requirements** de `copilot-instructions.md` documenta los estándares de programación del proyecto, pero aún no exige documentación dentro del código. En los pasos siguientes añadirás reglas para comentarios TSDoc y encabezados de comentario a nivel de archivo.

## Añadir un nuevo estándar del repositorio

El siguiente paso es la única edición que harás aquí: añadirás una regla general para que la documentación se incluya en el código mediante comentarios TSDoc y encabezados de comentario a nivel de archivo. El agente en la nube la aplicará a las incidencias que le asignes más adelante en este recorrido.

Antes de editar, crea una rama de trabajo. El agente en la nube leerá las instrucciones de la rama a la que se dirija la incidencia, pero así también confirmarás los cambios de forma ordenada:

1. En el terminal de tu Codespace, crea una rama y cámbiate a ella:

   ```bash
   git checkout -b custom-instructions-cloud
   ```

Como se indicó antes, `.github/copilot-instructions.md` proporciona información del proyecto a Copilot. Vamos a documentar los estándares de programación del repositorio para mejorar las sugerencias.

1. Vuelve a abrir `.github/copilot-instructions.md`.
2. Busca la sección **Code formatting requirements**, que debería estar cerca de la línea 27. Observa que documenta los estándares de programación, pero aún no incluye reglas para la documentación en el código; por eso el helper generado no tenía comentarios de documentación.
3. Añade las siguientes líneas de Markdown justo debajo de los estándares existentes para indicar a Copilot que incluya encabezados de comentario a nivel de archivo y comentarios TSDoc:

   ```markdown
   - Every exported function should have a TSDoc comment describing its purpose, parameters, and return value.
   - Before imports or any code, add a comment block to the file that explains its purpose.
   ```

4. Guarda `copilot-instructions.md`.

> [!TIP]
> Como viste en la lección anterior, los archivos de instrucciones pueden crearse a nivel de repositorio (`.github/copilot-instructions.md`) para ofrecer directrices generales o como archivos `*.instructions.md` para lenguajes, tipos de archivo o tareas concretos. El archivo a nivel de repositorio es el lugar adecuado para estándares generales del proyecto, como la regla sobre los comentarios de documentación que acabas de añadir.

## Confirmar, enviar y fusionar la actualización de instrucciones

El agente en la nube lee los archivos de instrucciones de la rama a la que se dirige la incidencia. Cuando le asignes una incidencia a Copilot en el siguiente ejercicio, Copilot creará una rama a partir de `main`. Por tanto, las actualizaciones de instrucciones deben estar en `main` para que el agente pueda usarlas.

1. Prepara los cambios y confírmalos:

   ```bash
   git add .github/copilot-instructions.md
   git commit -m "Add doc comment and file-header standards to copilot instructions"
   ```

2. Envía la rama:

   ```bash
   git push -u origin custom-instructions-cloud
   ```

3. En github.com, abre un pull request de `custom-instructions-cloud` a `main` y fusiónalo. Así, el agente podrá leer estas instrucciones cuando trabaje en las incidencias que le asignes en el siguiente ejercicio.

> [!TIP]
> Si prefieres trabajar directamente en `main` durante este taller, puedes omitir los pasos de la rama y confirmar los cambios directamente en `main`. Creamos la rama para reflejar cómo se haría este trabajo en un proyecto real.

## Resumen y siguientes pasos

Has explorado cómo Copilot obtiene el contexto de los archivos de instrucciones del proyecto y has añadido un nuevo estándar general a `.github/copilot-instructions.md`. Verás cómo se aplica ese estándar en los pull requests que genere el agente durante el resto del recorrido.

A continuación, [asignarás tu primera incidencia a GitHub Copilot cloud agent][next-lesson].

## Recursos

- [Archivos de instrucciones para personalizar GitHub Copilot][instruction-files]
- [Buenas prácticas para crear instrucciones personalizadas][instructions-best-practices]
- [5 consejos para escribir mejores instrucciones personalizadas para Copilot][copilot-instructions-five-tips]
- [Awesome Copilot: colección de archivos de instrucciones y otros recursos][awesome-copilot]

[previous-lesson]: ../0-prerequisites/
[next-lesson]: ../2-cloud-agent/
[instruction-files]: https://docs.github.com/copilot/customizing-copilot/about-customizing-github-copilot-chat-responses
[instructions-best-practices]: https://docs.github.com/enterprise-cloud@latest/copilot/using-github-copilot/coding-agent/best-practices-for-using-copilot-to-work-on-tasks#adding-custom-instructions-to-your-repository
[copilot-instructions-five-tips]: https://github.blog/ai-and-ml/github-copilot/5-tips-for-writing-better-custom-instructions-for-copilot/
