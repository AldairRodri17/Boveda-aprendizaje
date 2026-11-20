---
proyecto: Conocimiento
tipo: concepto
nivel: intermedio
progreso: pendiente
estado: pendiente
area: SQL Server
tags: [sql-server]
---
# Redis más allá de la caché (locks, colas y contadores)

Parte de: [[06-SQL-Server/06 Otros datos/_Índice Otros datos|Otros datos]]

> [!info] Cómo avanzar
> `progreso`: **pendiente** → **viendo** (estudiando) → **visto** (contestaste la autoevaluación sin abrir las respuestas) → **dominado** (hiciste el laboratorio o lo aplicaste de verdad).

## Contenido

### Qué es
**Redis** es un almacén de estructuras de datos en memoria. Casi todos lo conocen como caché, pero sus estructuras (strings, hashes, listas, sets, sorted sets, streams) y sus **operaciones atómicas** lo hacen útil para coordinación entre varias instancias de una app: **locks distribuidos**, **colas**, **contadores** y **límites de tasa**. Como Redis ejecuta los comandos de forma secuencial en un solo hilo principal, cada comando es atómico sin que tú manejes concurrencia.

En Azure, el servicio recomendado es **Azure Managed Redis**: Azure Cache for Redis (niveles Basic, Standard y Premium) está en proceso de retiro, sin altas nuevas desde 2026 y con fecha de retiro el 30 de septiembre de 2028.

### Cómo funciona
| Necesidad | Estructura / comando | Por qué |
|---|---|---|
| Lock distribuido | `SET clave valor NX PX 30000` | Crea la clave solo si no existe, con expiración automática |
| Contador | `INCR`, `INCRBY`, `HINCRBY` | Incremento atómico, sin leer-modificar-escribir |
| Límite de tasa | `INCR` + `EXPIRE` por ventana, o sorted set con marcas de tiempo | Cuenta peticiones por usuario y periodo |
| Cola simple | Lista con `LPUSH` / `BRPOP` (o `LMOVE` a una lista de "en proceso") | FIFO con espera bloqueante |
| Cola confiable | **Streams**: `XADD`, `XREADGROUP`, `XACK`, `XAUTOCLAIM` | Grupos de consumidores, confirmación y reintento de pendientes |
| Ranking | Sorted set: `ZINCRBY`, `ZREVRANGE` | Ordenado por puntaje |

**Lock distribuido sencillo**:
1. Adquirir: `SET lock:conciliacion <token-único> NX PX 30000`. Si devuelve OK, tienes el lock; si no, otro proceso lo tiene.
2. El **token único** (un GUID por proceso) evita que borres el lock de otro: liberas con un script Lua que compara el valor antes de borrar.
3. La **expiración** evita un lock eterno si el proceso muere. Si el trabajo puede durar más, renueva el lock antes de que expire.
4. Este lock es "de buena fe": con un solo nodo y conmutación por error, dos procesos podrían tenerlo brevemente. Para exclusión estricta usa además un *fencing token* o un lock en la base de datos (`sp_getapplock`).

**Por qué Pub/Sub no sirve como cola confiable**: `PUBLISH` entrega el mensaje solo a los suscriptores **conectados en ese momento** y no lo guarda. Si tu worker está reiniciándose, el mensaje se pierde; no hay confirmación, reintento ni historial. Para trabajo que no debe perderse usa **Streams** con grupos de consumidores, o un broker como Azure Service Bus.

