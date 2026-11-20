---
proyecto: Conocimiento
tipo: concepto
nivel: intermedio
progreso: pendiente
estado: pendiente
area: Automatización, DevOps y operación
tags: [automatizacion-devops]
---
# Frecuencia de despliegue (DORA)

Parte de: [[Métricas DORA]]

> [!info] Cómo avanzar
> `progreso`: **pendiente** → **viendo** (estudiando) → **visto** (contestaste la autoevaluación sin abrir las respuestas) → **dominado** (hiciste el laboratorio o lo aplicaste de verdad).

## Contenido

### Qué es
La **frecuencia de despliegue** mide **qué tan seguido tu organización despliega cambios a producción** (o los libera a los usuarios finales) para un servicio dado. Es una de las [[Métricas DORA]] del grupo de rendimiento. Se expresa como despliegues por día, por semana o por mes, o como la pregunta «¿con qué frecuencia desplegamos normalmente?»: bajo demanda varias veces al día, una vez por semana, una vez al mes, cada varios meses.

### Cómo funciona
La métrica es un **reflejo del tamaño de lote**. Si despliegas una vez al mes, cada despliegue lleva un mes de cambios mezclados: más riesgo, más difícil encontrar qué rompió, más miedo a desplegar, y por lo tanto se despliega todavía menos. Desplegar seguido obliga a lotes pequeños, que son más fáciles de revisar, probar y revertir.

**Qué suele frenarla**:
- **Pasos manuales**: pruebas de regresión a mano, copiar archivos por FTP, ejecutar scripts de base de datos a mano.
- **Ventanas de cambio y aprobaciones pesadas**: comités semanales o despliegues solo en fin de semana.
- **Ramas de vida larga** y *merges* grandes; ver [[Estrategias de ramas (GitFlow y trunk-based)]].
- **Acoplamiento**: no puedes desplegar la API sin desplegar a la vez el front y otros tres servicios.
- **Miedo justificado**: sin pruebas automáticas, monitoreo ni rollback rápido, cada despliegue es una apuesta.
- **Migraciones de base de datos acopladas al código** que obligan a ventanas de mantenimiento.

Las palancas para subirla: [[CI y CD - conceptos|CI y CD]] completo, pruebas automatizadas, [[Feature flags]] para separar despliegue de liberación, migraciones compatibles hacia atrás y [[Estrategias de despliegue (blue-green y canary)]] que eliminan la caída.

### Ejemplo
**Cómo obtenerla en Azure Pipelines.** Si tu despliegue a producción es un *deployment job* asociado a un *environment* (`dionisio-prod`), Azure DevOps guarda el historial de cada despliegue a ese ambiente. Puedes verlo en *Pipelines → Environments → dionisio-prod → Deployments* o consultarlo por la [[Azure DevOps REST API]] y contar por periodo.

```powershell
# Cuenta despliegues exitosos al environment de producción en los últimos 30 días
$org = 'mi-organizacion'; $proyecto = 'Itzamna'; $envId = 7      # id del environment
$basic = [Convert]::ToBase64String([Text.Encoding]::ASCII.GetBytes(":$env:AZDO_PAT"))
$uri = "https://dev.azure.com/$org/$proyecto/_apis/distributedtask/environments/$envId/environmentdeploymentrecords?api-version=7.1"

$registros = (Invoke-RestMethod -Uri $uri -Headers @{ Authorization = "Basic $basic" }).value
$desde = (Get-Date).AddDays(-30)
$exitosos = $registros | Where-Object { $_.result -eq 'succeeded' -and [datetime]$_.finishTime -ge $desde }

"{0} despliegues en 30 días = {1:N2} por día" -f $exitosos.Count, ($exitosos.Count / 30)
```

Alternativas: filtrar los *runs* del pipeline de release por etapa `Prod` exitosa, o usar *Analytics* (OData) de Azure DevOps para un tablero en Power BI. En GitHub, el equivalente son los *deployments* de un environment.

Cuenta solo despliegues **a producción** del servicio que mides; los despliegues a dev o QA no cuentan.

### Cuándo usarlo y cuándo no
- **Úsala** como indicador de tendencia: si baja trimestre a trimestre, algo está volviendo pesado el proceso.
- **No la persigas sola**: desplegar más seguido sin cuidar la [[Tasa de fallos de cambios (DORA)]] solo produce más incidentes.
- En productos con liberaciones sujetas a terceros (apps móviles en tiendas), mide la frecuencia de despliegue al canal interno o de pruebas además del público.

### Errores comunes
1. **Inflarla** con despliegues vacíos o redespliegues del mismo artefacto.
2. **Mezclar servicios**: sumar despliegues de diez microservicios y presentarlo como la frecuencia de «el sistema».
3. **Contar despliegues fallidos** como si fueran entregas de valor; registra el resultado.
4. **Medir a mano** en una hoja de cálculo que nadie actualiza.

### Para profundizar
- [DORA: métricas de entrega de software](https://dora.dev/guides/dora-metrics-four-keys/)
- [Azure Pipelines: environments](https://learn.microsoft.com/azure/devops/pipelines/process/environments)
- [Modelo de un registro de despliegue de environment (EnvironmentDeploymentExecutionRecord)](https://learn.microsoft.com/javascript/api/azure-devops-extension-api/environmentdeploymentexecutionrecord)

## Autoevaluación

Contesta primero en voz alta o en papel; luego despliega la respuesta.

> [!question]- ¿Qué mide?
> Con qué frecuencia se despliega con éxito a producción (o se libera a usuarios).

> [!question]- ¿Qué suele frenar esta métrica?
> Lotes grandes, aprobaciones manuales lentas, ambientes compartidos y falta de pruebas automáticas que den confianza.

> [!question]- ¿Cómo la obtienes en Azure Pipelines?
> Del historial del **environment** `prod`: número de deployment jobs exitosos por periodo.

