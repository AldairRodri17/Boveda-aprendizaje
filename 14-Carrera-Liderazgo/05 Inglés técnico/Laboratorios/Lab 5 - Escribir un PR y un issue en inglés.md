---
proyecto: Conocimiento
tipo: laboratorio
nivel: intermedio
progreso: pendiente
estado: borrador
area: Carrera y liderazgo técnico
herramienta: GitHub o Azure DevOps
tags: [carrera-liderazgo, laboratorio]
---
# Lab 5 - Escribir un PR y un issue en inglés

Parte de: [[14-Carrera-Liderazgo/05 Inglés técnico/_Índice Inglés técnico|Inglés técnico]]

**Cubre:** [[Inglés técnico - escribir PRs, issues y design docs]] · [[Vocabulario técnico en inglés]]

> [!info] Al terminar
> Si todos los criterios quedan marcados, cambia a `dominado` el `progreso` de este laboratorio y de las notas que cubre. Guarda tus archivos de práctica en esta misma carpeta.

## Objetivo
Escribir en inglés, en un repositorio real, un issue de bug y el PR que lo corrige, con calidad suficiente para un equipo internacional.

## Pasos
1. Elige un bug o mejora pequeña en uno de tus proyectos (o crea uno a propósito en un repo de práctica).
2. Escribe el issue en inglés con: *Summary*, *Steps to reproduce*, *Expected behavior*, *Actual behavior*, *Environment*.
3. Corrige el bug en una rama y escribe commits en inglés en imperativo ("Fix…", "Add…").
4. Abre el PR con título en imperativo y descripción *What / Why / How to test*, enlazando el issue ("Fixes #12").
5. Revisa tu texto buscando falsos amigos (*actually*, *eventually*, *assist*) y tiempos verbales.
6. Pide a alguien (o a una herramienta de corrección) revisar el inglés y aplica los cambios.

## Criterios de terminado
- [ ] El issue tiene las 5 secciones y alguien más puede reproducir el bug solo con él.
- [ ] Todos los commits están en inglés y en imperativo.
- [ ] El PR enlaza el issue y lo cierra al fusionarse.
- [ ] No hay falsos amigos ni calcos del español en el texto final.
- [ ] Recibiste al menos una corrección de inglés y la aplicaste.

## Solución

> [!success]- Ejemplo de issue
> ```markdown
> **Summary:** Hidden projects are still visible through the direct link.
>
> **Steps to reproduce:**
> 1. Hide a project from the dashboard.
> 2. Log out and open the project's direct URL.
>
> **Expected behavior:** A 404 page.
> **Actual behavior:** The project page is displayed.
> **Environment:** Production, Chrome, build from main.
> ```

> [!success]- Ejemplo de PR
> ```markdown
> **Title:** Return 404 for hidden projects on public routes
>
> **What:** The public project endpoint now filters out hidden projects.
> **Why:** Hidden projects were reachable through their direct link (#12).
> **How to test:** Hide a project, log out and open its URL; you should get a 404.
>
> Fixes #12
> ```

## Notas personales

_Qué me costó, errores que cometí, dudas._
