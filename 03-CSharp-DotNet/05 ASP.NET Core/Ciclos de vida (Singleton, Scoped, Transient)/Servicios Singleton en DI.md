---
proyecto: Conocimiento
tipo: concepto
nivel: intermedio
progreso: pendiente
estado: pendiente
area: C# y .NET
tags: [csharp-dotnet]
---
# Servicios Singleton en DI

Parte de: [[Ciclos de vida (Singleton, Scoped, Transient)]]

> [!info] Cómo avanzar
> `progreso`: **pendiente** → **viendo** (estudiando) → **visto** (contestaste la autoevaluación sin abrir las respuestas) → **dominado** (hiciste el laboratorio o lo aplicaste de verdad).

## Contenido

### Qué es
Un servicio **Singleton** tiene una sola instancia en toda la vida de la aplicación. El contenedor la crea la primera vez que alguien la pide (o usa la que registraste ya construida) y todas las peticiones, de todos los usuarios y en todos los hilos, usan ese mismo objeto. Es parte de [[Ciclos de vida (Singleton, Scoped, Transient)]].

Es el ciclo de vida más eficiente (no hay costo de creación por petición) y el más peligroso si el objeto guarda estado.

### Cómo funciona
Formas de registrar:

```csharp
builder.Services.AddSingleton<IReloj, RelojSistema>();          // el contenedor la crea y la libera
builder.Services.AddSingleton(TimeProvider.System);              // instancia existente: el contenedor NO la libera
builder.Services.AddSingleton<ICatalogo>(sp => new Catalogo(sp.GetRequiredService<ILogger<Catalogo>>()));
```

**Requisito principal: seguridad entre hilos.** Kestrel atiende muchas peticiones en paralelo en el ThreadPool. Si un singleton tiene un `List<T>` o un `Dictionary` que se modifica, dos peticiones pueden escribir a la vez y corromperlo. Opciones:
- No tener estado mutable (lo ideal: solo configuración y dependencias de solo lectura).
- Usar estructuras concurrentes (`ConcurrentDictionary`, `ImmutableArray`, `Interlocked`).
- Proteger con `lock` o `SemaphoreSlim` (para async) secciones cortas.

**Quién lo libera.** Si el contenedor creó el singleton y este implementa `IDisposable` o `IAsyncDisposable`, el contenedor lo libera al apagar la app (cuando se libera el proveedor raíz). Si tú pasaste una instancia ya creada con `AddSingleton(obj)`, el contenedor **no** la libera: tú la creaste, tú la liberas.

**Dependencias**: un singleton solo debe depender de otros singletons. Si recibe un Scoped lo captura ([[Captive dependency]]); si recibe un Transient, ese transient vive tanto como el singleton.

### Ejemplo
Buenos singletons en tus APIs:
- `IHttpClientFactory` y los typed clients registrados por él (la fábrica administra los handlers).
- `TimeProvider.System` (y en pruebas un `FakeTimeProvider`).
- Cachés: `IMemoryCache`, `HybridCache`.
- Opciones: `IOptions<T>` y `IOptionsMonitor<T>` ([[Options pattern]]).
- Las **sentencias SQL sin estado** de [[SQL en archivos con una interfaz por consulta]].
- Un `JsonSerializerOptions` compartido (crearlo por petición es caro y pierde la caché de metadatos).

Sentencia SQL como singleton, segura porque solo lee:

```csharp
public interface IListarEgresosSql
{
    string Texto { get; }
}

public sealed class ListarEgresosSql : IListarEgresosSql
{
    // Se lee una vez del recurso embebido; después solo se consulta
    public string Texto { get; } = RecursosSql.Leer("Egresos.Listar.sql");
}

builder.Services.AddSingleton<IListarEgresosSql, ListarEgresosSql>();
```

Ejemplo de **mal** singleton:

```csharp
public sealed class ContadorMalo
{
    private readonly Dictionary<string, int> _porUsuario = new(); // no es seguro entre hilos
    public void Sumar(string u) => _porUsuario[u] = _porUsuario.GetValueOrDefault(u) + 1;
}
// Corrección: ConcurrentDictionary<string,int> y AddOrUpdate, o moverlo a una caché/BD.
```

Si el singleton necesita algo Scoped de vez en cuando (por ejemplo, abrir una sesión SQL en una tarea programada), inyecta `IServiceScopeFactory` y crea un scope en cada uso; nunca guardes el Scoped en un campo.

### Cuándo usarlo y cuándo no
- **Sí** para objetos caros de crear, sin estado o con estado inmutable o concurrente.
- **Sí** para clientes que administran su propio pool (fábricas, clientes de Azure SDK como `BlobServiceClient`, que están diseñados para reutilizarse).
- **No** para nada que dependa del usuario o de la petición actual (usuario, tenant, transacción).
- **No** para conexiones SQL: el pool de ADO.NET ya reutiliza conexiones; la conexión debe ser por petición ([[Servicios Scoped en DI]]).

### Errores comunes
- **Estado mutable sin protección**: errores intermitentes, imposibles de reproducir en local.
- **Capturar un Scoped**: datos de un usuario visibles para otro.
- **Esperar que `AddSingleton(obj)` lo libere**: no lo hace.
- **Registrar un `DbContext` o `SqlConnection` como singleton**: no son seguros entre hilos.
- **Leer configuración una vez en el constructor** y esperar que cambie: usa `IOptionsMonitor<T>` si debe recargarse.

### Para profundizar
- [Ciclo de vida Singleton (Microsoft Learn)](https://learn.microsoft.com/dotnet/core/extensions/dependency-injection#singleton)
- [Guía de DI: liberación de servicios (Microsoft Learn)](https://learn.microsoft.com/dotnet/core/extensions/dependency-injection-guidelines)
- [[Servicios Transient en DI]]

## Autoevaluación

Contesta primero en voz alta o en papel; luego despliega la respuesta.

> [!question]- ¿Qué requisito tiene un singleton?
> Ser seguro entre hilos: lo usan todas las peticiones a la vez. Nada de estado mutable sin protección.

> [!question]- Ejemplos buenos de singleton
> `HttpClient` vía factory, `TimeProvider.System`, caches, opciones, sentencias SQL sin estado, `JsonSerializerOptions`.

> [!question]- ¿Quién libera un singleton `IDisposable`?
> El contenedor, al apagar la app; solo si él lo creó (no si registraste una instancia ya creada con `AddSingleton(obj)`).

