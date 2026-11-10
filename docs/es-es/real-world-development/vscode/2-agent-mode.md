---
title: "Ejercicio 2 - Añadir funcionalidades con el modo agente de Copilot"
authors:
  - geektrainer
lastUpdated: 2026-06-30
---

Incluso las actualizaciones más sencillas de una aplicación suelen requerir cambios en varios archivos y tareas como ejecutar pruebas. El flujo de trabajo de un desarrollador suele consistir en localizar todos los archivos necesarios, hacer los cambios, ejecutar las pruebas, depurar, averiguar qué archivo falta, hacer otra actualización y repetir el proceso.

El modo agente de Copilot ayuda con estas tareas.

El modo agente de Copilot está diseñado para trabajar de forma más autónoma en el IDE. Se comporta de manera parecida a un desarrollador: primero explora la estructura del proyecto, realiza las actualizaciones necesarias, ejecuta tareas como las pruebas y corrige automáticamente los errores que encuentra. Veamos cómo usar el modo agente para añadir funcionalidades al sitio.

> [!NOTE]
> Aunque los nombres se parecen, el modo agente y el agente en la nube ofrecen experiencias distintas. El modo agente trabaja en el IDE, con ciclos rápidos de comentarios e interacción. El agente en la nube funciona como un compañero de programación asíncrono que interactúa contigo mediante incidencias y pull requests.

En este ejercicio aprenderás cómo:

- el modo agente de Copilot explora el proyecto, identifica los archivos relevantes y coordina cambios;
- el modo agente de GitHub Copilot implementa funcionalidades en la interfaz y la capa de datos;
- revisar los cambios y las pruebas generados por el modo agente antes de combinarlos en el código.

## Escenario

A medida que aumenta la lista de juegos, quieres permitir que los usuarios filtren por categoría y editorial. En el ejercicio anterior ya creaste un helper de editoriales; ahora completarás con el modo agente de Copilot el resto del trabajo de datos, interfaz y pruebas.

## Ejecutar el sitio web de Tailspin Toys

Antes de hacer cambios, exploremos el sitio web de Tailspin Toys para conocer sus funciones actuales.

El sitio es una plataforma de financiación colectiva de juegos de mesa con temática de desarrollo. Permite mostrar listas de juegos y sus detalles. Es una única aplicación Astro que genera las páginas como HTML estático durante la compilación. Las páginas consultan directamente una base de datos SQLite local mediante Drizzle ORM; no hay una API de backend independiente ni un framework de interfaz en el cliente. Los helpers reutilizables de acceso a datos se encuentran en `src/lib/`. La interactividad se añade mediante un pequeño `<script>` de Astro con ámbito limitado y las API estándar del DOM.

### Iniciar el sitio web

Para facilitar el inicio del sitio, el comando `npm run dev` arranca el único servidor de desarrollo de Astro. Puedes ejecutarlo en tu Codespace de GitHub siguiendo estos pasos:

1. Vuelve a tu Codespace. Después del ejercicio anterior deberías estar en `main`.
2. Abre un terminal nuevo en el Codespace con <kbd>Control</kbd>+<kbd>\`</kbd> (Mac) o <kbd>Ctrl</kbd>+<kbd>\`</kbd> (Windows/Linux).
3. Crea y selecciona una rama nueva para el trabajo de filtrado:

   ```bash
   git checkout main
   git pull
   git checkout -b filtering-vscode
   ```

4. Inicia el sitio web:

   ```bash
   npm run dev
   ```

   Cuando el servidor de desarrollo de Astro esté listo, verás un mensaje con la URL, parecido a este:

   ```bash
   🚀 Tailspin Toys is ready!
      Astro server: http://localhost:4321
      Press Ctrl-C to stop.
   ```

> [!NOTE]
> Si aparece un cuadro de diálogo que te pregunta si quieres abrir `http://localhost:4321` en un navegador, ciérralo seleccionando **x**.

5. Abre el sitio manteniendo pulsada la tecla <kbd>Command</kbd> (Mac) o <kbd>Ctrl</kbd> (Windows/Linux) y seleccionando en el terminal la dirección del servidor Astro: `http://localhost:4321`.

