---
proyecto: Conocimiento
tipo: concepto
nivel: intermedio
progreso: pendiente
estado: pendiente
area: Azure
tags: [azure]
---
# Azure Artifacts

Parte de: [[07-Azure/09 Azure DevOps/_Índice Azure DevOps|Azure DevOps]]

> [!info] Cómo avanzar
> `progreso`: **pendiente** → **viendo** (estudiando) → **visto** (contestaste la autoevaluación sin abrir las respuestas) → **dominado** (hiciste el laboratorio o lo aplicaste de verdad).

## Contenido

### Qué es
Azure Artifacts es el servicio de Azure DevOps para alojar **feeds privados de paquetes**: NuGet, npm, Maven, Python, Cargo y Universal Packages (verificado en Microsoft Learn). Sirve para compartir librerías internas entre equipos (por ejemplo, un paquete común de ProblemDetails o de acceso a datos con Dapper) y para **cachear paquetes públicos** de nuget.org o npmjs, de modo que tus builds no dependan de que la versión pública siga existiendo. El nivel gratuito incluye **2 GiB** de almacenamiento por organización.

### Cómo funciona
**Feeds.** Un feed puede tener alcance de **proyecto** o de **organización**, y en un mismo feed conviven varios tipos de paquete. Los permisos se dan por feed (Reader, Collaborator, Contributor, Owner). La **identidad del build** del proyecto necesita al menos rol Contributor para publicar.

**Inmutabilidad.** Una versión publicada queda **reservada para siempre**: no puedes subir otra cosa con el mismo número, aunque borres la anterior. Por eso las versiones se generan automáticamente en el pipeline (con SemVer y sufijos de prerelease como `1.4.0-ci.231`).

**Upstream sources.** Configuras el feed para que, si un paquete no está publicado directamente en él, lo busque en nuget.org, npmjs u otro feed. Cuando alguien con rol Collaborator o superior lo instala, Azure Artifacts **guarda una copia** en tu feed. Orden de búsqueda: primero lo publicado en el feed, luego lo ya guardado de upstreams, y al final cada upstream en el orden configurado. Beneficios: si un paquete público se despublica o el registro público se cae, tus builds siguen funcionando; y tienes un solo origen en tu `nuget.config`, lo que también reduce ataques de *dependency confusion* (alguien publica en nuget.org un paquete con el nombre de tu paquete interno).

**Vistas.** Cada feed trae tres vistas: **`@local`** (todo lo publicado y lo guardado de upstreams; la predeterminada), **`@prerelease`** y **`@release`** (estas dos se pueden renombrar o borrar). Publicas siempre al feed y luego **promueves** una versión a `@prerelease` cuando pasa pruebas y a `@release` cuando está lista. Los consumidores apuntan a la vista que quieren: producción solo ve lo promovido.

```mermaid
flowchart LR
  CI[Pipeline CI] -->|publica 1.4.0-ci.231| F[(Feed itzamna @local)]
  F -->|promover| PR[@prerelease]
  PR -->|promover| RE[@release]
  U[nuget.org] -->|upstream: guarda copia| F
  Apps[Apps de producción] -->|leen| RE
```

### Ejemplo
Publicar el paquete `Itzamna.Comun.ProblemDetails` desde Azure Pipelines y consumirlo en otro proyecto:

```yaml
# azure-pipelines.yml del repo Itzamna.Comun
trigger:
  branches:
    include: [ main ]

pool:
  vmImage: ubuntu-24.04

variables:
  version: 1.4.$(Build.BuildId)

steps:
  - task: UseDotNet@2
    inputs:
      version: 10.0.x
  - script: dotnet pack src/Itzamna.Comun.ProblemDetails -c Release -p:PackageVersion=$(version) -o $(Build.ArtifactStagingDirectory)
    displayName: Empaquetar
  - task: NuGetAuthenticate@1
  - script: dotnet nuget push "$(Build.ArtifactStagingDirectory)/*.nupkg" --source "https://pkgs.dev.azure.com/itzamna/_packaging/itzamna/nuget/v3/index.json" --api-key az
    displayName: Publicar en Azure Artifacts
```

```xml
<!-- nuget.config en Itzamna.Dionisio: un solo origen, el feed con upstream a nuget.org -->
<configuration>
  <packageSources>
    <clear />
    <add key="itzamna" value="https://pkgs.dev.azure.com/itzamna/_packaging/itzamna@release/nuget/v3/index.json" />
  </packageSources>
</configuration>
```

En tu PC te autenticas con el *Azure Artifacts Credential Provider* (se instala con un script oficial) y `dotnet restore --interactive`. En el pipeline, `NuGetAuthenticate@1` lo resuelve. Ojo con apuntar a `@release`: lo que no esté promovido ahí (incluidas dependencias guardadas de upstream) no será visible; muchos equipos apuntan a `@local` en CI y a `@release` solo para consumidores externos.

### Cuándo usarlo y cuándo no
Úsalo para librerías internas compartidas entre varios repos, para tener control y caché de dependencias públicas, o para distribuir binarios con Universal Packages. Si el código compartido lo usa un solo repositorio, una referencia de proyecto es más simple que un paquete. Si tu equipo vive en GitHub, GitHub Packages es la alternativa. Vigila los 2 GiB gratis: los paquetes de CI se acumulan rápido; configura **políticas de retención** para borrar versiones viejas.

### Errores comunes
- **Varios orígenes en `nuget.config`** (feed y nuget.org a la vez): restauraciones lentas y riesgo de *dependency confusion*. Un solo feed con upstreams.
- **Intentar republicar una versión** después de un error: está reservada; incrementa la versión.
- **La identidad del build sin permiso** de Contributor en el feed: el push falla con 403.
- **Sin retención**: el feed se llena de miles de versiones `-ci` y se pasa del nivel gratuito.
- **Promover a `@release` sin pruebas**: la vista pierde su significado.

### Para profundizar
- [Conceptos clave de Azure Artifacts](https://learn.microsoft.com/azure/devops/artifacts/artifacts-key-concepts)
- [Upstream sources](https://learn.microsoft.com/azure/devops/artifacts/concepts/upstream-sources)
- [Vistas de feed](https://learn.microsoft.com/azure/devops/artifacts/concepts/views)

## Autoevaluación

Contesta primero en voz alta o en papel; luego despliega la respuesta.

> [!question]- ¿Qué es Azure Artifacts?
> Feeds privados de paquetes (**NuGet**, npm, Maven, Python, Cargo y Universal Packages) para compartir librerías entre equipos y cachear paquetes públicos.

> [!question]- ¿Qué son las *upstream sources*?
> Permiten que tu feed consulte y **guarde copia** de paquetes de nuget.org o npmjs; si el paquete público desaparece, sigues teniendo la versión usada.

> [!question]- ¿Qué son las vistas `@local`, `@prerelease` y `@release`?
> Etiquetas para promover versiones: publicas en el feed y luego promueves a `@prerelease` o `@release`; los consumidores apuntan a la vista que quieren.

