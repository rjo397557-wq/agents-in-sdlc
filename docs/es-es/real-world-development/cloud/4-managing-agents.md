---
title: "Ejercicio 4 - Supervisar y gestionar agentes"
authors:
  - geektrainer
lastUpdated: 2026-06-30
---

| [← Lección anterior: Agentes personalizados][previous-lesson] |
|:--|

En los últimos ejercicios has pedido al agente de Copilot en la nube que realice tres tareas distintas para mejorar la experiencia de usuario y añadir funcionalidades. Aunque el agente en la nube trabaja de forma autónoma y asíncrona, es importante poder supervisar esas tareas.

Hay varias herramientas para gestionar las tareas asignadas al agente en la nube, incluida la [página de agentes][agents-page] de GitHub.com. Desde este centro de control puedes ver todas las tareas de los agentes que tienen pull requests (PR) abiertas. También puedes explorar las operaciones realizadas y orientar una sesión en curso.

En esta lección vas a:

- explorar la página de agentes para supervisar las tareas del agente en la nube;
- orientar una sesión en curso para pedir una funcionalidad adicional.

## Escenario

Después de asignar al agente la creación de un modo de alto contraste, el equipo decidió que también sería un buen momento para añadir un modo claro. Como ya se estaba actualizando el estilo del sitio y añadiendo controles de alternancia, parecía lógico incluir esta funcionalidad. Orientarás el trabajo del agente para que añada el modo claro además del modo de alto contraste.

## Revisar las tareas del agente de Copilot en la nube

Veamos el estado actual de las tareas asignadas al agente de Copilot en la nube.

1. Ve a la [página de agentes de Copilot](https://github.com/copilot/agents).
2. Observa las tareas del panel principal y del panel izquierdo. Deberías ver las tareas que has asignado a Copilot, entre ellas:
    - actualizar la documentación del código;
    - generar API para modificar juegos;
    - añadir un modo de alto contraste al sitio web.
3. Selecciona una de las tareas en curso y revisa las operaciones realizadas por Copilot. Pueden incluir:
    - obtener el código del repositorio;
    - crear el entorno de trabajo de Copilot;
    - configurar servidores MCP;
    - realizar varios pasos para completar la tarea asignada.

> [!NOTE]
> Los pasos exactos que aparecen dependen del estado del trabajo de Copilot y del enfoque que haya seguido.

4. Fíjate también en el panel de pull requests (PR) de la derecha. Desde ahí puedes consultar la PR y los archivos modificados para seguir el trabajo.

## Orientar al agente en la nube

Ya has visto las tareas activas. Ahora pide a Copilot que añada un control de alternancia para el modo claro mientras trabaja en el modo de alto contraste.

1. Selecciona la sesión relacionada con el modo de alto contraste. El título exacto dependerá del nombre que le haya dado Copilot y del estado del trabajo.

    ![Sesión de accesibilidad en Mission Control](../../../_images/ex6-accessibility-session.png)

2. Observa la sesión durante unos minutos hasta que indique que ha terminado la configuración y ha empezado a trabajar. Lo notarás cuando aparezcan mensajes parecidos a los siguientes.
3. En el cuadro de diálogo **Steer active session while Copilot is working**, escribe este prompt:

    ```plaintext
    While we are working on a high contrast mode, let's also add a light mode. There should be a switch for this mode as well where users can select their desired display mode.
    ```

    ![Captura de la tarea del agente en la nube en la página de agentes, con el cuadro de diálogo para orientar la sesión activa de Copilot resaltado](../../../_images/ex6-steer-cloud-agent-task.png)

4. Pulsa <kbd>Enter</kbd> para enviar el prompt.
5. Observa cómo Copilot confirma que lo ha recibido y lo incorpora a su flujo de trabajo.

## Dejar que Copilot trabaje

Como antes, Copilot empezará a trabajar en la tarea actualizada. Incorporará la nueva solicitud al flujo de trabajo cuando termine el paso que estaba ejecutando al recibir el mensaje.

Esto también tardará varios minutos. Es un buen momento para hacer una pausa y repasar lo que has aprendido y explorado hasta ahora.

## Resumen y siguientes pasos

En esta lección has explorado la página de agentes de Copilot, el centro de control para supervisar y orientar las tareas del agente de GitHub Copilot en la nube. Puedes seguir las tareas activas y completadas, revisar el trabajo y redirigir sesiones en curso para modificar su alcance o añadir indicaciones.

Has explorado estos conceptos:

- usar Mission Control y la página de agentes para supervisar las tareas del agente en la nube;
- redirigir una sesión en curso para pedir una funcionalidad adicional.

Mientras Copilot completa las funciones de accesibilidad, puedes [iterar sobre las pull requests que ha creado][next-lesson].

## Recursos

- [Página de agentes de Copilot][agents-page]
- [Acerca de los agentes personalizados][custom-agents]

---

| [← Lección anterior: Agentes personalizados][previous-lesson] | [Siguiente lección: Iterar sobre el trabajo de Copilot →][next-lesson] |
|:--|--:|

[previous-lesson]: ../3-custom-agents/
[next-lesson]: ../5-iterating/
[agents-page]: https://github.blog/changelog/2025-07-03-agents-page-for-copilot-coding-agent-in-public-preview
[custom-agents]: https://docs.github.com/copilot/concepts/agents/cloud-agent/about-custom-agents
