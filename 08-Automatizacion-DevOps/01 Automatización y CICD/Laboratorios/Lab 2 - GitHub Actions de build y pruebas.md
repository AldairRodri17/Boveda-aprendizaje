---
proyecto: Conocimiento
tipo: laboratorio
nivel: intermedio
progreso: pendiente
estado: borrador
area: Automatización, DevOps y operación
herramienta: GitHub (repositorio gratuito) y GitHub Actions
tags: [automatizacion-devops, laboratorio]
---
# Lab 2 - GitHub Actions de build y pruebas

Parte de: [[08-Automatizacion-DevOps/01 Automatización y CICD/_Índice Automatización y CICD|Automatización y CICD]]

**Cubre:** [[GitHub Actions]] · [[CI y CD - conceptos]] · [[Análisis estático (SonarQube y linters)]]

> [!info] Al terminar
> Si todos los criterios quedan marcados, cambia a `dominado` el `progreso` de este laboratorio y de las notas que cubre. Guarda tus archivos de práctica en esta misma carpeta.

## Objetivo

Montar un workflow de CI que compile, verifique formato y ejecute pruebas en cada push y PR, que publique los resultados y que bloquee el merge si algo falla.

## Pasos

1. Crea una solución con la API y un proyecto de pruebas xUnit con al menos dos pruebas.
2. Sube el código a un repositorio de GitHub.
3. Crea `.github/workflows/ci.yml` que corra en `push` a `main` y en `pull_request`, con `permissions: contents: read`.
4. Pasos: checkout, instalar .NET, restore, build Release, `dotnet format --verify-no-changes`, pruebas con resultados `.trx` y subir los resultados como artefacto aun si fallan.
5. Abre un PR con una prueba que falle a propósito y observa el check en rojo.
6. Configura una regla de protección de `main` que exija el check `build` para hacer merge.
7. Corrige la prueba y completa el PR.
8. (Opcional) Agrega una `matrix` con `ubuntu-24.04` y `windows-latest`.

## Criterios de terminado

- [ ] El workflow corre automáticamente en el PR y aparece como check.
- [ ] Con la prueba rota el check queda en rojo y GitHub no permite el merge.
- [ ] El artefacto `test-results` se puede descargar desde la corrida.
- [ ] Un archivo mal formateado hace fallar el paso de `dotnet format`.
- [ ] Tras corregir, el check queda en verde y el PR se fusiona.

## Solución

> [!success]- Solución: .github/workflows/ci.yml
> ```yaml
> name: ci
> on:
>   push:
>     branches: [ main ]
>   pull_request:
>
> permissions:
>   contents: read
>
> jobs:
>   build:
>     runs-on: ubuntu-24.04
>     steps:
>       - uses: actions/checkout@v4
>       - uses: actions/setup-dotnet@v4
>         with:
>           dotnet-version: '10.0.x'
>       - run: dotnet restore
>       - run: dotnet build --no-restore -c Release
>       - run: dotnet format --verify-no-changes --no-restore
>       - run: dotnet test --no-build -c Release --logger trx --results-directory TestResults
>       - uses: actions/upload-artifact@v4
>         if: always()
>         with:
>           name: test-results
>           path: TestResults
> ```
> Revisa la versión mayor vigente de cada action en su repositorio (Pendiente de confirmar si ya hay una más nueva que `@v4`).

> [!success]- Solución: protección de rama y matrix
> Settings → Branches (o Rules → Rulesets) → regla para `main` → *Require status checks to pass* → elige `build`.
> ```yaml
>   build:
>     strategy:
>       matrix:
>         os: [ ubuntu-24.04, windows-latest ]
>     runs-on: ${{ matrix.os }}
> ```
> Con matrix, los checks se llaman `build (ubuntu-24.04)` y `build (windows-latest)`: actualiza la regla.

## Notas personales

_Qué me costó, errores que cometí, dudas._
