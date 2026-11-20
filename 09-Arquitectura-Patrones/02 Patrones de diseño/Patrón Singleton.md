---
proyecto: Conocimiento
tipo: concepto
nivel: basico
progreso: pendiente
estado: pendiente
area: Arquitectura y patrones
tags: [arquitectura-patrones]
---
# Patrón Singleton

Parte de: [[09-Arquitectura-Patrones/02 Patrones de diseño/_Índice Patrones de diseño|Patrones de diseño]]

> [!info] Cómo avanzar
> `progreso`: **pendiente** → **viendo** (estudiando) → **visto** (contestaste la autoevaluación sin abrir las respuestas) → **dominado** (hiciste el laboratorio o lo aplicaste de verdad).

## Contenido

### Qué es
*Singleton* es un patrón creacional que garantiza que **una clase tenga una única instancia** durante la vida de la aplicación y ofrece un punto de acceso a ella. En su forma clásica la clase tiene constructor privado y una propiedad estática `Instance`. Sirve para recursos que deben ser únicos y compartidos: una caché en memoria, un cliente de conexión costoso, un registro de configuración.

### Cómo funciona
#### La forma clásica
```csharp
public sealed class RelojSistema
{
    private static readonly Lazy<RelojSistema> _instancia = new(() => new RelojSistema());
    public static RelojSistema Instance => _instancia.Value; // Lazy<T> es seguro entre hilos
    private RelojSistema() { }
    public DateTimeOffset Ahora => DateTimeOffset.UtcNow;
}
```

Funciona, pero tiene problemas: es **estado global**, oculta la dependencia (cualquier método puede usar `RelojSistema.Instance` sin declararlo) y es difícil de sustituir en pruebas.

#### Cómo se hace hoy en .NET
En aplicaciones con [[Inyección de dependencias]], **el contenedor garantiza la unicidad** y la clase es normal:

```csharp
builder.Services.AddSingleton(TimeProvider.System);          // reloj del sistema (.NET 8+)
builder.Services.AddSingleton<ICatalogoMonedas, CatalogoMonedas>();

public sealed class CerrarPedidoHandler(TimeProvider reloj) // la dependencia es visible
{
    public void Handle(Pedido p) => p.Cerrar(reloj.GetUtcNow());
}
```

En pruebas inyectas un `FakeTimeProvider` y controlas el tiempo. La unicidad es una decisión de **registro**, no de diseño de la clase.

Los tres ciclos de vida:
| Ciclo | Instancias | Ejemplo |
|---|---|---|
| Singleton | Una para toda la app | `TimeProvider`, `HybridCache`, clientes de Service Bus o Blob |
| Scoped | Una por petición HTTP | `IDbConnection`, `DbContext`, handlers con estado de la petición |
| Transient | Una cada vez que se pide | Objetos ligeros sin estado |

```mermaid
flowchart LR
    S["Singleton (vive toda la app)"] -->|"captura"| C["Scoped (debería morir al terminar la petición)"]
    C --> X["Queda vivo para siempre: dependencia cautiva"]
```

### Singleton que depende de un scoped
Si un singleton recibe en su constructor un servicio *scoped* (por ejemplo `DbContext` o la conexión de la petición), ocurre una **dependencia cautiva** (*captive dependency*): el singleton se crea una vez y guarda **la primera instancia scoped para siempre**. Consecuencias:
- Todas las peticiones comparten el mismo `DbContext`, que **no es seguro entre hilos**: errores aleatorios con concurrencia.
- El contexto acumula entidades rastreadas y crece en memoria; ve datos viejos.
- La conexión nunca se cierra o se usa después de cerrada.

En entorno Development, ASP.NET Core activa la **validación de scopes** y lanza una excepción al iniciar ("Cannot consume scoped service ... from singleton"). En producción esa validación no está activa por defecto, así que el error puede aparecer solo con carga. Solución: si el singleton necesita algo scoped, inyecta `IServiceScopeFactory` y crea un scope por operación:

```csharp
public sealed class ProcesadorOutbox(IServiceScopeFactory scopes) : BackgroundService
{
    protected override async Task ExecuteAsync(CancellationToken ct)
    {
        while (!ct.IsCancellationRequested)
        {
            using var scope = scopes.CreateScope();
            var publicador = scope.ServiceProvider.GetRequiredService<PublicarPendientes>();
            await publicador.EjecutarAsync(ct);
            await Task.Delay(TimeSpan.FromSeconds(5), ct);
        }
    }
}
```

### Estado mutable en un singleton
Un singleton lo usan **todas las peticiones a la vez**, en hilos distintos. Si guarda estado mutable sin protección (un `List<T>` o `Dictionary` donde se agregan cosas, un campo "usuario actual", un contador), tendrás condiciones de carrera: datos corruptos, excepciones intermitentes del tipo "la colección fue modificada" y, peor, **datos de un usuario visibles para otro**. Reglas:
- Prefiere singletons **sin estado** o con estado inmutable.
- Si necesitas estado compartido, usa tipos concurrentes (`ConcurrentDictionary`, `Interlocked`) o una caché diseñada para eso (`IMemoryCache`, `HybridCache`).
- Nunca guardes datos de la petición (usuario, tenant) en un singleton; eso es scoped.

### Cuándo usarlo y cuándo no
- **Úsalo (vía DI)** para servicios sin estado o thread-safe y costosos de crear: clientes de Azure SDK (`BlobServiceClient`, `ServiceBusClient`), cachés, catálogos de solo lectura.
- **Evita el Singleton clásico estático** en código de aplicación; solo tiene sentido en utilidades sin dependencias.

### Errores comunes
1. **Dependencias cautivas** (ver arriba).
2. **Estado mutable sin sincronización.**
3. **`static` como atajo** para "no inyectar": dependencias ocultas y pruebas imposibles.
4. **Registrar como singleton algo que no es thread-safe** (una `SqlConnection`).
5. **Crear un `HttpClient` nuevo por petición** en lugar de reutilizar vía `IHttpClientFactory`.

### Para profundizar
- [Singleton (Refactoring.Guru)](https://refactoring.guru/design-patterns/singleton)
- [Guía de inyección de dependencias en .NET (Microsoft Learn)](https://learn.microsoft.com/en-us/dotnet/core/extensions/dependency-injection-guidelines)
- [Inyección de dependencias en .NET, ciclos de vida (Microsoft Learn)](https://learn.microsoft.com/en-us/dotnet/core/extensions/dependency-injection)

## Autoevaluación

Contesta primero en voz alta o en papel; luego despliega la respuesta.

> [!question]- ¿Qué garantiza Singleton y cómo se hace hoy en .NET?
> Una sola instancia compartida. En apps con DI se registra con `AddSingleton<T>()`; el contenedor controla la vida en vez de un `static Instance`.

> [!question]- ¿Qué pasa si un singleton depende de un servicio *scoped* (p. ej. `DbContext`)?
> *Captive dependency*: el scoped queda atrapado para toda la vida de la app (datos viejos, problemas de concurrencia). En desarrollo, la validación de scopes de .NET lanza excepción. Solución: crear un scope con `IServiceScopeFactory`.

> [!question]- Error común: estado mutable en un singleton.
> Varias peticiones lo modifican a la vez → condiciones de carrera. Debe ser inmutable o *thread-safe*.