### Ejemplo
Con StackExchange.Redis en un worker .NET que concilia pagos:
```csharp
using StackExchange.Redis;

public sealed class LockRedis(IConnectionMultiplexer redis)
{
    private readonly IDatabase _db = redis.GetDatabase();

    public async Task<bool> EjecutarConLockAsync(string recurso, Func<Task> trabajo)
    {
        var clave = $"lock:{recurso}";
        var token = Guid.NewGuid().ToString("N");
        // SET NX PX: solo si no existe, expira en 30 s
        if (!await _db.StringSetAsync(clave, token, TimeSpan.FromSeconds(30), When.NotExists))
            return false;   // otra instancia lo tiene
        try { await trabajo(); return true; }
        finally
        {
            // Libera solo si el lock sigue siendo nuestro
            const string lua = "if redis.call('get', KEYS[1]) == ARGV[1] then return redis.call('del', KEYS[1]) else return 0 end";
            await _db.ScriptEvaluateAsync(lua, [clave], [token]);
        }
    }

    // Contador de intentos de login por IP en una ventana de 15 minutos
    public async Task<long> RegistrarIntentoAsync(string ip)
    {
        var clave = $"login:intentos:{ip}";
        var n = await _db.StringIncrementAsync(clave);
        if (n == 1) await _db.KeyExpireAsync(clave, TimeSpan.FromMinutes(15));
        return n;
    }
}
```
StackExchange.Redis también trae `LockTakeAsync` y `LockReleaseAsync`, que implementan el mismo patrón.

### Cuándo usarlo y cuándo no
- **Sí**: coordinar instancias de una API (evitar que dos ejecuten el mismo job), límites de tasa en Xolotl.Hecate, contadores de vistas, colas ligeras de trabajo derivado.
- **No** como fuente de verdad de datos de negocio (pagos, pedidos): guárdalos en SQL Server.
- **No** para mensajería crítica entre servicios si ya tienes Azure Service Bus o Storage Queues, que ofrecen durabilidad, reintentos y *dead-letter*.

### Errores comunes
1. **Lock sin expiración** o liberado sin comparar el token: bloqueos eternos o liberar el lock ajeno.
2. **Usar Pub/Sub como cola**: mensajes perdidos durante reinicios.
3. **`GET` + `SET` para incrementar**: condición de carrera; usa `INCR`.
4. **Persistencia ignorada**: Redis vive en memoria. Con persistencia **RDB** (instantáneas periódicas) puedes perder los últimos minutos; con **AOF** (registro de cada escritura) pierdes menos según la política de `fsync`, a cambio de rendimiento. En un reinicio sin persistencia se pierde todo. Diseña asumiendo que los datos de Redis pueden desaparecer.
5. **Política de expulsión** (`maxmemory-policy`) que borra claves de locks o colas cuando falta memoria; separa usos o usa `noeviction` donde importe.

### Para profundizar
- [Locks distribuidos con Redis (documentación oficial)](https://redis.io/docs/latest/develop/clients/patterns/distributed-locks/)
- [Redis Streams](https://redis.io/docs/latest/develop/data-types/streams/)
- [Persistencia en Redis](https://redis.io/docs/latest/operate/oss_and_stack/management/persistence/)
- [Azure Managed Redis](https://learn.microsoft.com/azure/redis/overview)

## Autoevaluación

Contesta primero en voz alta o en papel; luego despliega la respuesta.

> [!question]- ¿Cómo se implementa un lock distribuido sencillo con Redis?
> `SET lock:pedido:42 <token-unico> NX PX 30000`: solo lo obtiene quien lo crea primero y expira solo si el dueño muere. Para liberarlo, un script Lua que borra la llave **solo si** el valor es tu token (si no, podrías borrar el lock de otro).

> [!question]- ¿Qué estructuras usarías para colas y para contadores?
> Contadores: `INCR`/`INCRBY` (atómicos) con `EXPIRE` para ventanas de tiempo (rate limiting). Colas: listas (`LPUSH` + `BRPOP`) para lo simple, o **Streams** (`XADD`, `XREADGROUP`, `XACK`) cuando necesitas grupos de consumidores y reconocer mensajes.

> [!question]- ¿Por qué Pub/Sub de Redis no sirve como cola confiable?
> Es «dispara y olvida»: si un suscriptor no está conectado en ese momento, pierde el mensaje. Para no perder mensajes, Streams o un broker de mensajería.

> [!question]- ¿Qué cuidado hay con la persistencia de Redis?
> Por defecto es memoria con instantáneas (RDB) y opcionalmente log de operaciones (AOF); puede perder los últimos segundos en una caída. No guardes ahí datos que no puedas reconstruir; la fuente de verdad sigue siendo la base relacional.

