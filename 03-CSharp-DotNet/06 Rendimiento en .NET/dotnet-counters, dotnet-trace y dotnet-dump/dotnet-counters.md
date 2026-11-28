---
proyecto: Conocimiento
tipo: concepto
nivel: avanzado
progreso: pendiente
estado: pendiente
area: C# y .NET
tags: [csharp-dotnet]
contenido: completo
---
# dotnet-counters

Parte de: [[dotnet-counters, dotnet-trace y dotnet-dump]]

> [!info] Cómo avanzar
> `progreso`: **pendiente** → **viendo** (estudiando) → **visto** (contestaste la autoevaluación sin abrir las respuestas) → **dominado** (hiciste el laboratorio o lo aplicaste de verdad).

## Contenido

### Qué es
`dotnet-counters` es una herramienta de línea de comandos que muestra en tiempo real los contadores de rendimiento que publica un proceso .NET: uso de CPU, memoria, recolecciones del GC, tasa de asignación, estado del thread pool, excepciones, y métricas de ASP.NET Core y de tus propios `Meter`. Es la primera herramienta a la que recurres para saber "qué está pasando ahora" sin afectar casi al proceso. Forma parte del grupo [[dotnet-counters, dotnet-trace y dotnet-dump]].

### Cómo funciona
El runtime publica métricas a través de `EventCounters` y, en versiones recientes, de la API `System.Diagnostics.Metrics` (los mismos *meters* que exporta [[OpenTelemetry]]). `dotnet-counters` se conecta por el diagnostic port, se suscribe a los proveedores que le pidas y refresca la pantalla cada segundo (configurable con `--refresh-interval`).

```bash
dotnet tool install --global dotnet-counters
dotnet-counters ps                                  # encuentra el PID
dotnet-counters monitor -p 12345                    # contadores por defecto (System.Runtime)
dotnet-counters monitor -n Itzamna.Dionisio --counters System.Runtime,Microsoft.AspNetCore.Hosting
```

También puede lanzar la app y monitorearla desde el inicio: `dotnet-counters monitor -- dotnet run`.

### Contadores que indican problemas de GC y memoria
Los nombres exactos cambian según la versión: en .NET 9 y posteriores el proveedor `System.Runtime` usa nombres estilo OpenTelemetry (por ejemplo `dotnet.gc.collections`, `dotnet.gc.heap.total_allocated`, `dotnet.gc.pause.time`), mientras que en versiones anteriores aparecen como "% Time in GC since last GC", "GC Heap Size", "Gen 0 GC Count", "Allocation Rate". Lo importante es saber qué significan:

- **Tiempo en GC / tiempo de pausa**: porcentaje del tiempo que el runtime pasó recolectando. Valores sostenidos altos indican demasiadas allocations o un heap gen 2 enorme.
- **Tasa de asignación** (bytes por segundo): cuánta memoria pide tu código. Si se dispara bajo carga, busca allocations ocultas ([[Allocations y modos del GC]]).
- **Recolecciones por generación**: muchas de gen 2 son mala señal; las de gen 0 son normales.
- **Tamaño del heap y del LOH**: si sólo crece, sospecha una fuga (caché sin límite, eventos sin desuscribir).
- **Working set**: memoria física del proceso.

Otros útiles: **ThreadPool Queue Length** y **ThreadPool Thread Count** (starvation), **Monitor Lock Contention Count** (locks), **Exception Count**, y en ASP.NET Core `current-requests` y `requests-per-second`.

### Ejemplo: guardar datos para comparar
Antes y después de optimizar un endpoint de Wee.Reporteria, guarda los contadores en un archivo mientras corres una prueba de carga:

```bash
 # CSV fácil de abrir en Excel o Power BI
dotnet-counters collect -n Wee.Reporteria --format csv -o antes.csv --refresh-interval 1
 # ... aplica el cambio, despliega ...
dotnet-counters collect -n Wee.Reporteria --format csv -o despues.csv
```

`collect` admite `--format csv` o `json`. Con los dos archivos puedes graficar tasa de asignación y tiempo en GC lado a lado. Para tus propias métricas, crea un `Meter("Dionisio.Pedidos")` con un `Counter<long>` y monitorea con `--counters Dionisio.Pedidos`.

### Cuándo usarlo y cuándo no
Úsalo para el triage inicial: confirmar si el problema es CPU, GC, thread pool o espera externa, y para comparar antes y después de un cambio. No te dice *qué método* causa el problema: para eso usa [[dotnet-trace]]; ni qué objetos ocupan la memoria: para eso [[dotnet-dump]]. Para monitoreo permanente, exporta las mismas métricas con OpenTelemetry a un backend con alertas.

### Errores comunes
- **Mirar un solo instante**: los contadores fluctúan; observa tendencias durante la carga real.
- **Buscar nombres de contadores de otra versión**: si no aparece "% Time in GC", revisa con `dotnet-counters list` o la documentación de métricas integradas.
- **Monitorear en reposo**: sin tráfico no verás el problema; genera carga.
- **Ignorar el thread pool**: una CPU baja con latencia alta suele estar ahí.

### Para profundizar
- [dotnet-counters (Microsoft Learn)](https://learn.microsoft.com/dotnet/core/diagnostics/dotnet-counters)
- [Métricas integradas de .NET (Microsoft Learn)](https://learn.microsoft.com/dotnet/core/diagnostics/built-in-metrics-runtime)
- [Contadores de eventos conocidos (Microsoft Learn)](https://learn.microsoft.com/dotnet/core/diagnostics/available-counters)

## Autoevaluación

Contesta primero en voz alta o en papel; luego despliega la respuesta.

> [!question]- ¿Qué comando monitorea los contadores del runtime?
> `dotnet-counters monitor -n MiApi --counters System.Runtime,Microsoft.AspNetCore.Hosting`.

> [!question]- ¿Qué contadores indican problemas de GC?
> Tasa de asignación (*allocation rate*), número de recolecciones Gen0/1/2, tamaño del heap y % de tiempo en GC.

> [!question]- ¿Cómo guardas los datos para compararlos?
> `dotnet-counters collect -n MiApi --format csv -o metricas.csv`.

## Práctica

- [[Lab 5 - async-await, CancellationToken y sync over async]]

