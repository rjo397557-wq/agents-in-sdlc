---
title: "Lección 3 - Modos de agente: Plan y Autopilot"
description: "Usa Plan para acordar un enfoque, Autopilot para crear el filtrado a partir de una incidencia e Interactive para revisar el resultado."
authors:
  - geektrainer
lastUpdated: 2026-09-18
---

Hemos empezado por añadir una pequeña funcionalidad al proyecto. Sin embargo, los cambios más amplios requieren un proceso más sólido. GitHub Copilot CLI está diseñado para trabajar con el flujo de trabajo existente de una organización y ayudarnos a crear lo que necesitamos de la forma adecuada. Esta es la primera de varias lecciones en las que seguirás un proceso de desarrollo habitual dirigido por agentes: partirás de una incidencia para generar una funcionalidad, comprobarás que el código es válido y que la funcionalidad se comporta como esperas y, por último, la integrarás en el proyecto.

> [!NOTE]
> A medida que desarrolles esta funcionalidad, seguirás usando la misma conversación y rama. Normalmente se usarían distintas ramas o PR para los diferentes tipos de archivo, pero haremos una excepción para centrarnos en los conceptos principales.

Para empezar, en esta lección vas a:

- iniciar una conversación nueva de Copilot a partir de una incidencia de GitHub;
- definir los requisitos en el modo Plan;
- implementar la funcionalidad con el modo Autopilot;
- revisar el código;
- validar manualmente la funcionalidad en un navegador reenviado.

Mientras desarrollas esta funcionalidad, actualizarás las instrucciones del repositorio, personalizarás la habilidad `quality-checks` existente, añadirás la validación MCP, crearás un agente de QA y abrirás la PR de la funcionalidad.

## Escenario

El catálogo de Tailspin Toys está creciendo y sus visitantes necesitan filtrar los juegos por categoría y editorial. La incidencia del backlog describe la funcionalidad, pero antes de programar hay que acordar algunos detalles, como la combinación de categorías. Usarás el modo Plan para resolver esas decisiones y, después, autorizarás una implementación acotada con Autopilot.

## Contexto

Incorporar agentes de programación con IA al flujo de desarrollo no cambia los fundamentos. ¡De hecho, los hace aún más importantes! La mayoría de los desarrolladores siguen un proceso parecido a este:

1. Partir de una incidencia registrada que describa el trabajo.
2. Crear un plan de lo que se debe desarrollar.
3. Implementar y revisar el código.
4. Ejecutar las pruebas para validar el código.
5. Validar manualmente la nueva funcionalidad.
6. Crear una pull request (PR).
7. Combinar el código cuando se haya revisado y se haya completado correctamente el proceso de integración continua.

> [!NOTE]
> Los detalles exactos dependen del equipo y de la organización, pero la mayoría de los procesos siguen alguna variante del patrón anterior.

Si sigues este enfoque estándar, te aseguras de que el código generado por IA cumple los requisitos establecidos y pasa por el mismo proceso de evaluación que el código escrito manualmente.

## Modos de conversación

El **modo de conversación** controla el grado de autonomía del agente. Pulsa <kbd>Shift</kbd>+<kbd>Tab</kbd> para alternar entre los modos:

- **Interactive**: trabajas con el agente. Este sugiere cambios y espera tus indicaciones antes de continuar.
- **Plan**: el agente crea primero un plan y no puede editar los archivos del proyecto.
- **Autopilot**: el agente trabaja de forma autónoma: escribe código, ejecuta pruebas e itera hasta considerar que ha terminado la tarea.

Empieza en el modo Plan, revisa el plan y, después, usa Autopilot para implementarlo.

## Partir de la incidencia

Antes de empezar a trabajar en el filtrado, vuelve a tu Codespace y comprueba que el repositorio y el terminal están listos.

1. Vuelve a tu Codespace. Si está detenido, reinícialo antes de continuar.
2. Confirma que se ha combinado la PR de las valoraciones con estrellas.
3. Si el terminal no está abierto, pulsa <kbd>Control</kbd>+<kbd>\`</kbd> (Mac) o <kbd>Ctrl</kbd>+<kbd>\`</kbd> (Windows/Linux).
4. Actualiza `main` y crea una rama para el trabajo de filtrado:

   ```bash
   git checkout main
   git pull --ff-only
   git checkout -b game-filters-cli
   ```

5. Inicia Copilot CLI:

   ```bash
   copilot --yolo
   ```

6. Pulsa <kbd>Tab</kbd> dos veces para abrir la pestaña **Issues**.
7. Pulsa <kbd>A</kbd> para mostrar todas las incidencias.
8. Usa las teclas de flecha para resaltar la incidencia titulada **Allow users to filter games by category and publisher**.
9. Pulsa <kbd>C</kbd> para añadir la incidencia al prompt y volver a la pestaña **Session**.

Fíjate en que el prompt ahora empieza por `#7` (o un número parecido). El carácter `#` permite añadir al contexto una incidencia o una pull request (PR) de GitHub.

