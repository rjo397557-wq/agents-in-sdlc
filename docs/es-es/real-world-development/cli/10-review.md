---
title: "Lección 10 - Repaso y próximos pasos"
description: "Repasa el flujo de trabajo de Copilot CLI, las dos pull requests, las personalizaciones reutilizables y otros recursos."
authors:
  - geektrainer
lastUpdated: 2026-09-29
---

Has usado GitHub Copilot CLI en un flujo de trabajo continuo de Tailspin Toys. En este taller:

- preparaste un Codespace, instalaste Copilot CLI, exploraste el proyecto y localizaste la incidencia de filtrado;
- añadiste valoraciones con estrellas, revisaste el resultado en un navegador reenviado y combinaste manualmente tu primera pull request (PR);
- partiste de la incidencia de filtrado, definiste el enfoque con el modo Plan, lo implementaste con Autopilot y lo revisaste en Interactive;
- orientaste al agente con instrucciones personalizadas, personalizaste la habilidad `quality-checks` existente y la usaste para ejecutar pruebas unitarias, lint y comprobaciones de tipos;
- añadiste el servidor Model Context Protocol (MCP) de Playwright y lo usaste para explorar el filtrado en un navegador real;
- creaste y seleccionaste un agente personalizado de control de calidad (QA) para evaluar los requisitos, la cobertura, los resultados de la habilidad y las pruebas del navegador;
- revisaste todos los cambios del filtrado y autorizaste Agent Merge para la PR correspondiente;
- exploraste comandos de barra para gestionar el contexto, los modelos, el uso compartido y la delegación opcional en la nube.

## Lo que has publicado

El taller incluye dos hitos de PR, cada uno en su propia rama creada desde `main` actualizado:

1. **Valoraciones con estrellas**: muestra en las tarjetas de juegos el `starRating` existente y un estado explícito cuando todavía no hay valoración.
2. **Filtrado y flujo de calidad**: implementa el filtrado, actualiza las instrucciones y aplícalas a la funcionalidad, personaliza el informe de `quality-checks`, crea un perfil de QA e incluye las pruebas correspondientes.

Desde la planificación del filtrado hasta la creación de su PR, usaste la misma conversación y rama. Combinamos todo ese trabajo en una sola PR para simplificar el taller.

## Diferentes tipos de verificación

Comprobaste el código de varias formas: con pruebas automatizadas, inspeccionando el sitio en el navegador y usando Copilot para explorarlo mediante MCP. La habilidad `quality-checks` ejecutó las pruebas unitarias, lint y las comprobaciones de tipos, y presentó los resultados con el nuevo formato. QA reunió esos resultados con una revisión de los requisitos y la cobertura antes de crear la PR.

Las pruebas nuevas deben cubrir carencias reales; si una ejecución de QA no requiere pruebas adicionales, puede ser correcto. Revisa el código y las pruebas de verificación antes de autorizar la combinación, y vuelve a recopilar las pruebas afectadas después de hacer cambios.

## Buenas prácticas

El contexto y las herramientas que proporcionas influyen en el trabajo de Copilot. En este taller actualizaste instrucciones, personalizaste una habilidad, creaste un perfil de QA y configuraste un servidor MCP. Reutiliza estas personalizaciones entre conversaciones y ajústalas a las necesidades del equipo. Las instrucciones establecen estándares, las habilidades describen tareas repetibles, los agentes personalizados definen roles especializados y los servidores MCP conectan herramientas externas. Revisa los cambios y los resultados reales de las herramientas, no solo el resumen del agente.

Adapta el **modo y el modelo** a la tarea. Usa **Plan** para analizar el enfoque antes de implementarlo, **Interactive** para seguir de cerca los cambios específicos y **Autopilot** para tareas bien acotadas. Elige un modelo más rápido para ediciones rutinarias y uno más capaz para tareas complejas.

El contexto sigue siendo tan importante como la infraestructura. Explicar claramente *qué* quieres crear, *por qué* y *cómo* influye de forma importante en el resultado.

## Más opciones para explorar

Ya has cubierto el flujo de trabajo principal. Estas son algunas funciones más de la CLI que puedes probar:

- `/review` para pedir al agente de revisión de código que analice los cambios;
- `/rubber-duck` para explicar un problema y obtener otra perspectiva;
- `/fleet` para coordinar subtareas independientes en paralelo;
- `/worktree` para aislar otra tarea;
- `/delegate` para enviar una tarea a Copilot cloud agent.

## Próximos pasos

La mejor forma de mejorar con cualquier herramienta es seguir usándola. Úsala con código de producción, en proyectos personales o para crear esa pequeña aplicación que llevas años imaginando. Comparte lo que aprendas con el equipo y aprende de sus experiencias. Y, como siempre, explora la documentación.

Si quieres ampliar Tailspin Toys desde el terminal, continúa con la [serie opcional Backer Concierge de Foundry][foundry]. Para comparar otros entornos, explora el [taller de VS Code][vscode], el [taller de la aplicación GitHub Copilot][app] o el [taller de Copilot cloud agent][cloud].

## Recursos

- [Acerca de GitHub Copilot CLI][about-cli]
- [Referencia de comandos de Copilot CLI][cli-reference]
- [Personalizar Copilot CLI][customize-cli]
- [Gestionar pull requests con Copilot CLI][manage-prs]

[previous-lesson]: ../9-cli-power-tools/
[foundry]: ../8-foundry-agent/
[vscode]: ../../vscode/
[app]: ../../app/
[cloud]: ../../cloud/
[about-cli]: https://docs.github.com/copilot/concepts/agents/about-copilot-cli
[cli-reference]: https://docs.github.com/copilot/reference/copilot-cli-reference/cli-command-reference
[customize-cli]: https://docs.github.com/copilot/how-tos/copilot-cli/customize-copilot
[manage-prs]: https://docs.github.com/copilot/how-tos/copilot-cli/use-copilot-cli/manage-pull-requests
