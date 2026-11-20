---
proyecto: Conocimiento
tipo: concepto
nivel: intermedio
progreso: pendiente
estado: pendiente
area: Automatización, DevOps y operación
tags: [automatizacion-devops]
---
# Azure Functions programadas

Parte de: [[08-Automatizacion-DevOps/01 Automatización y CICD/_Índice Automatización y CICD|Automatización y CICD]]

> [!info] Cómo avanzar
> `progreso`: **pendiente** → **viendo** (estudiando) → **visto** (contestaste la autoevaluación sin abrir las respuestas) → **dominado** (hiciste el laboratorio o lo aplicaste de verdad).

## Contenido

### Qué es
Una **Azure Function programada** es una función con **timer trigger**: Azure la ejecuta según un horario, sin servidor que administrar. Es la opción natural en Azure para tareas recurrentes escritas en .NET: purgar registros, generar un reporte nocturno, sincronizar catálogos o renovar caches. Se beneficia de lo que da [[Azure Functions]]: escalado, logs en [[Application Insights]], [[Managed Identity]] y despliegue desde el mismo pipeline que tu API.

Para .NET, el modelo vigente es el **worker aislado** (*isolated worker*); el soporte del modelo *in-process* termina el **10 de noviembre de 2026**, y la guía de migración de Microsoft recomienda apuntar a **.NET 10**.

### Cómo funciona
El horario es una expresión **NCRONTAB** de **6 campos**: igual que cron pero con **segundos al inicio**.

```text
{segundo} {minuto} {hora} {día} {mes} {día-de-la-semana}
```

Ejemplos:

| Necesidad | NCRONTAB |
|---|---|
| Todos los días a las 06:00 UTC | `0 0 6 * * *` |
| Lunes a viernes a las 02:30 UTC | `0 30 2 * * 1-5` |
| Cada 15 min de 8 a 18 h (incluye 18:00…18:45) | `0 */15 8-18 * * *` |
| 23:00 de Ciudad de México (UTC−6) | `0 0 5 * * *` (05:00 UTC del día siguiente) |

En el rango de horas `8-18` se incluye **toda la hora 18**, por eso hay ejecuciones a las 18:15, 18:30 y 18:45.

**Zona horaria**: por defecto el horario se evalúa en **UTC**. En algunos planes se puede cambiar con la configuración `WEBSITE_TIME_ZONE`, pero **no está soportado en el plan Consumo de Linux** (Microsoft documenta que causa problemas de SSL y métricas). Lo más portable es dejar UTC y convertir: Ciudad de México está en UTC−6 todo el año desde que se eliminó el horario de verano en 2022, así que 23:00 local = 05:00 UTC.

**Escalado y ejecución única**: aunque la Function App escale a varias instancias, el timer trigger usa un **bloqueo (lease) en la cuenta de Storage** para que **solo una instancia** ejecute cada ocurrencia. No corre 3 veces con 3 instancias. Además, si la app estuvo apagada y se perdió una ejecución, `ScheduleStatus` y la propiedad `IsPastDue` te avisan del retraso.

**`RunOnStartup = true`** ejecuta la función **cada vez que el host arranca**: en cada despliegue, reinicio, escalado o cuando el plan Consumo «despierta» la app tras inactividad. En producción eso significa ejecuciones impredecibles (imagina una purga o un envío de correos disparado por un reinicio). Úsalo solo en desarrollo.

### Ejemplo
Purgar la bitácora vieja de [[Itzamna.Dionisio]] llamando a un procedimiento con Dapper:

```csharp
using Dapper;
using Microsoft.Azure.Functions.Worker;
using Microsoft.Data.SqlClient;
using Microsoft.Extensions.Configuration;
using Microsoft.Extensions.Logging;

public sealed class PurgarBitacora(IConfiguration config, ILogger<PurgarBitacora> log)
{
    // 23:00 hora de Ciudad de México = 05:00 UTC
    [Function(nameof(PurgarBitacora))]
    public async Task Run([TimerTrigger("0 0 5 * * *")] TimerInfo timer, CancellationToken ct)
    {
        if (timer.IsPastDue)
            log.LogWarning("La ejecución llegó tarde respecto a su horario");

        await using var cn = new SqlConnection(config.GetConnectionString("Dionisio"));
        var borrados = await cn.ExecuteScalarAsync<int>(new CommandDefinition(
            "dbo.usp_Bitacora_PurgarAntiguos",
            new { DiasRetencion = 90 },
            commandType: System.Data.CommandType.StoredProcedure,
            commandTimeout: 300,
            cancellationToken: ct));

        log.LogInformation("Purga terminada: {Borrados} registros", borrados);
        // Siguiente ejecución programada: timer.ScheduleStatus?.Next
    }
}
```

