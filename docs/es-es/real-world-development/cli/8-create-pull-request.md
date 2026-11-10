---
title: "Lección 8 - Crear y combinar la PR de la funcionalidad"
description: "Revisa el filtrado, las instrucciones, la habilidad, el perfil de QA y las pruebas; después crea una PR y usa Agent Merge."
authors:
  - geektrainer
lastUpdated: 2026-09-18
---

La implementación del filtrado, las actualizaciones de instrucciones y de la habilidad, el perfil de control de calidad (QA) y las pruebas están guardados en una misma rama. Ahora toca revisarlos y abrir una pull request. Combinaste tú mismo la pull request (PR) de las valoraciones con estrellas; esta vez dejarás que **Agent Merge** gestione el proceso.

> [!NOTE]
> Lo normal sería separar la funcionalidad, las actualizaciones de instrucciones, la habilidad y el agente de QA en varias PR. Para simplificar el taller, has mantenido todo el trabajo de filtrado y calidad en una conversación y una rama, y lo incluirás en esta PR.

En esta lección vas a:

- aprender qué es Agent Merge y cómo automatiza el ciclo de vida de una PR;
- inspeccionar todos los cambios de la funcionalidad y las pruebas de verificación;
- crear la PR de filtrado;
- activar Agent Merge solo después de la revisión y confirmar que se ha combinado la PR.

## Escenario

Durante el flujo de trabajo de filtrado, has usado Copilot para planificar, implementar y verificar una funcionalidad. Tailspin Toys quiere automatizar el trabajo restante de la PR sin quitar al desarrollador el control sobre la autorización de la combinación.

## Introducción a Agent Merge

**Agent Merge** automatiza el trabajo restante para incorporar una pull request. Al activarlo, Copilot resuelve los obstáculos que impiden completar la PR —por ejemplo, corrige comprobaciones de integración continua (CI) que fallan, responde a comentarios de revisión y actualiza la rama cuando es necesario— y, después, activa la combinación automática de GitHub si el repositorio lo permite.

Hasta ahora has seleccionado **Merge pull request** manualmente. Agent Merge puede hacerse cargo de ese proceso. Revisa el trabajo y confirma que está listo antes de activarlo.

## Usar Agent Merge para gestionar la PR

Ya has creado todo el código. Revísalo, crea la PR y deja que Agent Merge se encargue del resto.

1. Vuelve a tu Codespace.
2. Escribe `/agent` para abrir el cuadro de diálogo de agentes.
3. Selecciona **Default** en la lista y pulsa <kbd>Enter</kbd>.
4. Crea una PR con el comando `/pr create`.
5. Activa Agent Merge con `/pr agentmerge`.
6. Copilot supervisará el proceso de integración continua de la PR. Cuando todas las comprobaciones terminen correctamente, la combinará.
7. Confirma que Copilot muestra un mensaje parecido a «PR #14 was squash-merged successfully».

> [!IMPORTANT]
> Agent Merge no omite las aprobaciones obligatorias, la protección de ramas, las colas de combinación, la configuración del repositorio ni los permisos que falten. Si el proceso queda bloqueado, lee el motivo indicado y completa manualmente la combinación revisada cuando el repositorio lo permita.

## Resumen y siguientes pasos

Has automatizado varias partes del proceso de desarrollo: la generación del código, las pruebas y la validación, y ahora también el proceso de pull request. En esta lección:

- aprendiste qué es Agent Merge y cómo automatiza el ciclo de vida de una PR;
- inspeccionaste todos los cambios de la funcionalidad y las pruebas de verificación;
- creaste la PR de filtrado;
- activaste Agent Merge después de la revisión y confirmaste que se combinó la PR.

A continuación, [explorarás otros comandos de barra útiles de Copilot CLI][next-lesson] para gestionar el contexto, los modelos, el uso compartido y la delegación opcional en la nube.

## Recursos

- [Gestionar pull requests con Copilot CLI][manage-prs]
- [Referencia de comandos de Copilot CLI][cli-reference]

[previous-lesson]: ../7-qa-agent/
[next-lesson]: ../9-cli-power-tools/
[manage-prs]: https://docs.github.com/copilot/how-tos/copilot-cli/use-copilot-cli/manage-pull-requests
[cli-reference]: https://docs.github.com/copilot/reference/copilot-cli-reference/cli-command-reference
