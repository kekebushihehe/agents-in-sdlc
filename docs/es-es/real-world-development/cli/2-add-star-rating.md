---
title: "Lección 2 - Añadir valoraciones por estrellas: un cambio rápido"
description: "Usa Copilot CLI para hacer un pequeño cambio en las tarjetas de juegos, revísalo en un navegador con el puerto reenviado y combínalo en tu primera solicitud de incorporación de cambios."
authors:
  - geektrainer
lastUpdated: 2026-09-18
---

Ahora que has instalado Copilot CLI y probado una conversación, harás tu primer cambio en el proyecto. Será pequeño: los juegos ya tienen una valoración por estrellas en sus datos, pero aún no aparece en las tarjetas de la página principal. Pedirás al agente que la muestre, revisarás el cambio y lo combinarás en tu primera solicitud de incorporación de cambios (PR).

En esta lección:

- iniciarás una conversación específica de Copilot en una rama de funcionalidad;
- pedirás al agente que haga un pequeño cambio en el proyecto;
- revisarás el cambio con `/diff`;
- ejecutarás la aplicación para comprobar el cambio en un navegador con el puerto reenviado;
- abrirás y combinarás tu primera PR.

## Escenario

Cada juego de Tailspin Toys puede tener una valoración por estrellas, que ya aparece en la página de detalles. Sin embargo, las tarjetas de la página principal solo muestran el título, la categoría, la editorial y la descripción. Como calentamiento, pedirás al agente que muestre en cada tarjeta la valoración existente: es un cambio pequeño y autónomo, perfecto para tu primera sesión.

## Anatomía de una conversación

Una **conversación** es el espacio donde trabajas con Copilot CLI en una tarea. A diferencia de la aplicación Copilot, una conversación normal de CLI usa el repositorio y la rama de Git seleccionados en el terminal, en lugar de crear un árbol de trabajo (worktree) dedicado. Las conversaciones guardadas te permiten volver a la misma conversación más tarde, mientras que los archivos y la rama siguen siendo el estado normal de Git en el disco.

En una conversación verás tres elementos: tus prompts y las respuestas del agente, la actividad de las herramientas mientras explora y edita archivos, y los cambios que puedes inspeccionar con `/diff`.

## Iniciar una conversación y pedir el cambio

Empezarás una conversación nueva para implementar la funcionalidad.

1. Vuelve a tu codespace.
2. Si el terminal no sigue abierto, pulsa <kbd>Control</kbd>+<kbd>\`</kbd> (Mac) o <kbd>Ctrl</kbd>+<kbd>\`</kbd> (Windows/Linux).
3. Si Copilot CLI aún no está abierto, inicia Copilot con este comando:

   ```bash
   copilot --yolo
   ```

4. Comprueba que se inicia una sesión nueva con el comando de barra `/new` y pulsa <kbd>Enter</kbd>.
5. Usa este prompt para pedir el cambio:

   ```plaintext
   Show each game's starRating out of 5 in the game cards on the list page. If the rating is null, show "No rating yet". Keep the card layout as it is, add tests, and run the relevant checks.
   ```

Copilot explorará el proyecto, localizará los archivos que muestran los detalles de los juegos y creará el código necesario. Así habrás añadido una funcionalidad con Copilot CLI.

## Revisar las diferencias

Revisa todos los cambios generados por IA antes de combinarlos, aunque sean pequeños. Puedes inspeccionarlos directamente en Copilot CLI.

1. Escribe `/diff` e inspecciona todos los archivos modificados.
2. Confirma que las tarjetas muestran la valoración numérica cuando existe y `No rating yet` cuando `starRating` es `null`.
3. Confirma que las pruebas cubren ambos casos.
4. Revisa los resultados de las comprobaciones que haya ejecutado Copilot y pídele que corrija los errores.
5. Cuando termines la revisión, pulsa <kbd>Esc</kbd> para salir de la vista de diferencias.

> [!NOTE]
> Como todas las herramientas de IA generativa, Copilot es probabilístico, no determinista, por lo que el código exacto puede variar. Revisa el comportamiento en lugar de esperar una implementación concreta.

## Comprobar los cambios

No basta con leer el código y dar por hecho que funciona. Pide a Copilot que inicie el sitio web para poder examinar la interfaz de usuario (UI) en el navegador mediante el puerto reenviado de Codespaces.

1. Pide a Copilot que inicie la aplicación:

   ```plaintext
   Start the app so I can inspect the star-rating change in my browser. Tell me the URL and leave the server running.
   ```

2. Cuando Codespaces indique que el puerto `4321` está disponible, selecciona **Open in Browser**.
3. Confirma que las tarjetas muestran las valoraciones sobre cinco.
4. Vuelve a Copilot y pídele que detenga el servidor que ha iniciado:

   ```plaintext
   Stop the development server you started.
   ```

## Abrir y combinar tu primera solicitud de incorporación de cambios

Ya has creado la funcionalidad. Ahora crearás una solicitud de incorporación de cambios (PR) para integrar el código en el proyecto.

1. Pide al agente predeterminado que confirme el cambio:

   ```plaintext
   Commit the reviewed star-rating changes with an appropriate commit message.
   ```

2. Escribe `/pr create`. Cuando Copilot CLI cree la PR, enviará la confirmación existente y mostrará la URL de la PR.
3. Abre la PR manteniendo pulsada la tecla <kbd>Command</kbd> (Mac) o <kbd>Ctrl</kbd> (Windows/Linux) y seleccionando la URL que muestra Copilot CLI.
4. Revisa los archivos modificados y las comprobaciones.
5. Cuando esté todo listo, selecciona **Merge pull request** y confirma la combinación.
6. Vuelve a tu codespace y sal de Copilot CLI con `/exit`.
7. Actualiza tu rama local `main`:

   ```bash
   git checkout main
   git pull
   ```

## Resumen y pasos siguientes

¡Enhorabuena! Has publicado tu primer cambio con GitHub Copilot CLI. En concreto:

- has iniciado una conversación específica de Copilot en una rama de funcionalidad;
- has pedido al agente que haga un pequeño cambio en las tarjetas de juegos;
- has revisado el cambio con `/diff`;
- has ejecutado la aplicación y comprobado la valoración por estrellas en un navegador con el puerto reenviado;
- has abierto y combinado tu primera PR.

A continuación, [empezarás con la incidencia de filtrado y usarás los modos Plan y Autopilot][next-lesson] para crear una funcionalidad más amplia.

## Recursos

- [Acerca de GitHub Copilot CLI][about-copilot-cli]
- [Referencia de comandos de Copilot CLI][cli-reference]

[previous-lesson]: ../1-install-copilot-cli/
[next-lesson]: ../3-agent-modes/
[about-copilot-cli]: https://docs.github.com/copilot/concepts/agents/about-copilot-cli
[cli-reference]: https://docs.github.com/copilot/reference/copilot-cli-reference/cli-command-reference
