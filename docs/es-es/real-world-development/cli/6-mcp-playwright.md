---
title: "Lección 6 - Validar la funcionalidad con Playwright MCP"
description: "Inspecciona o configura Playwright MCP en Copilot CLI y úsalo para explorar el filtrado en un navegador."
authors:
  - geektrainer
lastUpdated: 2026-09-18
---

Como ya hemos señalado, programar no consiste solo en escribir código. También hay que trabajar con datos y servicios externos, y permitir que Copilot use automatizaciones adicionales. Para ello sirven los servidores MCP: amplían las funciones integradas en la CLI con más herramientas y servicios.

En esta lección vas a:

- aprender qué es Model Context Protocol (MCP) y cómo lo usa Copilot CLI;
- añadir el servidor Playwright MCP si todavía no está disponible;
- pedir al agente que controle un navegador y explore el filtrado.

## Escenario

Las pruebas unitarias y de extremo a extremo son importantes, pero para validar cambios en la interfaz hay que interactuar con ella. Quieres que Copilot pueda usar el sitio web como lo haría una persona y automatizar más tareas, para tener mayor confianza en que los cambios funcionan según lo previsto.

## ¿Qué es Model Context Protocol (MCP)?

[Model Context Protocol (MCP)][mcp-blog-post] permite que los agentes de IA se comuniquen con herramientas y servicios externos en tiempo real. Así pueden acceder a información actualizada y realizar acciones en tu nombre.

Se accede a estas herramientas y recursos mediante un servidor MCP, que actúa como puente entre el agente de IA y los servicios y herramientas externos. Cada servidor MCP representa un conjunto distinto de herramientas y recursos a los que puede acceder el agente.

Algunos servidores MCP conocidos son:

- **[GitHub MCP Server][github-mcp]**: proporciona acceso a las API para gestionar repositorios, incidencias y pull requests de GitHub.
- **[Playwright MCP Server][playwright-mcp-server]**: proporciona funciones de automatización del navegador mediante Playwright.

Hay muchos otros servidores MCP disponibles. GitHub aloja un [registro de MCP][mcp-registry] que facilita descubrir servidores y contribuir al ecosistema.

> [!CAUTION]
> Trata los servidores MCP como cualquier otra dependencia del proyecto. Antes de usar uno, revisa su código fuente, comprueba quién lo publica y ten en cuenta sus implicaciones de seguridad.

## Añadir el servidor Playwright MCP

Si el servidor Playwright MCP todavía no está disponible, añádelo para que Copilot pueda interactuar con el sitio como lo haría un usuario.

> [!NOTE]
> Si Playwright MCP ya está configurado en Copilot CLI, omite estos pasos y continúa con la sección siguiente.

1. Vuelve a tu Codespace.
2. En Copilot CLI, escribe el siguiente comando para abrir el cuadro de diálogo para añadir un servidor MCP:

   ```plaintext
   /mcp add
   ```

3. Escribe `playwright` como nombre y pulsa <kbd>Tab</kbd>.
4. Confirma que el tipo de servidor sea **STDIO** pulsando <kbd>Enter</kbd> y, después, pulsa <kbd>Tab</kbd>.
5. Pega lo siguiente en el cuadro de diálogo **Command**:

   ```plaintext
   npx -y @playwright/mcp@latest --headless --no-sandbox
   ```

6. Pulsa <kbd>Control</kbd>+<kbd>S</kbd> (Mac) o <kbd>Ctrl</kbd>+<kbd>S</kbd> (Windows/Linux) para guardar el servidor MCP.
7. Pulsa <kbd>Esc</kbd> para salir del cuadro de diálogo MCP.

## Pedir a Copilot que explore la funcionalidad con Playwright

Ya has comprobado manualmente que la funcionalidad se comporta como esperas. Ahora usa el servidor Playwright MCP disponible en Copilot para hacer lo mismo.

1. Usa el siguiente prompt para pedir a Copilot que valide la funcionalidad con Playwright MCP:

   ```plaintext
   Start the app and use Playwright MCP to check filtering against the issue and our plan. Tell me what works and what doesn't, without making changes. Stop the server you started when you're done.
   ```

> [!NOTE]
> No hace falta indicar a Copilot que use el servidor MCP: normalmente lo deduce por sí mismo. Aun así, como sabes qué herramienta debe usar, puedes mencionarla para orientarlo. Así los resultados serán más uniformes y se ahorrarán algunos tokens.

2. Observa los pasos que Copilot realiza en el navegador para comprobar que la funcionalidad funciona.
3. Lee el informe y confirma que todo se comporta según lo previsto.

Copilot iniciará el servidor, usará Playwright para interactuar con el sitio web, detendrá el servidor y te proporcionará un informe.

## Resumen y siguientes pasos

¡Has usado el servidor Playwright MCP para explorar la funcionalidad en un navegador real desde Copilot CLI! En resumen:

- aprendiste qué es Model Context Protocol (MCP) y cómo lo usa Copilot CLI;
- añadiste el servidor Playwright MCP si todavía no estaba disponible;
- pediste al agente que controlara un navegador y explorara el filtrado.

A continuación, [crearás un agente personalizado de QA][next-lesson] que combinará la habilidad y las herramientas del navegador.

## Recursos

- [¿Qué es MCP y por qué habla todo el mundo de ello?][mcp-blog-post]
- [Servidor Playwright MCP de Microsoft][playwright-mcp-server]
- [Añadir servidores MCP a Copilot CLI][add-mcp]

[previous-lesson]: ../5-agent-skills/
[next-lesson]: ../7-qa-agent/
[mcp-blog-post]: https://github.blog/ai-and-ml/llms/what-the-heck-is-mcp-and-why-is-everyone-talking-about-it/
[playwright-mcp-server]: https://github.com/microsoft/playwright-mcp
[github-mcp]: https://github.com/github/github-mcp-server
[mcp-registry]: https://github.com/mcp
[add-mcp]: https://docs.github.com/copilot/how-tos/copilot-cli/customize-copilot/add-mcp-servers
