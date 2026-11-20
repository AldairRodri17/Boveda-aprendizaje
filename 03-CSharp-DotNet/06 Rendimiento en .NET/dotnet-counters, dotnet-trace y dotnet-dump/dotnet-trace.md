---
proyecto: Conocimiento
tipo: concepto
nivel: avanzado
progreso: pendiente
estado: pendiente
area: C# y .NET
tags: [csharp-dotnet]
---
# dotnet-trace

Parte de: [[dotnet-counters, dotnet-trace y dotnet-dump]]

> [!info] Cómo avanzar
> `progreso`: **pendiente** → **viendo** (estudiando) → **visto** (contestaste la autoevaluación sin abrir las respuestas) → **dominado** (hiciste el laboratorio o lo aplicaste de verdad).

## Contenido

### Qué es
`dotnet-trace` graba una **traza** de un proceso .NET: una secuencia de eventos del runtime (GC, JIT, excepciones, contención, peticiones HTTP) y muestras periódicas de las pilas de llamadas de todos los hilos. Con esa traza averiguas **en qué métodos se va el tiempo de CPU** o qué está esperando el proceso. Es la herramienta intermedia del grupo [[dotnet-counters, dotnet-trace y dotnet-dump]]: más detallada que los contadores y menos invasiva que un dump.

### Cómo funciona
Se conecta por EventPipe al proceso y activa proveedores de eventos según un **perfil**. El perfil de muestreo de CPU toma la pila de cada hilo con frecuencia (del orden de milisegundos); al acumular miles de muestras, los métodos que aparecen más son los que más tiempo consumen. El resultado se escribe en un archivo `.nettrace`.

```bash
dotnet tool install --global dotnet-trace
dotnet-trace ps
dotnet-trace list-profiles          # perfiles disponibles en tu versión
 # Graba hasta que presionas Enter o Ctrl+C, o por una duración fija (hh:mm:ss)
dotnet-trace collect -p 12345 --profile cpu-sampling --duration 00:00:30
 # Directo en formato speedscope para verlo en el navegador
dotnet-trace collect -p 12345 --format speedscope
 # Convertir uno existente
dotnet-trace convert trace.nettrace --format speedscope
```

Los nombres de perfil pueden cambiar entre versiones de la herramienta; revisa `list-profiles`. También puedes pedir proveedores concretos con `--providers`, por ejemplo `Microsoft-Windows-DotNETRuntime` con palabras clave de GC, o `Microsoft.AspNetCore.Hosting`.

### Con qué la abres
- **Visual Studio** (Windows): abre `.nettrace` directamente con vistas de CPU y eventos.
- **PerfView** (Windows, gratuito): el más completo para GC, allocations y pilas.
- **speedscope** (navegador, cualquier SO): sube el archivo convertido a `speedscope.app` y obtienes un flame graph interactivo.
- **TraceEvent**: biblioteca para analizar trazas por código.

### Qué buscas en un flame graph
En un flame graph cada barra es un método; el **ancho** es la proporción de muestras en que aparecía (tiempo), y las barras de arriba son las funciones llamadas por las de abajo. El eje horizontal no es el tiempo cronológico (en la vista "left heavy" se ordena por peso).

- Busca **mesetas anchas en la parte alta**: métodos que consumen CPU por sí mismos (*self time*). Ahí está el trabajo real: serialización, regex, LINQ pesado, hashing.
- Torres anchas de **frames del GC** (`gc_heap::...`) indican demasiadas allocations: combina con [[Allocations y modos del GC]].
- Mucho tiempo en **`Monitor.Enter`, `WaitHandle` o `Task.Wait`** indica bloqueo, no cálculo.
- Llamadas inesperadas: un `Regex` construido en cada petición, reflexión de un mapper, logging con formato costoso.

### Ejemplo
El endpoint de exportación de Wee.Reporteria usa 100 % de un núcleo. Corres `dotnet-trace collect -n Wee.Reporteria --format speedscope --duration 00:00:20` mientras generas el reporte. En speedscope ves que el 40 % del ancho está en `Regex..ctor` dentro de un método de limpieza de texto. Cambias a un `[GeneratedRegex]` estático (ver [[Source generators y GeneratedRegex]]), repites la traza y la barra desaparece.

### Cuándo usarlo y cuándo no
Úsalo cuando `dotnet-counters` muestra CPU alta o mucho tiempo en GC y necesitas saber dónde. También para analizar arranque lento (eventos del JIT) o contención. No es ideal para fugas de memoria (usa [[dotnet-dump]]) ni para monitoreo continuo (usa [[OpenTelemetry]]). En Linux, si necesitas también pilas nativas del kernel, existen `perf` y otras herramientas.

### Errores comunes
- **Trazas demasiado largas**: archivos enormes y difíciles de abrir. 20 a 60 segundos durante el problema suelen bastar.
- **Grabar sin carga**: el perfil saldrá vacío o dominado por el idle.
- **Confundir tiempo total con tiempo propio**: un método ancho en la base puede ser sólo el que llama a todo (por ejemplo, el middleware).
- **Activar proveedores muy verbosos en producción**: el overhead sube; usa perfiles acotados.

### Para profundizar
- [dotnet-trace (Microsoft Learn)](https://learn.microsoft.com/dotnet/core/diagnostics/dotnet-trace)
- [speedscope](https://www.speedscope.app/)
- [PerfView en GitHub](https://github.com/microsoft/perfview)

## Autoevaluación

Contesta primero en voz alta o en papel; luego despliega la respuesta.

> [!question]- ¿Cómo grabas una traza?
> `dotnet-trace collect -n MiApi --duration 00:00:30` produce un `.nettrace`.

> [!question]- ¿Con qué la abres?
> Visual Studio, PerfView o, con `--format speedscope`, en speedscope.app como *flame graph*.

> [!question]- ¿Qué buscas en un flame graph?
> Los métodos más anchos: ahí se va el tiempo de CPU. También pilas con mucho tiempo en GC o en serialización.

