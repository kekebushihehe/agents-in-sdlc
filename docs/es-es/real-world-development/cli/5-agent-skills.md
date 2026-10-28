---
title: "Lección 5 - Personalizar y usar una habilidad de comprobaciones de calidad"
description: "Explora la habilidad existente quality-checks, personaliza el formato de su informe y úsala para validar el filtrado."
authors:
  - geektrainer
lastUpdated: 2026-09-29
---

Escribir código implica mucho más que programarlo. Ya hemos comprobado manualmente que funciona y hemos usado archivos de instrucciones para asegurarnos de que cumple nuestros estándares. Pero ¿qué ocurre con las pruebas, el lint y el resto de tareas de integración continua (CI)?

Para este tipo de tareas, las **habilidades de agente** son la opción ideal. Ayudan a Copilot a entender cómo realizar operaciones como estas.

En esta lección:

- explorarás la habilidad existente `quality-checks`;
- personalizarás el formato de los resultados;
- volverás a cargar la habilidad y la ejecutarás.

## Escenario

Tailspin Toys usa la habilidad `quality-checks` para ejecutar pruebas unitarias, lint y comprobaciones de tipos. El equipo quiere mejorar el informe para que los resultados sean más fáciles de leer.

## Instrucciones, scripts y recursos

Las habilidades de agente agrupan instrucciones reutilizables para tareas, scripts ejecutables y recursos auxiliares que un agente carga bajo demanda. En esencia, son una carpeta con el nombre de la habilidad y un archivo Markdown llamado `SKILL.md`. El Markdown incluye frontmatter con un nombre y una descripción que definen la habilidad, además de una introducción sobre su función y pautas sobre cuándo debe utilizarse. La carpeta también puede incluir subcarpetas con scripts y otros recursos que la habilidad puede usar.

> [!NOTE]
> No es obligatorio incluir archivos ni carpetas adicionales. La habilidad `quality-checks` de Tailspin Toys solo contiene `SKILL.md` porque utiliza los comandos existentes del proyecto.

Las habilidades pueden estar en la carpeta `.github/skills` del proyecto para que el equipo las comparta y reutilice como recursos del repositorio, o en la carpeta de habilidades del usuario, `~/.copilot/skills`.

## Explorar la habilidad

Vamos a explorar la habilidad `quality-checks` que creó el equipo de Tailspin Toys para ejecutar pruebas unitarias, lint y comprobaciones de tipos.

1. Vuelve a tu codespace. En el editor de Codespaces, abre `.github/skills/quality-checks/SKILL.md`.
2. Lee el `name` y la `description` de la parte superior. La descripción ayuda a Copilot a decidir cuándo utilizar la habilidad.
3. Lee las instrucciones y observa cómo guían a Copilot durante las pruebas y el lint.
4. Fíjate en que la habilidad aún no tiene una sección **Results output formatting**.

## Ejecutar la habilidad antes de hacer cambios

Puedes invocar las habilidades directamente desde Copilot CLI o mediante lenguaje natural. Vamos a pedirle a Copilot que ejecute las tres comprobaciones de la habilidad.

1. Vuelve a la conversación sobre el filtrado en modo interactivo.
2. Usa este prompt:

   ```plaintext
   Run the quality-checks skill for unit tests, lint, and type checks.
   ```

3. Revisa el informe que aparece al final.

## Personalizar el informe

Ahora vamos a mejorar el informe para que indique qué se ejecutó, si se completó correctamente y qué resultados comunicaron realmente las herramientas.

1. Vuelve a `.github/skills/quality-checks/SKILL.md`.
2. Añade al final del archivo esta sección:

   ```markdown
   ## Results output formatting

   Upon completion, report each command that ran and whether it passed, failed, or was blocked. Include test counts, durations, errors, warnings, and other metrics only when the tool reports them. Identify the next action for any failure or blocker, and never describe a skipped or incomplete check as passed.
   ```

3. El archivo se guarda automáticamente.

## Ejecutar la habilidad actualizada

Ahora que has actualizado la habilidad, comprueba el resultado. Copilot CLI puede volver a cargar las habilidades editadas sin reiniciar la conversación.

1. Escribe:

   ```plaintext
   /skills reload
   ```

2. Usa el mismo prompt que antes:

   ```plaintext
   Run the quality-checks skill for unit tests, lint, and type checks.
   ```

3. Revisa el informe final y compáralo con el primero.

## Resumen y pasos siguientes

Has personalizado y utilizado una habilidad de agente existente. En esta lección:

- has explorado la habilidad `quality-checks` para las pruebas unitarias, el lint y las comprobaciones de tipos;
- has personalizado el formato de sus resultados;
- has vuelto a cargar y ejecutado la habilidad.

Ese cambio acompañará al filtrado en la solicitud de incorporación de cambios (PR) de la funcionalidad. A continuación, permitirás que Copilot interactúe directamente con el sitio [mediante el servidor de Playwright de Model Context Protocol (MCP)][next-lesson].

## Otros ejemplos de habilidades

Estos ejemplos de la comunidad son referencias, no tareas adicionales:

- [Especificación de Agent Skills][skill-spec]
- [Flujo de contribución: `make-repo-contribution`][contribution-example]
- [Documentos de requisitos: `prd`][prd-example]
- [Diagramas y un script de exportación incluido: `drawio`][drawio-example]
- [Pruebas en el navegador: `webapp-testing`][browser-example]

[previous-lesson]: ../4-custom-instructions/
[next-lesson]: ../6-mcp-playwright/
[skill-spec]: https://agentskills.io/specification
[contribution-example]: https://github.com/github/awesome-copilot/tree/main/skills/make-repo-contribution
[prd-example]: https://github.com/github/awesome-copilot/tree/main/skills/prd
[drawio-example]: https://github.com/github/awesome-copilot/tree/main/skills/drawio
[browser-example]: https://github.com/github/awesome-copilot/tree/main/skills/webapp-testing
