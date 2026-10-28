---
title: "Ejercicio 2 - Añadir funcionalidades con el modo agente de Copilot"
authors:
  - geektrainer
lastUpdated: 2026-06-30
---

Incluso las actualizaciones más sencillas de una aplicación suelen requerir cambios en varios archivos y tareas como ejecutar pruebas. En el entorno de desarrollo integrado (IDE), el flujo de trabajo puede incluir localizar los archivos necesarios, hacer cambios, ejecutar pruebas, depurar, descubrir qué archivo falta y volver a modificarlo. Y la lista continúa.

El modo agente de Copilot ayuda con estas tareas.

El modo agente está diseñado para trabajar con mayor autonomía en el IDE. Se comporta de forma parecida a un desarrollador: explora la estructura del proyecto, realiza los cambios necesarios, ejecuta tareas como las pruebas y corrige automáticamente los errores que encuentra. Vamos a utilizarlo para añadir una funcionalidad al sitio.

> [!NOTE]
> Aunque los nombres se parecen, el modo agente y el agente en la nube ofrecen experiencias distintas. El modo agente trabaja en el IDE y permite ciclos de respuesta rápidos e interacción directa. El agente en la nube actúa como un compañero que trabaja de forma asíncrona con incidencias y solicitudes de incorporación de cambios.

En este ejercicio aprenderás cómo:

- el modo agente de Copilot explora el proyecto, identifica los archivos relevantes y coordina cambios;
- el modo agente de GitHub Copilot implementa funcionalidades en la interfaz de usuario y la capa de datos;
- revisar los cambios y las pruebas generados por el modo agente de Copilot antes de combinar el trabajo en el código base.

## Escenario

Como cada vez hay más juegos, quieres permitir que los usuarios filtren por categoría y editorial. En el ejercicio anterior creaste una función auxiliar para obtener las editoriales; ahora completarás el trabajo restante en la capa de datos, la interfaz de usuario y las pruebas con el modo agente de Copilot.

## Ejecutar el sitio web de Tailspin Toys

Antes de hacer cambios, explora el sitio web de Tailspin Toys para conocer sus funciones actuales.

El sitio web es una plataforma de financiación colectiva de juegos de mesa con temática de desarrollo. Permite mostrar una lista de juegos y ver sus detalles. El sitio es una única aplicación Astro que genera páginas como lenguaje de marcado de hipertexto (HTML) estático durante la compilación. Las páginas consultan directamente una base de datos SQLite local mediante el mapeador objeto-relacional (ORM) Drizzle desde sus metadatos iniciales; no hay una interfaz de programación de aplicaciones (API) de backend independiente ni un marco de trabajo de interfaz de usuario en el cliente. Las funciones auxiliares reutilizables para acceder a los datos se encuentran en `src/lib/`. La interactividad se añade mediante un pequeño `<script>` de Astro con alcance limitado y las API estándar del modelo de objetos del documento (DOM).

### Iniciar el sitio web

Para facilitar la ejecución del sitio web, se proporciona la tarea `npm run dev`, que inicia el único servidor de desarrollo de Astro. Ejecútala en el codespace de GitHub:

1. Vuelve al codespace. Deberías estar en `main` después del ejercicio anterior.
2. Abre un terminal nuevo en el codespace pulsando <kbd>Control</kbd>+<kbd>`</kbd> (Mac) o <kbd>Ctrl</kbd>+<kbd>`</kbd> (Windows/Linux).
3. Crea una rama para el trabajo de filtrado:

   ```bash
   git checkout main
   git pull
   git checkout -b filtering-vscode
   ```

4. Inicia el sitio web con este comando:

   ```bash
   npm run dev
   ```

   Cuando el servidor de desarrollo de Astro esté listo, aparecerá un mensaje con la URL, parecido a este:

   ```bash
   🚀 Tailspin Toys is ready!
      Astro server: http://localhost:4321
      Press Ctrl-C to stop.
   ```

> [!NOTE]
> Si aparece un cuadro de diálogo para abrir una ventana del navegador en `http://localhost:4321`, ciérralo seleccionando **x**.

5. Abre el sitio web manteniendo pulsado <kbd>Command</kbd> (Mac) o <kbd>Ctrl</kbd> (Windows/Linux) y seleccionando en el terminal la dirección del servidor Astro: `http://localhost:4321`.

> [!NOTE]
> En un codespace, al seleccionar el enlace a la URL local del terminal, se te redirigirá automáticamente a `https://<your-codespace-name>-4321.app.github.dev/`. Es un túnel privado al codespace, que ahora aloja el servidor web.