> [!NOTE]
> En un Codespace, al seleccionar el enlace localhost en el terminal, se te redirigirá automáticamente a `https://<your-codespace-name>-4321.app.github.dev/`. Es un túnel privado a tu Codespace, donde ahora se ejecuta el servidor web.

### Explorar el sitio web

Cuando el sitio esté en ejecución, puedes explorar sus funciones principales:

- **Página de inicio**: muestra juegos de mesa con sus títulos, imágenes y descripciones.
- **Página de detalles del juego**: al seleccionar un juego, se abre una página con más información, como el título, la descripción, la editorial y la categoría.

## Explorar el backlog con Copilot

> [!TIP]
> **Abrir Copilot Chat**
>
> Antes de empezar los ejercicios siguientes, vuelve a tu Codespace, abre el panel Copilot Chat y selecciona **New Chat** para iniciar una conversación nueva. El modo y la selección del modelo varían según el ejercicio; cada paso lo indicará cuando sea necesario.

La implementación inicial del sitio funciona, pero queremos añadir capacidades. Empecemos por revisar el backlog. Al crear el proyecto a partir de la plantilla se generó automáticamente un backlog de incidencias. Pide a GitHub Copilot que te muestre esas tareas.

1. Selecciona **Agent** en el menú desplegable de agentes de Chat. El agente planifica e implementa cambios de forma autónoma en varios archivos, ejecuta comandos de terminal y usa herramientas.

   ![Captura del selector de agentes en la vista Chat](../../../_images/shared-chat-mode-selector.png)

2. Selecciona **Claude Sonnet 4.5** en la lista de modelos disponibles.

> [!CAUTION]
> Los autores del taller no recomiendan un modelo concreto. Durante su desarrollo usamos Claude Sonnet 4.5 y lo incluimos en estas instrucciones para intentar que las sugerencias sean relativamente uniformes. Sin embargo, como los modelos de lenguaje grandes (LLM) son probabilísticos, es posible que las sugerencias que recibas sean distintas. Es normal y esperable.

> [!NOTE]
> Debido a la naturaleza probabilística de los LLM, Copilot puede utilizar otro comando MCP, pero debería poder completar la tarea.

3. Pide a Copilot que muestre el backlog enviando este prompt:

   ```plaintext
   Please show me the backlog of items from my GitHub repository. Help me prioritize them based on those which will be most useful to the user.
   ```

4. Selecciona **Continue** para ejecutar el comando que enumera todas las incidencias.
5. Revisa la lista generada.

Fíjate en que Copilot ha priorizado las tareas según las que considera más útiles para los usuarios.

## Revisar los archivos de instrucciones

