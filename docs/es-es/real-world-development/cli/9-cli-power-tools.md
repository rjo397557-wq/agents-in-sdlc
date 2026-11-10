---
title: "Lección 9 - Comandos de barra en GitHub Copilot CLI"
description: "Explora comandos de barra para gestionar el contexto, elegir modelos, compartir sesiones y delegar tareas opcionalmente en la nube."
authors:
  - geektrainer
lastUpdated: 2026-09-18
---

Como cualquier buena herramienta de CLI, GitHub Copilot CLI incluye muchos comandos de barra. Estos comandos permiten acceder a funciones avanzadas, información interna y opciones de configuración adicionales. Ya has usado comandos como `/diff`, `/mcp`, `/skills`, `/agent` y `/pr`. Vamos a explorar otros que pueden ser útiles.

En esta lección vas a:

- usar `/context` y `/compact` para explorar cómo gestiona Copilot el contexto de la conversación;
- usar `/model` para explorar los modelos disponibles;
- aprender a exportar o compartir una sesión con `/share`;
- explorar comandos opcionales para trabajar en paralelo, usar árboles de trabajo y delegar tareas en el agente en la nube.

## Escenario

Ya has completado el flujo de trabajo principal de la CLI. Ahora explorarás algunas funciones adicionales: gestionar el contexto, cambiar de modelo, compartir sesiones y delegar tareas opcionalmente en [Copilot cloud agent][about-cloud-agent].

## Explorar el contexto de Copilot CLI

En tareas grandes o complejas puedes llegar al límite de contexto del modelo. Copilot CLI compacta automáticamente la conversación cuando es necesario. También puedes inspeccionarla o compactarla tú mismo con los comandos de barra.

1. Vuelve a tu Codespace e inicia Copilot CLI desde la raíz del repositorio si todavía no está abierto.
2. Escribe:

   ```plaintext
   /context
   ```

3. Fíjate en el modelo, el uso actual de tokens y la distribución del contexto entre las instrucciones del sistema, las herramientas, los mensajes y el espacio disponible.
4. Compacta la conversación:

   ```plaintext
   /compact
   ```

5. Vuelve a escribir `/context` y compara el resultado. Si la conversación todavía es breve, quizá no notes una gran diferencia.

> [!NOTE]
> Copilot CLI compacta el contexto automáticamente a medida que se llena la ventana. Usa `/compact` para elegir cuándo hacerlo. Usa `/clear` o `/new` cuando cambies a una tarea que no está relacionada y quieras empezar una conversación nueva.

## Elegir un modelo

Los modelos tienen diferentes capacidades y preferencias, según el desarrollador. Copilot CLI permite mostrar y seleccionar el modelo que quieres usar.

1. Escribe:

   ```plaintext
   /model
   ```

2. Explora los modelos disponibles y la información sobre su uso.
3. Mantén el modelo actual, selecciona otro o pulsa <kbd>Esc</kbd> para cerrar la lista.

## Compartir una sesión

Colaborar y compartir lo aprendido ayuda a todo el equipo a mejorar el uso de las herramientas de IA. El comando `/share` puede exportar una sesión a un archivo Markdown o HTML, crear un enlace para compartirla o publicar un gist de GitHub.

1. Escribe `/help` y revisa las opciones de `/share` de la versión que tienes instalada.
2. Si quieres compartir la sesión, elige el destino adecuado. Por ejemplo, `/share file` crea una exportación Markdown local.
3. Revisa el contenido exportado antes de enviarlo o publicarlo. Las exportaciones pueden incluir prompts, respuestas y detalles del proyecto.

Publicar un enlace o un gist es opcional. No publiques contenido del repositorio o de la conversación que el equipo no tenga previsto compartir.

## Opcional: ampliar o delegar el trabajo

Ya has completado el taller principal. Copilot CLI también ofrece comandos para tareas más grandes:

- `/fleet` puede repartir subtareas independientes entre subagentes y ejecutarlas en paralelo.
- `/worktree` puede crear un árbol de trabajo de Git aislado para otra tarea.
- `/delegate` puede enviar una tarea a Copilot cloud agent, que trabajará de forma asíncrona y quizá abra una pull request.

Estos comandos son opcionales, ya que pueden crear árboles de trabajo adicionales o iniciar tareas remotas. Antes de probarlos, inicia una tarea nueva y bien definida, y revisa el resultado con tu proceso habitual. Para profundizar en el trabajo asíncrono con agentes, continúa con el [taller sobre cloud agent][cloud-workshop].

## Resumen y siguientes pasos

Los comandos de barra de Copilot CLI permiten configurar la herramienta, compartir sesiones y consultar lo que ocurre en segundo plano. En esta lección:

- usaste `/context` y `/compact` para explorar cómo gestiona Copilot el contexto;
- usaste `/model` para explorar los modelos disponibles;
- aprendiste a exportar o compartir una sesión con `/share`;
- exploraste comandos opcionales para trabajar en paralelo, usar árboles de trabajo y delegar tareas en el agente en la nube.

Hay más comandos de barra y funciones de Copilot CLI por explorar. Para cerrar el recorrido, [repasarás lo aprendido][next-lesson] y verás los siguientes pasos.

## Recursos

- [Referencia de comandos de Copilot CLI][cli-reference]
- [Gestión del contexto en Copilot CLI][context-management]
- [Acerca de Copilot cloud agent][about-cloud-agent]

[previous-lesson]: ../8-create-pull-request/
[next-lesson]: ../10-review/
[cli-reference]: https://docs.github.com/copilot/reference/copilot-cli-reference/cli-command-reference
[context-management]: https://docs.github.com/copilot/concepts/agents/copilot-cli/context-management
[about-cloud-agent]: https://docs.github.com/copilot/concepts/agents/cloud-agent/about-cloud-agent
[cloud-workshop]: ../../cloud/
