---
title: "Ejercicio 2 - Agente de GitHub Copilot en la nube"
authors:
  - geektrainer
lastUpdated: 2026-06-30
---

| [← Lección anterior: Instrucciones personalizadas][previous-lesson] |
|:--|

Es probable que casi todas las organizaciones tengan problemas de deuda técnica. Puede tratarse de problemas de seguridad sin resolver, código heredado que necesita actualizarse o solicitudes de funcionalidad que llevan tiempo en la lista de tareas pendientes por falta de tiempo. El agente de GitHub Copilot en la nube está diseñado para tareas como actualizar código y añadir funcionalidades de forma autónoma. Cuando termina, crea una solicitud de incorporación de cambios (PR) en borrador para que la revise un desarrollador. Así puedes delegar tareas repetitivas, acelerar el desarrollo y centrarte en asuntos de mayor alcance.

Con el agente de GitHub Copilot en la nube explorarás cómo:

- personalizar el entorno de generación de código;
- garantizar que las operaciones se realicen de forma segura;
- definir incidencias con un alcance claro;
- asignar incidencias a Copilot.

## Escenarios

Tailspin Toys tiene deuda técnica pendiente. Los contratistas que crearon la primera versión del sitio dejaron la documentación en mal estado: comprobarás que no hay ninguna. Como primer paso, quieren añadir comentarios de TypeScript Documentation (TSDoc) a todas las funciones exportadas de la aplicación.

Además, el equipo de diseño quiere mejorar la búsqueda de juegos. Quiere mostrar juegos relacionados —otros títulos de la misma categoría— en la página de detalles de cada juego para que la gente pueda seguir explorando. No necesita una implementación refinada todavía; solo quiere probar la experiencia de usuario. Aunque ahora mismo es un impedimento, hay otras incidencias con mayor prioridad.

Son ejemplos de tareas que pueden quedar relegadas y que puedes asignar al agente de GitHub Copilot en la nube. El agente trabajará en segundo plano mientras te ocupas de otras tareas. Después podrás volver para revisar su trabajo y comprobar que el resultado es correcto.

## Presentación del agente de GitHub Copilot en la nube