### Explorar el sitio web

Con el sitio en ejecución, puedes explorar sus funciones principales:

- **Página de inicio**: muestra juegos de mesa con sus títulos, imágenes y descripciones.
- **Página de detalles del juego**: al seleccionar un juego, se abre una página con más información, como el título, la descripción, la editorial y la categoría.

## Explorar la lista de tareas pendientes con Copilot

> [!TIP]
> **Abrir Copilot Chat**
>
> Antes de empezar los ejercicios siguientes, vuelve al codespace, abre el panel Copilot Chat y selecciona **New Chat** para iniciar una conversación nueva. El modo y el modelo varían según el ejercicio; cada paso lo indicará cuando sea necesario.

La implementación inicial del sitio funciona, pero queremos añadir capacidades. Empezaremos revisando la lista de tareas pendientes. Al crear el proyecto a partir de la plantilla, se generó automáticamente una lista de incidencias. Pide a GitHub Copilot que te muestre esos elementos.

1. Selecciona **Agent** en el menú desplegable de agentes de la vista Chat. El agente planifica e implementa cambios en varios archivos de forma autónoma, ejecuta comandos de terminal y utiliza herramientas.

   ![Captura del selector de agentes en la vista Chat](../../../_images/shared-chat-mode-selector.png)

2. Selecciona **Claude Sonnet 4.5** en la lista de modelos disponibles.

> [!CAUTION]
> Los autores de este taller no expresan preferencia por ningún modelo. Al crear el taller, utilizamos Claude Sonnet 4.5 y lo indicamos para que las sugerencias de código sean relativamente coherentes y la experiencia sea más uniforme. Sin embargo, como los modelos de lenguaje grandes (LLM) son probabilísticos, es posible que recibas sugerencias distintas a las descritas. Es normal y previsible.

> [!NOTE]
> Debido al carácter probabilístico de los modelos de lenguaje grandes (LLM), Copilot podría utilizar un comando de Model Context Protocol (MCP) diferente y aun así completar la tarea.

3. Envía este mensaje a Copilot para que muestre las incidencias pendientes:

   ```plaintext
   Please show me the backlog of items from my GitHub repository. Help me prioritize them based on those which will be most useful to the user.
   ```

4. Selecciona **Continue** para ejecutar el comando que enumera todas las incidencias.
5. Revisa la lista de incidencias generada.

Observa que Copilot también ha priorizado los elementos según lo que considera más útil para los usuarios.

## Revisar los archivos de instrucciones

