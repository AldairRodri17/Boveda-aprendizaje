---
proyecto: Conocimiento
tipo: laboratorio
nivel: intermedio
progreso: pendiente
estado: borrador
area: Azure
herramienta: Azure DevOps (organización gratuita) y Azure Pipelines YAML
tags: [azure, laboratorio]
---
# Lab 5 - Pipeline YAML con stages y aprobaciones

Parte de: [[07-Azure/09 Azure DevOps/_Índice Azure DevOps|Azure DevOps]]

**Cubre:** [[Azure Pipelines]] · [[Azure Pipelines - environments y aprobaciones]] · [[Azure Pipelines - agentes]] · [[Azure Repos]]

> [!info] Al terminar
> Si todos los criterios quedan marcados, cambia a `dominado` el `progreso` de este laboratorio y de las notas que cubre. Guarda tus archivos de práctica en esta misma carpeta.

## Objetivo

Construir un pipeline con tres stages (Build → Dev → Prod) que publica un artefacto, despliega con deployment jobs y se detiene en una aprobación manual antes de producción.

## Pasos

1. Crea un proyecto en Azure DevOps e importa o sube la API del Lab 2 a Azure Repos.
2. Crea dos environments: `dev` (sin checks) y `prod` con un check de **Approvals** donde tú seas el aprobador.
3. Escribe `azure-pipelines.yml` con un stage `Build` que compile, pruebe y publique el artefacto `api`.
4. Agrega el stage `Deploy_Dev` con un deployment job al environment `dev` que liste el artefacto descargado.
5. Agrega `Deploy_Prod` que dependa de `Deploy_Dev` y solo corra si la rama es `main`.
6. Ejecuta el pipeline; aprueba `prod` en una corrida y **rechaza** en otra.
7. (Opcional) Reemplaza el `echo` de prod por `AzureWebApp@1` usando una service connection con federación de identidad.
8. Si el job queda en cola sin agente, revisa el paralelismo de la organización o registra un agente autohospedado.

## Criterios de terminado

- [ ] La corrida muestra 3 stages y `Deploy_Prod` queda en *Waiting* hasta tu aprobación.
- [ ] Al rechazar, `Deploy_Prod` aparece como rechazado/omitido y no se despliega.
- [ ] El environment `prod` muestra el historial de despliegues con el número de corrida.
- [ ] Desde una rama distinta de `main`, `Deploy_Prod` se omite.
- [ ] El YAML no contiene ningún secreto.

## Solución

> [!success]- Solución: azure-pipelines.yml
> ```yaml
> trigger:
>   branches:
>     include: [ main ]
>
> variables:
>   buildConfiguration: Release
>
> stages:
> - stage: Build
>   jobs:
>   - job: Build
>     pool:
>       vmImage: ubuntu-24.04
>     steps:
>     - task: UseDotNet@2
>       inputs:
>         version: 10.0.x
>     - script: dotnet build -c $(buildConfiguration)
>     - script: dotnet test -c $(buildConfiguration) --no-build
>     - script: dotnet publish -c $(buildConfiguration) -o $(Build.ArtifactStagingDirectory)/api
>     - task: PublishPipelineArtifact@1
>       inputs:
>         targetPath: $(Build.ArtifactStagingDirectory)/api
>         artifact: api
>
> - stage: Deploy_Dev
>   dependsOn: Build
>   jobs:
>   - deployment: Dev
>     environment: dev
>     pool:
>       vmImage: ubuntu-24.04
>     strategy:
>       runOnce:
>         deploy:
>           steps:
>           - script: ls -R $(Pipeline.Workspace)/api
>
> - stage: Deploy_Prod
>   dependsOn: Deploy_Dev
>   condition: and(succeeded(), eq(variables['Build.SourceBranch'], 'refs/heads/main'))
>   jobs:
>   - deployment: Prod
>     environment: prod
>     pool:
>       vmImage: ubuntu-24.04
>     strategy:
>       runOnce:
>         deploy:
>           steps:
>           - script: echo "Desplegando $(Build.BuildNumber) a prod"
> ```

> [!success]- Solución: aprobación y despliegue real
> La aprobación **no va en el YAML**: Pipelines → Environments → `prod` → ⋯ → *Approvals and checks* → *Approvals*. Para desplegar de verdad:
> ```yaml
>           - task: AzureWebApp@1
>             inputs:
>               azureSubscription: sc-azure-lab     # service connection (workload identity federation)
>               appType: webAppLinux
>               appName: api-lab-xxxx
>               package: $(Pipeline.Workspace)/api
> ```
> Si no hay paralelismo hospedado, instala un agente autohospedado (Project settings → Agent pools → Default → New agent) y usa `pool: Default`.

## Notas personales

_Qué me costó, errores que cometí, dudas._
