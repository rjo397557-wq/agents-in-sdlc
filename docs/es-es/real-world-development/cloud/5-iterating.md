---
title: "Ejercicio 5: Iterar sobre el trabajo de GitHub Copilot"
authors:
  - geektrainer
lastUpdated: 2026-06-30
---

| [← Lección anterior: Gestionar agentes][previous-lesson] |
|:--|

## Revisar el trabajo

A lo largo de este taller has trabajado con GitHub Copilot en varias tareas para mejorar la experiencia de usuario y añadir funcionalidades. Pediste a Copilot que documentara el código, creara una funcionalidad de juegos relacionados para que el equipo de diseño pudiera probarla e implementara mejoras de accesibilidad, incluidos controles para activar el alto contraste y el modo claro. Vamos a revisar los cambios de código y, si hace falta, pedir a Copilot que los mejore.

### Escenario

Como hemos destacado varias veces, los fundamentos del diseño de software y DevOps no cambian al incorporar la IA generativa. Siempre hay que revisar el código generado y seguir el proceso habitual de DevOps. Con eso en mente, revisemos las sugerencias de GitHub Copilot para documentar el código, añadir juegos relacionados y mejorar la accesibilidad antes de activar la revisión para el resto del equipo.

## Seguridad y agente de GitHub Copilot en la nube

Como el agente de Copilot en la nube trabaja de forma asíncrona y sin supervisión, se han establecido restricciones de seguridad para proteger el entorno:

