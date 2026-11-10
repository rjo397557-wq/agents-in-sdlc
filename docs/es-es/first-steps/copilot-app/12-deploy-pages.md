---
title: "Lección 12 - Desplegar el sitio con GitHub Pages"
description: "Usa la aplicación GitHub Copilot para configurar un flujo de trabajo de Pages, revisar su pull request y verificar que Space Quiz está disponible."
authors:
  - jamesmontemagno
lastUpdated: 2026-10-12
---

Publica Space Quiz en Internet con GitHub Pages. Copilot puede preparar el flujo de trabajo y guiarte durante el despliegue, pero tú eres responsable de revisar los cambios y confirmar que el sitio está disponible.

En esta lección vas a:

- pedir a Copilot que configure el despliegue de GitHub Pages;
- revisar el flujo de trabajo y la pull request antes de combinarla;
- seguir el despliegue y verificar el sitio publicado.

## Escenario

Space Quiz ya está en un repositorio de GitHub, pero hace falta una forma sencilla de probarlo. Publicar el sitio de un solo archivo con GitHub Pages permite acceder a él mediante una URL pública sin añadir un servidor ni una dependencia de runtime.

## Pedir a Copilot que despliegue el sitio

En la sesión de la aplicación Copilot para `space-quiz`, envía este prompt:

```plaintext
Enable GitHub Pages for this repository. Create a GitHub Actions workflow that builds the website if needed and deploys it to GitHub Pages. Add a README describing the app and how to run it locally if one does not already exist. Create a pull request with these changes and merge it after required checks pass. Monitor the deployment until it succeeds, then report the final URL of the live website. If you lack permission to enable Pages or merge, tell me exactly what I need to approve or do; do not report success until the site is live.
```

![Diagrama de tres fases: una pull request de space-quiz combinada; una ejecución correcta de Deploy website en GitHub Actions con los pasos Build website, Upload artifact y Deploy to Pages; y Space Quiz publicado en yourname.github.io/space-quiz/.](../../../_images/first-steps-app-pages-deploy.svg)

> [!WARNING]
> Este prompt pide al agente que cree y combine una pull request y modifique la configuración de Pages del repositorio. Revisa las diferencias, los permisos de despliegue y las solicitudes de aprobación de GitHub antes de continuar.

## Revisar el flujo de trabajo y verificar el despliegue

1. Confirma que el archivo README solo se añade si el repositorio todavía no tiene uno.
2. Revisa los cambios de la pull request, incluidos los pasos de compilación y despliegue del flujo de trabajo.
3. Combina la pull request solo cuando se hayan superado las comprobaciones obligatorias.
4. Sigue la ejecución de despliegue de GitHub Actions hasta que finalice correctamente.
5. Abre la URL indicada y confirma que se carga Space Quiz.

Si el agente no puede habilitar Pages o combinar la pull request, sigue las instrucciones concretas que te proporcione. No des la tarea por terminada hasta que el despliegue se complete correctamente y el sitio publicado se cargue.

## Resumen y siguientes pasos

Space Quiz ya está disponible como sitio publicado y has verificado el despliegue en lugar de fiarte del informe del agente. Continúa con la [Lección 13: Revisión y próximos pasos][next-lesson].

[next-lesson]: ../13-review/