Antes de pedir al agente que genere código, revisa el archivo de instrucciones que proporcionará contexto para su trabajo. Utilizarás el archivo de [interfaz de usuario (UI)](https://github.com/github-samples/tailspin-toys/blob/main/.github/instructions/ui.instructions.md), que explica cómo añadir funcionalidades al sitio web.

1. En el codespace, abre `.github/instructions/ui.instructions.md`.
2. Fíjate en las pautas generales para añadir funcionalidades, entre ellas:
   - una descripción general de la arquitectura;
   - principios de diseño de componentes, capacidad de prueba y accesibilidad;
   - enlaces a archivos de instrucciones específicos para varios tipos de archivos, como Astro, Tailwind CSS (hojas de estilo en cascada) y Drizzle.

> [!TIP]
> Los archivos de instrucciones pueden hacer referencia a otros archivos de instrucciones y a archivos del proyecto. Las rutas son relativas al archivo de instrucciones. Así se pueden reutilizar pautas, dividir instrucciones complejas en archivos más manejables e incluir ejemplos y plantillas.

## Implementar el filtrado

Para completar el filtrado, habrá que realizar al menos tres cambios distintos en la aplicación:

- añadir o ajustar la lógica restante de filtrado en la capa de datos (`src/lib/`);
- añadir o actualizar las pruebas del comportamiento de filtrado;
- actualizar la página de la lista de juegos para incluir la interfaz de filtrado.

Además, las pruebas deben ejecutarse y superarse antes de combinar los cambios en el código base. El modo agente de Copilot puede realizar estas tareas.

1. Continúa la conversación actual con Copilot o selecciona **New Chat** para iniciar otra.
2. Selecciona **Add Context**, **Instructions** y **ui** para añadir el archivo de instrucciones.

   ![Captura de cómo seleccionar el archivo de instrucciones de interfaz de usuario](../../../_images/ex3-select-instructions-file.png)

3. Comprueba que **Agent** sigue seleccionado en el menú desplegable de agentes de la vista Chat.

   ![Captura del selector de agentes en la vista Chat](../../../_images/shared-chat-mode-selector.png)

4. Comprueba que **Claude Sonnet 4.5** sigue seleccionado como modelo.
5. Pide a Copilot que implemente la funcionalidad descrita en la incidencia relacionada de la lista de tareas:

   ```plaintext
   Please update the site to include filtering by publisher and category based on the requirements from the related GitHub issue in the backlog. A publishers helper already exists from the previous exercise, so preserve and refine that work as needed while completing the remaining data-layer, UI, and tests. Ensure all tests are passing before completion. The server is already running, so you do not need to start it up.
   ```

6. Observa cómo Copilot explora el proyecto y localiza los archivos relacionados con la funcionalidad: funciones auxiliares de la capa de datos, interfaz de usuario y pruebas. Después modifica los archivos y ejecuta las pruebas.

   ![Captura de Copilot explorando archivos del proyecto](../../../_images/ex3-agent-mode-explores.png)

> [!NOTE]
> Copilot realizará varias tareas, como explorar el proyecto, modificar archivos y ejecutar pruebas. El proceso puede tardar unos minutos según la complejidad de la tarea y del código base. Mientras trabaja, es posible que aparezcan los botones **Keep** y **Undo** en el editor. Cuando termine, tendrás la opción de conservar o deshacer todos los cambios; no tienes que seleccionar esos botones mientras el agente sigue trabajando.

7. Cuando Copilot te lo solicite, selecciona **Continue** para ejecutar las pruebas.

   ![Captura de un cuadro de diálogo de Copilot Chat que pide confirmar la ejecución de pruebas](../../../_images/ex3-agent-mode-run-tests.png)

8. Es posible que haya pausas y que algunas pruebas fallen durante el proceso. No pasa nada: Copilot alterna entre generar código y ejecutar pruebas hasta completar la tarea y dejar de detectar errores.

   ![Captura de una sesión completa de Copilot Chat con el modo agente](../../../_images/ex3-agent-mode-proposed-changes.png)

9. Explora el código generado para detectar posibles problemas.

> [!CAUTION]
> Revisa siempre el código generado por Copilot o cualquier otra herramienta de IA.

10. Vuelve al navegador con el sitio en ejecución y explora la nueva funcionalidad.
11. Cuando hayas confirmado que todo funciona y revisado el código, selecciona **Keep** en la ventana de Copilot Chat.

## Resumen y pasos siguientes

En este ejercicio has explorado cómo utilizar el modo agente de GitHub Copilot para añadir funcionalidades a Tailspin Toys. Has aprendido cómo:

- el modo agente de GitHub Copilot puede implementar funcionalidades en la interfaz de usuario y la capa de datos;
- el modo agente de Copilot puede explorar el proyecto, identificar los archivos relevantes y coordinar cambios;
- revisar los cambios y las pruebas generados por el modo agente de Copilot antes de combinar el trabajo en el código base.

A continuación, [probarás la funcionalidad con el servidor MCP de Playwright][next-lesson] y abrirás una solicitud de incorporación de cambios.

### Ejercicio de exploración adicional: implementar paginación

Cuando aumenta la lista de juegos, puede que sea necesario habilitar la paginación. Utiliza lo aprendido en este ejercicio para pedir a Copilot que la implemente en el sitio. Al definir la petición:

- sigue las prácticas recomendadas y utiliza los archivos de instrucciones existentes;
- decide cómo quieres implementar la paginación: por ejemplo, si el usuario podrá elegir el tamaño de página o si será fijo;
- proporciona a Copilot las pautas necesarias para que implemente el comportamiento que esperas;
- itera con GitHub Copilot y pide cambios adicionales cuando sea necesario. Es lo habitual al trabajar con Copilot.

## Recursos

- [Modos ask, edit y agent de Copilot: qué hacen y cuándo utilizarlos][choose-mode]
- [Modo agente en VS Code][vs-code-agent-mode]

[previous-lesson]: ../1-custom-instructions/
[next-lesson]: ../3-mcp/
[choose-mode]: https://github.blog/ai-and-ml/github-copilot/copilot-ask-edit-and-agent-modes-what-they-do-and-when-to-use-them/
[vs-code-agent-mode]: https://code.visualstudio.com/docs/copilot/chat/chat-agent-mode