Para no fijar el horario en código, usa un parámetro de configuración: `[TimerTrigger("%HorarioPurga%")]` y define `HorarioPurga` en los *app settings*; así lo cambias por ambiente sin recompilar. La cadena de conexión puede usar autenticación de Entra con la identidad administrada de la Function, sin contraseña.

### Cuándo usarlo y cuándo no
- **Úsalo** para tareas recurrentes en .NET que necesitan logs, alertas y despliegue versionado.
- **Para trabajos largos** (más de los minutos que permite el plan) divide en lotes, encola trabajo para otras funciones, o usa Durable Functions o un Container Apps Job.
- **Para SQL puro** dentro de Azure SQL, Elastic Jobs puede bastar; para flujos sin código, [[Logic Apps]].
- Compara alternativas en [[Alternativas para automatizar (comparativa)]] y [[cron y Programador de tareas]].

### Errores comunes
1. **`RunOnStartup = true` en producción**: ejecuciones extra en cada reinicio o escalado.
2. **Pensar en hora local**: la expresión se evalúa en UTC salvo configuración explícita.
3. **Confundir cron de 5 campos con NCRONTAB de 6**: `0 5 * * *` en Functions es inválido o significa otra cosa.
4. **Tareas no idempotentes**: si una ejecución falla a la mitad y se reintenta, no debe duplicar efectos. Procesa por lotes con marcas de avance.
5. **Compartir la cuenta de Storage** entre apps con el mismo nombre de función sin cuidar el *host ID*: los bloqueos chocan.

### Para profundizar
- [Timer trigger de Azure Functions](https://learn.microsoft.com/azure/azure-functions/functions-bindings-timer)
- [Migrar apps .NET al modelo de worker aislado](https://learn.microsoft.com/azure/azure-functions/migrate-dotnet-to-isolated-model)
- [AZFD0010: Linux Consumption no soporta TZ ni WEBSITE_TIME_ZONE](https://learn.microsoft.com/azure/azure-functions/errors-diagnostics/diagnostic-events/azfd0010)

## Ejercicios

Resuélvelos en papel o en la herramienta; la solución está plegada.

> [!question]- Expresión NCRONTAB para todos los días a las 06:00 UTC.
> `0 0 6 * * *`

> [!question]- De lunes a viernes a las 02:30 UTC.
> `0 30 2 * * 1-5`

> [!question]- Cada 15 minutos en horario de 8 a 18 h (incluye 18:00, 18:15… hasta 18:45).
> `0 */15 8-18 * * *`

> [!question]- Quieres que corra a las 23:00 hora de Ciudad de México (UTC−6, sin horario de verano). ¿Qué hora UTC pones?
> 23:00 − (−6) = 05:00 UTC del día siguiente → `0 0 5 * * *`.

## Autoevaluación

Contesta primero en voz alta o en papel; luego despliega la respuesta.

> [!question]- ¿Qué formato usa el timer trigger?
> **NCRONTAB** de 6 campos: `{segundo} {minuto} {hora} {día} {mes} {día-semana}`. Ej.: `0 */5 * * * *` = cada 5 minutos.

> [!question]- ¿En qué zona horaria corre el horario?
> En **UTC** por defecto. Se puede cambiar con `WEBSITE_TIME_ZONE` con restricciones según SO y plan (verificar en la página oficial); si no, convierte tú la hora a UTC.

> [!question]- Si la app escala a 3 instancias, ¿la tarea corre 3 veces?
> No: el timer trigger usa un **lease** en la cuenta de almacenamiento para que solo una instancia ejecute cada ocurrencia.

> [!question]- Error común: `RunOnStartup = true` en producción.
> Ejecuta la función cada vez que la app arranca o escala, de forma impredecible. Úsalo solo para depurar.