Antes de pedir al agente que genere el código, revisa el archivo de instrucciones que dará contexto a Copilot. Usarás el archivo de instrucciones de [interfaz de usuario (UI)](https://github.com/github-samples/tailspin-toys/blob/main/.github/instructions/ui.instructions.md), que explica cómo añadir funcionalidades al sitio.

1. En el Codespace, abre `.github/instructions/ui.instructions.md`.
2. Fíjate en las directrices generales para añadir funciones, entre ellas:
   - una descripción general de la arquitectura;
   - principios de diseño de componentes, capacidad de prueba y accesibilidad;
   - enlaces a archivos de instrucciones específicos para Astro, Tailwind CSS y Drizzle.

> [!TIP]
> Los archivos de instrucciones pueden incluir referencias a otros archivos de instrucciones y a archivos del proyecto. Las rutas son relativas al archivo que contiene el enlace. Así se pueden reutilizar directrices, dividir conjuntos complejos en partes más manejables y proporcionar a Copilot ejemplos y plantillas.

## Implementar el filtrado

Para completar el filtrado, la aplicación necesita al menos tres cambios:

- añadir o ajustar la lógica restante de filtrado en la capa de datos (`src/lib/`);
- añadir o actualizar las pruebas del comportamiento de filtrado;
- actualizar la página de lista de juegos para incluir la interfaz de filtrado.

Además, las pruebas deben superarse antes de combinar los cambios en el código. El modo agente de Copilot puede realizar estas tareas. Añadamos la funcionalidad.

1. Puedes continuar la conversación actual con Copilot o iniciar otra con **New Chat**.
2. Selecciona **Add Context**, **Instructions** y **ui** para añadir el archivo de instrucciones.

   ![Ejemplo de selección del archivo de instrucciones de interfaz](../../../_images/ex3-select-instructions-file.png)

3. Confirma que **Agent** sigue seleccionado en el menú desplegable de agentes de Chat.

   ![Captura del selector de agentes en la vista Chat](../../../_images/shared-chat-mode-selector.png)

4. Confirma que **Claude Sonnet 4.5** sigue seleccionado como modelo.
5. Pide a Copilot que implemente la funcionalidad a partir de la incidencia relacionada del backlog:

   ```plaintext
   Please update the site to include filtering by publisher and category based on the requirements from the related GitHub issue in the backlog. A publishers helper already exists from the previous exercise, so preserve and refine that work as needed while completing the remaining data-layer, UI, and tests. Ensure all tests are passing before completion. The server is already running, so you do not need to start it up.
   ```

6. Observa cómo Copilot explora el proyecto y localiza los archivos relacionados. Deberías ver que encuentra los helpers de datos, la interfaz y las pruebas. Después empieza a modificar archivos y ejecutar las pruebas.

   ![Copilot explora los archivos del proyecto](../../../_images/ex3-agent-mode-explores.png)

> [!NOTE]
> Copilot realizará varias tareas: explorar el proyecto, modificar archivos y ejecutar pruebas. El proceso puede tardar unos minutos según la complejidad de la tarea y del código. Mientras trabaja, puede que aparezcan los botones **Keep** y **Undo** en el editor. Al terminar, habrá un botón **Keep** o **Undo** para cada cambio; no hace falta seleccionarlos mientras el agente sigue trabajando.

7. Cuando Copilot lo solicite, selecciona **Continue** para ejecutar las pruebas.

   ![Cuadro de Copilot Chat que pide permiso para ejecutar pruebas](../../../_images/ex3-agent-mode-run-tests.png)

8. Puede que Copilot se detenga y que algunas pruebas fallen durante el proceso. No pasa nada: seguirá alternando entre generar código y ejecutar pruebas hasta completar la tarea sin detectar errores.

   ![Sesión completa de Copilot en modo agente](../../../_images/ex3-agent-mode-proposed-changes.png)

9. Revisa el código generado para detectar posibles problemas.

> [!CAUTION]
> Recuerda que siempre es importante revisar el código generado por Copilot o cualquier otra herramienta de IA.

10. Vuelve al navegador con el sitio en ejecución y explora la nueva funcionalidad.
11. Cuando hayas confirmado que todo funciona y revisado el código, selecciona **Keep** en la ventana de Copilot Chat.

## Resumen y siguientes pasos

¡Enhorabuena! En este ejercicio has aprendido a usar el modo agente de GitHub Copilot para añadir capacidades a Tailspin Toys. Has visto cómo:

- el modo agente de GitHub Copilot implementa funcionalidades en la interfaz y la capa de datos;
- el modo agente explora el proyecto, identifica archivos relevantes y coordina cambios;
- revisar los cambios y las pruebas generados por el modo agente antes de combinarlos en el código.

Ahora [probarás la funcionalidad con el servidor Playwright MCP][next-lesson] y abrirás una pull request.

### Ejercicio de exploración adicional: implementar paginación

A medida que crezca la lista de juegos, será necesario habilitar la paginación. Usa lo aprendido en este ejercicio para pedir a Copilot que añada la paginación. Ten en cuenta lo siguiente:

- sigue las buenas prácticas existentes, incluidos los archivos de instrucciones;
- decide cómo se implementará la paginación y si el usuario podrá elegir el tamaño de página o será fijo;
- proporciona a Copilot las directrices necesarias para implementar la solución que quieres;
- puede que tengas que iterar con Copilot, pedir cambios y aportar contexto. Es lo normal al trabajar con Copilot.

## Recursos

- [Copilot: modos ask, edit y agent; qué hacen y cuándo usarlos][choose-mode]
- [Modo agente en VS Code][vs-code-agent-mode]

[previous-lesson]: ../1-custom-instructions/
[next-lesson]: ../3-mcp/
[choose-mode]: https://github.blog/ai-and-ml/github-copilot/copilot-ask-edit-and-agent-modes-what-they-do-and-when-to-use-them/
[vs-code-agent-mode]: https://code.visualstudio.com/docs/copilot/chat/chat-agent-mode
