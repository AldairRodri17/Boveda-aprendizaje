---
proyecto: Conocimiento
tipo: laboratorio
nivel: basico
progreso: pendiente
estado: borrador
area: Seguridad
herramienta: .NET SDK, npm, gitleaks, GitHub (Dependabot y Actions)
tags: [seguridad, laboratorio]
---
# Lab 9 - Escaneo de dependencias y secretos en un repositorio propio

Parte de: [[12-Seguridad/05 Herramientas defensivas/_Índice Herramientas defensivas|Herramientas defensivas]]

**Cubre:** [[Escaneo de dependencias y de secretos]] · [[SAST, DAST y SCA]] · [[Pruebas de seguridad en el pipeline]] · [[Componentes y dependencias vulnerables]] · [[Secretos en repositorios]]

> [!info] Al terminar
> Si todos los criterios quedan marcados, cambia a `dominado` el `progreso` de este laboratorio y de las notas que cubre. Guarda tus archivos de práctica en esta misma carpeta.

## Objetivo
En un repositorio tuyo de práctica, encontrar dependencias vulnerables y secretos (uno plantado por ti), y automatizar ambos escaneos en el pipeline.

## Pasos
1. Crea un repo de práctica con una API .NET y, si quieres, un front con npm.
2. Agrega a propósito una versión vieja de algún paquete con vulnerabilidad conocida (busca uno en el GitHub Advisory Database) y ejecuta `dotnet list package --vulnerable --include-transitive`.
3. Ejecuta `npm audit` en el front.
4. Planta un secreto **falso** en un archivo (por ejemplo una cadena con formato de llave de acceso inventada) y haz commit.
5. Instala gitleaks y ejecuta un escaneo de todo el historial.
6. Configura un workflow de GitHub Actions que corra en cada pull request: build, `dotnet list package --vulnerable` (fallando si hay resultados) y gitleaks.
7. Activa Dependabot para NuGet y npm con un `dependabot.yml`.
8. Elimina el secreto falso, actualiza el paquete y verifica que el pipeline quede en verde.

## Criterios de terminado
- [ ] `dotnet list package --vulnerable` mostró el paquete viejo y, tras actualizarlo, ya no muestra nada.
- [ ] gitleaks detectó el secreto falso en el historial.
- [ ] El workflow falla con el paquete vulnerable y pasa al corregirlo.
- [ ] Dependabot abrió (o está configurado para abrir) pull requests de actualización.
- [ ] Anotaste qué harías si el secreto hubiera sido real (rotar primero).

## Solución

> [!success]- Workflow mínimo
> ```yaml
> name: seguridad
> on: [pull_request]
> jobs:
>   escaneo:
>     runs-on: ubuntu-latest
>     steps:
>       - uses: actions/checkout@v4
>         with: { fetch-depth: 0 }
>       - uses: actions/setup-dotnet@v4
>         with: { dotnet-version: '8.0.x' }
>       - run: dotnet restore
>       - name: Dependencias vulnerables
>         run: |
>           dotnet list package --vulnerable --include-transitive | tee vuln.txt
>           ! grep -q "has the following vulnerable packages" vuln.txt
>       - uses: gitleaks/gitleaks-action@v2
>         env: { GITHUB_TOKEN: ${{ secrets.GITHUB_TOKEN }} }
> ```
> Revisa en el repositorio de gitleaks-action si tu tipo de cuenta requiere licencia (verificar en la página oficial). Fija las acciones por hash de commit en proyectos reales.

> [!success]- dependabot.yml
> ```yaml
> version: 2
> updates:
>   - package-ecosystem: "nuget"
>     directory: "/"
>     schedule: { interval: "weekly" }
>   - package-ecosystem: "npm"
>     directory: "/web"
>     schedule: { interval: "weekly" }
> ```

## Notas personales

_Qué me costó, errores que cometí, dudas._