El [agente de GitHub Copilot en la nube](https://docs.github.com/copilot/concepts/agents/cloud-agent/about-cloud-agent#overview-of-copilot-cloud-agent-formerly-copilot-coding-agent) puede realizar tareas en segundo plano de forma parecida a un desarrollador. Como ocurre al trabajar con otra persona, hay varias formas de asignarle trabajo, entre ellas [asignarle una incidencia de GitHub](https://docs.github.com/copilot/how-tos/use-copilot-agents/cloud-agent/start-copilot-sessions). Cuando se le asigna una incidencia, Copilot crea una solicitud de incorporación de cambios en borrador para registrar el progreso, configura un entorno y comienza a trabajar. Puedes consultar la sesión mientras sigue activa o después de que termine. Cuando la solución esté lista para revisarla, Copilot te mencionará en la solicitud de incorporación de cambios.

## La importancia de definir bien el alcance

GitHub Copilot no hace magia: no puedes dejar de pensar. Incluso las tareas aparentemente sencillas pueden tener más complejidad de la prevista, así que [define con cuidado el alcance del trabajo del agente en la nube][cloud-agent-best-practices]. Trátalo como a un compañero de programación con IA: trabaja por etapas, aprende, experimenta y adapta el enfoque. Los fundamentos del desarrollo de software no cambian al incorporar IA generativa.

## Instrucciones personalizadas en este repositorio

En ejercicios anteriores has conocido las instrucciones personalizadas y cómo orientan a Copilot. Si ya has completado el ejercicio práctico sobre instrucciones personalizadas, habrás visto `.github/copilot-instructions.md` en acción. Si no, este es un buen momento para echarle un vistazo. Antes de asignar trabajo al agente, revisa los archivos de instrucciones incluidos en este repositorio, sin modificarlos, para que después puedas identificar su efecto.

Abre los siguientes archivos en la interfaz web de GitHub o en un codespace que ya tengas en ejecución:

- `.github/copilot-instructions.md`: revisa la sección **Code standards**, especialmente las convenciones de TypeScript y los requisitos de comentarios TSDoc.
- `.github/instructions/unit-tests.instructions.md`: fíjate en los metadatos `applyTo`, que limitan estas instrucciones a los archivos `**/*.test.ts`.

Cuando asignes al agente la incidencia `Code lacks documentation` en la sección siguiente, comprueba si la solicitud de incorporación de cambios resultante incluye comentarios TSDoc, sigue las convenciones de TypeScript y añade encabezados de comentarios. Estas indicaciones proceden de los archivos de instrucciones que acabas de revisar. Volveremos a señalarlas al revisar las PR en un ejercicio posterior.

## Configurar el entorno de desarrollo del agente en la nube

Para crear código suele ser necesario configurar el entorno y ejecutar scripts de preparación. Lo mismo se aplica al asignar tareas a Copilot, que trabaja de forma parecida a un ingeniero de software (SWE).

El agente en la nube utiliza [GitHub Actions][github-actions] como entorno de trabajo. Puedes personalizarlo mediante un [flujo de trabajo de configuración especial][setup-workflow], definido en `.github/workflows/copilot-setup-steps.yml`, que se ejecuta antes de que el agente empiece a trabajar. Así puede acceder a las herramientas de desarrollo y dependencias necesarias. El flujo ya está configurado para facilitar el taller: proporciona Node.js, las dependencias del proyecto, los binarios del navegador para las pruebas de extremo a extremo y la base de datos SQLite migrada y con datos iniciales de la aplicación Astro única:

```yaml
name: "Copilot Setup Steps"

# Permite probar los pasos de configuración desde la pestaña "Actions" del repositorio
on: workflow_dispatch

env:
  ASTRO_TELEMETRY_DISABLED: "1"

jobs:
  copilot-setup-steps:
    runs-on: ubuntu-latest
    # Define los permisos mínimos necesarios para estos pasos. Copilot utilizará su propio token para las operaciones.
    permissions:
      # Si los pasos de configuración clonan el repositorio, por ejemplo para instalar dependencias, necesitarás el permiso `contents: read`. Si no lo clonan, Copilot lo hará automáticamente cuando terminen los pasos.
      contents: read
    steps:
      - name: Checkout code
        uses: actions/checkout@v5

      # Preparación de la aplicación: Node.js (la aplicación completa ahora utiliza Astro y Drizzle/libSQL)
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

      # Migra y carga la base de datos SQLite local para que las compilaciones y pruebas dispongan de datos.
      - name: Set up the database
        run: npm run db:setup
```

El archivo se parece a cualquier otro flujo de trabajo de GitHub, pero tiene algunos aspectos importantes:

- Contiene un único trabajo llamado `copilot-setup-steps`. GitHub Actions lo ejecuta antes de que Copilot empiece a trabajar en la solicitud de incorporación de cambios.
- El activador `workflow_dispatch` permite ejecutar manualmente el flujo de trabajo desde la pestaña **Actions** del repositorio. Resulta útil para comprobar que funciona sin esperar a que Copilot lo ejecute.

## Añadir documentación

Todos reconocen la importancia de la documentación, pero muchos proyectos tienen información obsoleta o carecen de ella. Esta deuda técnica puede ralentizar el trabajo y dificultar el mantenimiento del código o la incorporación de nuevos miembros al equipo. Copilot destaca en la generación de documentación, así que esta es una buena incidencia para asignar al agente en la nube. Trabajará en segundo plano y podrás revisar el resultado en un ejercicio posterior.

1. En una nueva pestaña del navegador, ve al repositorio en github.com.
2. Selecciona la pestaña **Issues**.
3. Selecciona **New issue** para abrir el cuadro de diálogo de incidencias.
4. Selecciona **Blank issue** para crear una incidencia en blanco.
5. Escribe `Code lacks documentation` en el campo **Title**.
6. Escribe lo siguiente en **Description**:

    ```plaintext
    Our organization has a requirement that functions and methods include TSDoc doc comments where helpful. Unfortunately, recent updates haven't followed this standard. We need to update the existing code to ensure doc comments are included where they clarify behavior.
    ```

7. Selecciona **Create** para crear la incidencia.
8. A la derecha, selecciona **Assign to Copilot** para abrir el cuadro de diálogo de asignación.

  ![Asignación de Copilot a una incidencia](../../../_images/shared-assign-copilot.png)

9. Selecciona **Assign**.
10. Selecciona la pestaña **Pull Requests**.
11. Abre la solicitud de incorporación de cambios recién creada, cuyo título será similar a `[WIP]: Code lacks documentation`. Si todavía no aparece en la lista, espera un momento y actualiza la ventana del navegador.
12. Al cabo de unos minutos, Copilot debería crear una lista de tareas.

> [!NOTE]
> La lista de tareas puede tardar varios minutos en aparecer en la PR. Copilot está preparando el entorno —ejecutando el flujo de trabajo descrito anteriormente—, analizando el proyecto y determinando cómo abordar la tarea.

13. Revisa la lista y las tareas que va a completar.
14. Desplázate por la cronología de la PR. Deberías ver una actualización que indica que Copilot ha empezado a trabajar en la incidencia.
15. Selecciona el botón **View session**.

  ![Vista de la sesión de Copilot](../../../_images/ex4-view-session.png)

> [!CAUTION]
> Puede que tengas que actualizar la ventana para ver el indicador actualizado.

16. Observa la sesión en directo y cómo Copilot resuelve el problema. Verás que explora el código y comprende el estado del proyecto, se detiene a pensar y decidir el plan adecuado y, después, crea código.

El proceso puede tardar varios minutos. Una de las principales ventajas del agente en la nube es que trabaja de forma asíncrona y te permite ocuparte de otras tareas. Aprovecharemos esa capacidad: asignaremos otra tarea al agente y, mientras trabaja, nos centraremos en añadir funcionalidades a la aplicación.

## Añadir una sección de juegos relacionados a la página de detalles

Una ventaja del agente de GitHub Copilot en la nube es que puedes repartir el trabajo y centrarte en unas tareas mientras él se ocupa de otras. Aunque añadir una sección de juegos relacionados quizá no lleve mucho tiempo, puedes aprovechar ese tiempo para otras tareas. Vamos a asignársela al agente.

1. Vuelve al repositorio en github.com.
2. Selecciona la pestaña **Issues**.
3. Selecciona **New issue** para abrir el cuadro de diálogo de incidencias.
4. Selecciona **Blank issue** para utilizar la plantilla en blanco.
5. Escribe `Show related games on the game details page` en el campo **Title**.
6. Escribe lo siguiente en **Description**:

    ```markdown
    We want to help people discover more games by showing related games on each game's details page. The design team wants to explore the UX and do some acceptance testing. Our requirements are:

   - Add a data-access helper in `src/lib/` that returns other games in the same category as a given game, excluding that game
   - Show a "Related games" section on the game details page that uses the helper
   - Handle the case where a game has no related games
   - There should be unit tests created for the new helper
   - Before creating the PR, ensure all tests pass
    ```

7. Selecciona **Create** para crear la incidencia.
8. A la derecha, selecciona **Assign to Copilot** para abrir el cuadro de diálogo de asignación.

  ![Asignación de Copilot a una incidencia](../../../_images/shared-assign-copilot.png)

9. Selecciona **Assign**.

Al poco tiempo, verás el emoji 👀 en el primer comentario de la incidencia. Indica que Copilot ya está trabajando.

![Copilot utiliza el emoji de ojos para indicar que está trabajando en la incidencia](../../../_images/ex4-issue-eyes-emoji.png)

Copilot ya está trabajando en la segunda solicitud. El agente en la nube funciona de manera parecida a un ingeniero de software, así que no hace falta que lo supervises continuamente. Puedes revisar el resultado cuando termine. Mientras tanto, vamos a crear y utilizar agentes personalizados.

## Resumen y pasos siguientes

En esta lección has explorado el [agente de GitHub Copilot en la nube][copilot-agents]. Puedes asignarle incidencias para que trabaje de forma asíncrona. Copilot puede ayudarte a resolver deuda técnica, crear funcionalidades o migrar código de un marco de trabajo a otro.

Has explorado estos conceptos:

- personalizar el entorno de generación de código;
- garantizar que las operaciones se realicen de forma segura;
- definir incidencias con un alcance claro;
- asignar incidencias a Copilot.

Mientras el agente en la nube trabaja, puedes crear y utilizar agentes personalizados. [El agente en la nube también puede utilizar servidores de Model Context Protocol (MCP)][cloud-agent-mcp] y puede acceder a las instrucciones personalizadas que has explorado en ejercicios anteriores.

## Recursos

- [Acerca del agente de GitHub Copilot en la nube][copilot-agents]
- [Asignar incidencias de GitHub a Copilot][assign-issue]
- [Prácticas recomendadas para el flujo de configuración del agente en la nube][cloud-agent-best-practices]

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