## Planificar la funcionalidad de filtrado

La planificación permite definir cómo se implementará una funcionalidad o se realizará una tarea antes de delegarla en Copilot. Siempre es buena idea dedicar tiempo a planificar las tareas complejas. Cambia al modo Plan y pide a Copilot que elabore el plan.

1. Pulsa <kbd>Shift</kbd>+<kbd>Tab</kbd> para cambiar al modo Plan. Confirma que el indicador debajo del prompt muestra **Plan**.
2. Después de la referencia a la incidencia que añadiste en el paso anterior, escribe el siguiente prompt:

   ```plaintext
   Create a plan for implementing this feature.
   ```

Copilot empezará a elaborar el plan. Primero explorará el proyecto y, después, determinará el mejor enfoque.

3. Durante el proceso, Copilot puede preguntarte cómo quieres que funcionen las opciones de filtrado. Responde según tus preferencias; no hay respuestas incorrectas.
4. Cuando termine el plan, pulsa <kbd>Control</kbd>+<kbd>E</kbd> (Mac) o <kbd>Ctrl</kbd>+<kbd>E</kbd> (Windows/Linux) para ampliarlo.
5. Desplázate por el plan para revisarlo.
6. Pide a Copilot que modifique cualquier parte del plan que no coincida con tus decisiones.

## Aprobar Autopilot

Ya has redactado y revisado el plan; ahora toca implementarlo. Deja que Copilot trabaje con el modo Autopilot.

Autopilot permite que Copilot itere sobre el problema hasta que considere que lo ha resuelto.

1. Selecciona **Accept plan and build on autopilot (recommended)** o la opción equivalente en la versión que tengas instalada.
2. Confirma que el indicador debajo del prompt muestra **Autopilot**.
3. Observa cómo Copilot sigue el plan acordado, genera el código y ejecuta las pruebas.

> [!NOTE]
> Al aprobar el plan puede comenzar la implementación de inmediato, así que revísalo antes. Si Copilot informa de dependencias ausentes o de un conflicto de puertos, resuelve el problema de configuración antes de dar por completadas las comprobaciones.

## Revisar y verificar la implementación

Una vez generado el código, debes revisarlo antes de combinarlo, como harías con cualquier otro cambio. Revisaremos el código y ejecutaremos el sitio para comprobar que todo funciona.

1. Pulsa <kbd>Shift</kbd>+<kbd>Tab</kbd> para cambiar al modo Interactive. Confirma que el indicador de modo ya no muestra **Plan** ni **Autopilot**.
2. Escribe `/diff` y revisa la implementación del filtrado y las pruebas.
3. Compara el resultado con la incidencia y las decisiones que tomaste durante la planificación.
4. Cuando termines la revisión del código, pulsa <kbd>Esc</kbd> para salir de la vista de diferencias.
5. Revisa los resultados de las comprobaciones del proyecto y pide a Copilot que corrija cualquier error.

## Explorar la nueva funcionalidad

El código parece correcto, pero ¿se ejecuta? Inicia la aplicación como antes y abre el sitio a través del puerto reenviado de Codespaces.

1. Pide a Copilot que inicie la aplicación:

   ```plaintext
   Start the app so I can try the filtering feature in my browser. Tell me the URL and leave the server running.
   ```

2. Cuando Codespaces indique que el puerto `4321` está disponible, selecciona **Open in Browser**.
3. Prueba los filtros por categoría y editorial, así como las combinaciones que acordaste en el plan.
4. Confirma que el comportamiento de restablecimiento y de resultados vacíos coincide con la incidencia y tus decisiones.
5. Vuelve a tu Codespace y pide a Copilot que detenga el servidor de desarrollo que inició.

## Resumen y siguientes pasos

Has usado distintos modos de conversación para crear y revisar una funcionalidad. En esta lección:

- iniciaste una conversación nueva de Copilot a partir de una incidencia de GitHub;
- definiste los requisitos en el modo Plan;
- implementaste la funcionalidad con Autopilot;
- revisaste el código;
- validaste manualmente la funcionalidad en un navegador reenviado.

A continuación, veremos con más detalle cómo generar código para que siga las prácticas documentadas mediante las [instrucciones personalizadas][next-lesson].

## Recursos

- [Autopilot en GitHub Copilot CLI][autopilot]
- [Referencia de comandos de Copilot CLI][cli-reference]

[autopilot]: https://docs.github.com/copilot/concepts/agents/copilot-cli/autopilot
[cli-reference]: https://docs.github.com/copilot/reference/copilot-cli-reference/cli-command-reference
[previous-lesson]: ../2-add-star-rating/
[next-lesson]: ../4-custom-instructions/
