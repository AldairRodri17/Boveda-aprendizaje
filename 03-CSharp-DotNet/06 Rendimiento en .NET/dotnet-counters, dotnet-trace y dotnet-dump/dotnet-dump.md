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
# dotnet-dump

Parte de: [[dotnet-counters, dotnet-trace y dotnet-dump]]

> [!info] Cómo avanzar
> `progreso`: **pendiente** → **viendo** (estudiando) → **visto** (contestaste la autoevaluación sin abrir las respuestas) → **dominado** (hiciste el laboratorio o lo aplicaste de verdad).

## Contenido

### Qué es
`dotnet-dump` captura un **volcado de memoria** (*dump*) de un proceso .NET y permite analizarlo sin un depurador nativo. Un dump es una "foto" de toda la memoria del proceso en un instante: objetos del heap, hilos y sus pilas, excepciones. Sirve para investigar fugas de memoria, bloqueos (*deadlocks*), *thread pool starvation* y fallos que no puedes reproducir en tu PC. Es la herramienta más pesada del grupo [[dotnet-counters, dotnet-trace y dotnet-dump]].

### Cómo funciona
Al pedir el dump, el runtime suspende el proceso, escribe su memoria en un archivo y lo reanuda. Luego `dotnet-dump analyze` abre una consola interactiva que carga **SOS**, la extensión de depuración que entiende las estructuras del CLR (tipos, objetos administrados, pilas de .NET).

```bash
dotnet tool install --global dotnet-dump
dotnet-dump ps
 # Tipos: Mini, Heap (incluye heap administrado), Triage, Full (toda la memoria)
dotnet-dump collect -p 12345 --type Heap -o dionisio.dmp
dotnet-dump analyze dionisio.dmp
```

### Comandos SOS que más usarás
Dentro de `analyze`:

- `clrthreads` (o `threads`): lista hilos administrados y si tienen excepción.
- `clrstack` / `clrstack -all`: pila administrada de un hilo o de todos. Con `setthread <n>` cambias de hilo. Ideal para ver dónde están bloqueados.
- `dumpheap -stat`: cuántos objetos hay de cada tipo y cuánto ocupan, ordenado por tamaño. Es el punto de partida para fugas.
- `dumpheap -type Itzamna.Dionisio.Pedido`: lista instancias de un tipo con sus direcciones.
- `gcroot <dirección>`: muestra **quién mantiene vivo** un objeto (la cadena de referencias hasta una raíz: estático, hilo, handle). Responde "¿por qué no se recolecta?".
- `dumpobj <dirección>` (`do`): campos de un objeto.
- `printexception` (`pe`): detalle de la excepción actual o de una dirección.
- `dumpasync`: estado de las máquinas de estado `async` pendientes; útil cuando "todo espera".
- `syncblk`: locks retenidos y quién los tiene.

### Ejemplo: fuga de memoria
Itzamna.Clio llama a la API de Dionisio y la memoria del servidor sube de 300 MB a 2 GB en un día.

1. Tomas `dumpA` al inicio y `dumpB` horas después.
2. En ambos corres `dumpheap -stat` y comparas: `ReporteCacheItem` pasó de 200 a 900,000 instancias.
3. Tomas una dirección con `dumpheap -type ReporteCacheItem` y corres `gcroot`: la cadena termina en un `static Dictionary` de una clase de utilidades que nunca expira entradas.
4. Corriges usando `IMemoryCache` con expiración y tamaño máximo (ver [[IMemoryCache]]).

Para un bloqueo: `clrstack -all` muestra decenas de hilos detenidos en `Task.Wait` dentro de un método que hace `.Result` sobre una llamada HTTP; es sync over async.

### Precauciones en producción
- **Congela el proceso** mientras se escribe el archivo: segundos o más según la memoria. En una API con tráfico, saca la instancia del balanceador o hazlo en una réplica.
- **El archivo pesa casi lo mismo que la memoria del proceso** (en `Full` o `Heap`); asegura espacio en disco.
- **Contiene datos sensibles**: cadenas de conexión, tokens JWT, datos personales que estaban en memoria. Trátalo como secreto: no lo subas a repos ni lo compartas por chat, cífralo o bórralo al terminar.
- Debe analizarse en el mismo SO (un dump de Linux se analiza en Linux o con herramientas compatibles).

También puedes configurar que el runtime genere un dump automáticamente al caerse el proceso con las variables `DOTNET_DbgEnableMiniDump=1` y `DOTNET_DbgMiniDumpType`.

### Cuándo usarlo y cuándo no
Úsalo cuando necesitas ver el estado interno: fugas, deadlocks, starvation o caídas. No lo uses como primera herramienta para lentitud general (empieza con [[dotnet-counters]]) ni para medir tiempos de CPU (usa [[dotnet-trace]]). Visual Studio y WinDbg también abren estos dumps con vistas más gráficas.

### Errores comunes
- **Un solo dump para una fuga**: sin comparar dos momentos no sabes qué crece.
- **Usar `Mini`** cuando necesitas objetos: no incluye el heap completo.
- **Olvidar el riesgo de datos**: un dump copiado a tu laptop es una fuga de información.
- **Interpretar `dumpheap -stat` solo por cantidad**: mira también el tamaño total; un único `byte[]` gigante puede ser el culpable.

### Para profundizar
- [dotnet-dump (Microsoft Learn)](https://learn.microsoft.com/dotnet/core/diagnostics/dotnet-dump)
- [Extensión de depuración SOS (Microsoft Learn)](https://learn.microsoft.com/dotnet/core/diagnostics/sos-debugging-extension)
- [Depurar una fuga de memoria (Microsoft Learn)](https://learn.microsoft.com/dotnet/core/diagnostics/debug-memory-leak)

## Autoevaluación

Contesta primero en voz alta o en papel; luego despliega la respuesta.

> [!question]- ¿Cómo capturas y analizas un dump?
> `dotnet-dump collect -p <PID>` y luego `dotnet-dump analyze archivo.dmp`.

> [!question]- ¿Qué comandos SOS usas?
> `clrthreads` (hilos), `clrstack -all` / `parallelstacks` (pilas), `dumpheap -stat` (qué tipos ocupan memoria) y `gcroot <dirección>` (quién retiene un objeto).

> [!question]- ¿Qué precaución tiene en producción?
> El dump contiene toda la memoria: datos personales, tokens y cadenas de conexión. Trátalo como información sensible.