- Copilot solo tiene acceso de lectura al repositorio y acceso de escritura **únicamente** a la rama que usará para su trabajo.
- El agente en la nube se ejecuta en GitHub Actions y crea un entorno independiente y efímero para trabajar.
- Una persona debe aprobar todos los flujos de trabajo de GitHub Actions antes de que se ejecuten.
- [El acceso a recursos externos está limitado de forma predeterminada](https://docs.github.com/copilot/how-tos/copilot-on-github/customize-copilot/customize-cloud-agent/customize-the-agent-firewall), incluidos los servidores MCP.

## Revisar la documentación generada

Empecemos por la primera pull request (PR) que generó el agente de GitHub Copilot en la nube: añadir documentación al código. Revisaremos el trabajo con la interfaz habitual de PR de GitHub.com.

> [!NOTE]
> Al abrir la PR puede aparecer una advertencia indicando que un firewall ha bloqueado a GitHub Copilot. Es **normal**: Copilot tiene acceso limitado a recursos externos, incluidas las llamadas a servidores MCP externos. Si quieres, puedes [personalizar o deshabilitar el firewall del agente de Copilot en la nube][agent-firewall].

1. Vuelve a tu repositorio en github.com.
2. Selecciona **Pull Requests** para abrir la lista de pull requests.
3. Abre la pull request titulada algo parecido a **Add missing documentation** u otro título más descriptivo.

> [!NOTE]
> Si Copilot sigue trabajando, la pull request tendrá la marca **[WIP]**. Espera a que termine; puede tardar unos minutos. Mientras tanto, puedes descansar o repasar lo que has aprendido.

4. Cuando la pull request esté lista, selecciona la pestaña **Files changed** y revisa los cambios.

    ![Pestaña Files changed](../../../_images/shared-pr-files-changed.png)

5. Explora el código actualizado, que ahora incluye comentarios de documentación TSDoc y otra documentación. Los cambios concretos pueden variar.

    Al revisar los cambios, busca comentarios de documentación TSDoc, convenciones de TypeScript y encabezados de comentario. Proceden de los archivos de instrucciones personalizadas que revisaste al principio del Ejercicio 2.

6. Cuando hayas revisado las actualizaciones y te parezcan correctas, vuelve a la pestaña **Conversation** y desplázate hacia abajo.
7. Deberías ver un indicador que señala que algunos flujos de trabajo esperan aprobación.
8. Si hay flujos de trabajo pendientes, selecciona **Approve and run workflows**.

   ![Aprobar y ejecutar flujos de trabajo](../../../_images/shared-approve-workflows.png)

9. Los flujos de trabajo deberían aparecer en la sección de comprobaciones de la pull request. Si todo va bien, las comprobaciones del proyecto para la aplicación Astro terminarán correctamente. Puede tardar unos minutos.

## Pedir cambios a GitHub Copilot

Trabajar con Copilot en una pull request no es una comunicación unidireccional. Puedes mencionarlo en comentarios de la PR —igual que a otros integrantes del equipo— o en comentarios específicos del código. Copilot verá esos comentarios e iniciará otra sesión para resolverlos. Como los resultados no son deterministas, no podemos darte una frase exacta para pedir cambios. Algunas ideas:

- Añadir al principio de cada archivo de código un bloque de comentarios que describa su función.
- Añadir comentarios de documentación TSDoc a los archivos TypeScript y Astro.
- Crear un README que describa la estructura de la aplicación Astro.

1. Añade un comentario para pedir un cambio en la documentación generada y menciona a **@copilot**, como harías con cualquier otra persona. Usa una de las ideas anteriores u otra petición sobre la documentación del código.
2. Selecciona **View Session** para observar cómo trabaja Copilot. Verás que inicia una nueva sesión para realizar las actualizaciones.
3. Puedes seleccionar **Back to pull request** para volver a la pull request.

    ![Volver a la pull request](../../../_images/ex7-back-to-pr.png)

4. Cuando Copilot termine los cambios, verás una nueva confirmación en la pull request.
5. Selecciona la pestaña **Files changed** para revisar los cambios.

Puedes seguir iterando hasta que estés conforme. Después, puedes marcar la PR como lista para revisión y combinarla en la rama principal.

![Marcar la PR como lista para revisión](../../../_images/ex7-ready-for-review.png)

## Revisar la funcionalidad de juegos relacionados

Volvamos a la PR que Copilot generó para resolver la incidencia sobre los juegos relacionados en la página de detalles.

1. Vuelve a tu repositorio en GitHub.com.
2. Selecciona la pestaña **Pull Requests**.
3. Selecciona la PR con un título parecido a **Show related games on the game details page** u otro título más descriptivo.
4. Selecciona la pestaña **Files changed** para revisar el código generado.
5. Cuando hayas revisado las actualizaciones y te parezcan correctas, vuelve a la pestaña **Conversation** y desplázate hacia abajo.
6. Deberías ver un indicador que señala que algunos flujos de trabajo esperan aprobación.
7. Si hay flujos de trabajo pendientes, selecciona **Approve and run workflows**.

   ![Aprobar y ejecutar flujos de trabajo](../../../_images/shared-approve-workflows.png)

8. Los flujos de trabajo deberían aparecer en la sección de comprobaciones de la pull request. Si todo va bien, las comprobaciones del proyecto para la aplicación Astro terminarán correctamente. Puede tardar unos minutos.
9. **Opcional:** puedes cambiar a esta rama en tu Codespace y probar manualmente la funcionalidad de juegos relacionados. Ve al Codespace, abre el terminal y ejecuta estos comandos (sustituye `<branch-name>` por el nombre de la rama creada por Copilot, por ejemplo, **copilot/fix-8**):

    ```bash
    git fetch origin
    git checkout <branch-name>
    ```

Copilot ha creado la funcionalidad de juegos relacionados. Como antes, puedes trabajar de forma iterativa con el agente de Copilot en la nube y pedirle que haga cambios. Por ejemplo, puedes modificar el número de juegos relacionados que se muestran o pedir que añada encabezados de comentario y comentarios de documentación TSDoc (recuerda que la incidencia se asignó **antes** de actualizar las instrucciones personalizadas). Para ello, añade un comentario nuevo en la pestaña **Conversation**; Copilot lo verá e iniciará otra sesión.

## Revisar las funciones de accesibilidad

Por último, revisemos las funciones de accesibilidad implementadas con el agente personalizado de accesibilidad. Esta PR debería incluir tanto el modo de alto contraste que asignaste en el Ejercicio 3 como el modo claro que pediste en Mission Control en el Ejercicio 4.

1. Vuelve a tu repositorio en GitHub.com.
2. Selecciona la pestaña **Pull Requests**.
3. Selecciona la PR con un título parecido a **Add high contrast mode to website** u otro título más descriptivo.

> [!NOTE]
> Si Copilot sigue trabajando, la pull request tendrá la marca **[WIP]**. Espera a que termine; puede tardar unos minutos.

4. Selecciona la pestaña **Files changed** para revisar el código generado.
5. Revisa la implementación, prestando especial atención a:
   - los componentes de interfaz para alternar entre modos;
   - el uso del almacenamiento local para guardar las preferencias del usuario;
   - los cambios de CSS o de estilos para los modos de alto contraste y claro;
   - los atributos de accesibilidad (etiquetas ARIA, navegación por teclado, etc.);
   - el código JavaScript/TypeScript que gestiona el cambio de modo.

6. Cuando hayas revisado las actualizaciones y te parezcan correctas, vuelve a la pestaña **Conversation** y desplázate hacia abajo.
7. Deberías ver un indicador que señala que algunos flujos de trabajo esperan aprobación.
8. Si hay flujos de trabajo pendientes, selecciona **Approve and run workflows**.

   ![Aprobar y ejecutar flujos de trabajo](../../../_images/shared-approve-workflows.png)

9. Los flujos de trabajo deberían aparecer en la sección de comprobaciones de la pull request. Si todo va bien, las comprobaciones del proyecto para la aplicación Astro terminarán correctamente. Puede tardar unos minutos.
10. **Opcional:** puedes cambiar a esta rama en tu Codespace y probar manualmente las funciones de accesibilidad. Ve al Codespace, abre el terminal y ejecuta estos comandos (sustituye `<branch-name>` por el nombre de la rama creada por Copilot):

    ```bash
    git fetch origin
    git checkout <branch-name>
    ```

    Después, inicia la aplicación y prueba los controles de alto contraste y modo claro en el navegador. Comprueba que funcionan y que la configuración se conserva al volver a cargar la página.

Fíjate en cómo el agente personalizado de accesibilidad ha orientado a Copilot para implementar estas funciones según buenas prácticas de accesibilidad. Si detectas algún problema o mejora, puedes mencionarlo en un comentario con **@copilot**, igual que hiciste con las PR anteriores.

## Ejercicio opcional: seguir delegando

El agente en la nube es más útil cuando puedes asignarle incidencias reales del backlog y ocuparte de otra cosa. Para practicarlo, crea algunas incidencias más en el repositorio y asígnaselas a Copilot. Por ejemplo:

- Crear un formulario para que las personas interesadas en financiar un juego se registren desde su página de detalles.
- Implementar la paginación en la página de la lista de juegos.
- Añadir validación de entradas y gestión de errores a las funciones auxiliares de acceso a datos.

## Resumen

Has completado el recorrido del agente en la nube. En estas lecciones:

- **Revisaste los archivos de instrucciones personalizados del repositorio** para reconocer su efecto en el trabajo del agente.
- **Asignaste incidencias al agente de Copilot en la nube** y observaste cómo configura su entorno, planifica y trabaja de forma asíncrona.
- **Creaste y usaste un agente personalizado** para mejorar la accesibilidad con modos de alto contraste y claro.
- **Usaste la página de agentes de Copilot como centro de control** para supervisar y orientar la sesión de accesibilidad mientras estaba en curso.
- **Revisaste e iteraste sobre las pull requests del agente en la nube**, mencionando a `@copilot` para pedir cambios y aprobando los flujos de trabajo.

## Revisión y siguientes pasos

Has completado el recorrido del agente en la nube. Si quieres seguir explorando, los otros recorridos complementan lo que has practicado:

- 🖥️ **[Recorrido de VS Code](../../vscode/)**: explora el modo agente de Copilot Chat y la integración con MCP directamente desde el IDE.
- 💻 **[Recorrido de CLI](../../cli/)**: entrega dos cambios revisados desde el terminal con los modos Plan y Autopilot, habilidades de agente, un agente de QA personalizado, Playwright MCP, Agent Merge y el puente opcional `/delegate` hacia el agente en la nube.

En tu propio repositorio, prueba estas ideas:

- Asigna al agente en la nube una incidencia de refactorización o de cobertura de pruebas.
- Crea un agente personalizado para otro ámbito.
- Configura `copilot-setup-steps.yml` para otra pila tecnológica.

## Recursos

- [GitHub Copilot][github-copilot]
- [Acerca de los agentes de Copilot][copilot-agents]
- [Asignar incidencias de GitHub a Copilot][assign-issue]
- [Buenas prácticas para el flujo de trabajo de configuración del agente en la nube][cloud-agent-best-practices]
- [Configurar el firewall del agente de Copilot en la nube][agent-firewall]

---

| [← Lección anterior: Gestionar agentes][previous-lesson] |
|:--|

[github-copilot]: https://github.com/features/copilot
[cloud-agent-overview]: https://docs.github.com/copilot/concepts/agents/cloud-agent/about-cloud-agent
[assign-issue]: https://docs.github.com/copilot/how-tos/use-copilot-agents/cloud-agent/start-copilot-sessions
[setup-workflow]: https://docs.github.com/copilot/how-tos/copilot-on-github/customize-copilot/customize-cloud-agent/customize-the-agent-environment
[copilot-agents]: https://docs.github.com/copilot/concepts/agents/cloud-agent/about-cloud-agent
[cloud-agent-best-practices]: https://docs.github.com/copilot/how-tos/copilot-on-github/customize-copilot/customize-cloud-agent/customize-the-agent-environment
[agent-firewall]: https://docs.github.com/copilot/how-tos/copilot-on-github/customize-copilot/customize-cloud-agent/customize-the-agent-firewall

[previous-lesson]: ../4-managing-agents/
