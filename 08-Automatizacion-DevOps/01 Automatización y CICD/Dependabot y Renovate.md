---
proyecto: Conocimiento
tipo: concepto
nivel: intermedio
progreso: pendiente
estado: pendiente
area: Automatización, DevOps y operación
tags: [automatizacion-devops]
---
# Dependabot y Renovate

Parte de: [[08-Automatizacion-DevOps/01 Automatización y CICD/_Índice Automatización y CICD|Automatización y CICD]]

> [!info] Cómo avanzar
> `progreso`: **pendiente** → **viendo** (estudiando) → **visto** (contestaste la autoevaluación sin abrir las respuestas) → **dominado** (hiciste el laboratorio o lo aplicaste de verdad).

## Contenido

### Qué es
**Dependabot** (de GitHub) y **Renovate** (de Mend, de código abierto) son bots que **vigilan las dependencias** de tu repositorio (paquetes NuGet, npm, imágenes Docker, actions de GitHub, proveedores de Terraform) y **abren pull requests automáticamente** cuando hay versiones nuevas o vulnerabilidades conocidas. Convierten la actualización de dependencias, que nadie hace por gusto, en un flujo pequeño y continuo de PR revisables con el CI corriendo en cada uno.

### Cómo funciona
1. El bot lee tus archivos de dependencias (`*.csproj`, `Directory.Packages.props`, `package.json`, `package-lock.json`, `Dockerfile`, `.github/workflows/*.yml`).
2. Consulta los registros (nuget.org, npm, tu feed privado de Azure Artifacts) en el horario configurado.
3. Por cada actualización (o grupo de actualizaciones) abre un PR con el cambio, notas de la versión y, en el caso de vulnerabilidades, el aviso de seguridad.
4. Tu CI corre en el PR; si las pruebas pasan, alguien lo revisa y hace merge (o se fusiona solo, si configuraste *automerge*).

Hay dos modos en Dependabot: **security updates** (PR solo para dependencias con vulnerabilidad conocida, ligadas a las alertas) y **version updates** (mantener todo al día según `dependabot.yml`).

**Diferencias principales**:

| | Dependabot | Renovate |
|---|---|---|
| Plataformas | GitHub (nativo) | GitHub, Azure DevOps, GitLab, Bitbucket y otras |
| Configuración | `.github/dependabot.yml` | `renovate.json` (muy configurable, presets) |
| Agrupación | Grupos por patrón o tipo | Agrupación muy flexible, *dashboard* de dependencias |
| Ejecución | Servicio de GitHub | App hospedada (GitHub) o CLI en tu pipeline |

### Ejemplo
`dependabot.yml` mínimo para NuGet semanal (más las actions de GitHub):

```yaml
version: 2
updates:
  - package-ecosystem: "nuget"
    directory: "/"
    schedule:
      interval: "weekly"
    groups:
      microsoft:                       # un solo PR para los paquetes Microsoft.*
        patterns: [ "Microsoft.*", "System.*" ]
  - package-ecosystem: "github-actions"
    directory: "/"
    schedule:
      interval: "weekly"
```

**En Azure Repos**, Dependabot de GitHub no funciona de forma nativa. La opción madura es **Renovate** ejecutado desde un pipeline programado con su CLI y `platform: azure`. Microsoft también ofrece actualizaciones de seguridad con Dependabot como parte de **GitHub Advanced Security for Azure DevOps**, que abren PR para corregir alertas de dependencias; su alcance y disponibilidad general: (Pendiente de confirmar) en la documentación vigente.

```yaml
# azure-pipelines-renovate.yml: corre Renovate cada lunes a las 06:00 UTC
schedules:
- cron: "0 6 * * 1"
  branches: { include: [ main ] }
  always: true
trigger: none

pool: { vmImage: ubuntu-latest }
steps:
- task: NodeTool@0
  inputs: { versionSpec: '22.x' }
- bash: |
    git config --global user.email 'renovate@ejemplo.com'
    git config --global user.name 'Renovate'
    npx renovate
  env:
    RENOVATE_PLATFORM: azure
    RENOVATE_ENDPOINT: $(System.CollectionUri)
    RENOVATE_TOKEN: $(System.AccessToken)       # identidad del build: ver permisos abajo
    RENOVATE_REPOSITORIES: "MiProyecto/Itzamna.Dionisio"
```

Según la documentación de Renovate, la identidad *Project Collection Build Service* necesita en cada repo los permisos **Contribute**, **Contribute to pull requests**, **Create branch** y **Read**, además de «View project-level information» en el proyecto.

```json
{
  "$schema": "https://docs.renovatebot.com/renovate-schema.json",
  "extends": [ "config:recommended" ],
  "packageRules": [
    { "matchUpdateTypes": [ "patch" ], "matchDepTypes": [ "devDependencies" ], "automerge": true }
  ]
}
```

### Riesgo del automerge
Fusionar actualizaciones automáticamente ahorra tiempo, pero:
- **Cambios incompatibles no cubiertos por pruebas** llegan a `main` sin que nadie los vea.
- **Ataques a la cadena de suministro**: si un paquete legítimo publica una versión comprometida, el automerge la mete en tu código en horas. Configura una **edad mínima** de versión (Renovate tiene `minimumReleaseAge`; Dependabot tiene un periodo de enfriamiento configurable) para dejar que la comunidad detecte versiones maliciosas.
- Las pruebas pasan, pero el comportamiento cambia en algo no probado.

Úsalo solo para parches de dependencias de desarrollo o herramientas con buena cobertura, y nunca sin CI obligatorio.

### Cuándo usarlo y cuándo no
- **Úsalo siempre** en proyectos vivos: actualizar un poco cada semana es mucho más fácil que saltar tres versiones mayores una vez al año.
- En repos archivados o de solo mantenimiento, limita a actualizaciones de seguridad.
- Complementa con escaneo de vulnerabilidades: [[Escaneo de dependencias y de secretos]].

### Errores comunes
1. **Avalancha de PR** sin agrupar: el equipo los ignora. Agrupa y limita los PR abiertos.
2. **Ignorar los PR** durante meses.
3. **Feeds privados sin credenciales** configuradas para el bot.

### Para profundizar
- [Opciones de configuración de dependabot.yml](https://docs.github.com/code-security/reference/supply-chain-security/dependabot-options-reference)
- [Documentación de Renovate](https://docs.renovatebot.com/)
- [Renovate en Azure DevOps](https://docs.renovatebot.com/modules/platform/azure/)

## Autoevaluación

Contesta primero en voz alta o en papel; luego despliega la respuesta.

> [!question]- ¿Qué hacen Dependabot y Renovate?
> Abren **PRs automáticos** para actualizar dependencias (NuGet, npm, Docker, Actions) y, en el caso de Dependabot, alertas y PRs de **seguridad** por vulnerabilidades conocidas.

> [!question]- Escribe un `dependabot.yml` mínimo para NuGet semanal.
> ```yaml
> version: 2
> updates:
>   - package-ecosystem: "nuget"
>     directory: "/"
>     schedule:
>       interval: "weekly"
> ```

> [!question]- ¿Cuál usarías en Azure Repos?
> **Renovate** (se ejecuta como pipeline y soporta Azure DevOps); Dependabot está integrado en GitHub. Configura agrupación de PRs para no ahogarte en ellos.

> [!question]- Riesgo de *automerge* de dependencias.
> Una versión nueva rota o maliciosa entra sin revisión. Limítalo a parches con buena cobertura de pruebas en el pipeline.

