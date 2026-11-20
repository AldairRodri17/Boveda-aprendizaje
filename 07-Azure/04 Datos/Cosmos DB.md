---
proyecto: Conocimiento
tipo: concepto
nivel: intermedio
progreso: pendiente
estado: pendiente
area: Azure
tags: [azure]
---
# Cosmos DB

Parte de: [[07-Azure/04 Datos/_Índice Datos|Datos]]

> [!info] Cómo avanzar
> `progreso`: **pendiente** → **viendo** (estudiando) → **visto** (contestaste la autoevaluación sin abrir las respuestas) → **dominado** (hiciste el laboratorio o lo aplicaste de verdad).

## Contenido

### Qué es
Azure Cosmos DB es una base de datos **NoSQL administrada y distribuida globalmente**: replica tus datos en las regiones que elijas, responde con latencias de milisegundos de un dígito y escala horizontalmente particionando los datos. Su API nativa es **NoSQL** (documentos JSON consultados con un SQL propio), y también ofrece APIs compatibles con **MongoDB**, **Cassandra**, **Gremlin** (grafos) y **Table**, además de una opción para **PostgreSQL** distribuido. Eliges la API al crear la cuenta y no se cambia después.

### Cómo funciona
Jerarquía: **cuenta** → **base de datos** → **contenedor** → **ítems** (documentos JSON). Tres conceptos deciden si te va bien o mal:

**1. Partition key.** Cada contenedor tiene una ruta de partición (por ejemplo `/tenantId`). Los ítems con el mismo valor forman una *partición lógica* (máximo 20 GB), y Cosmos reparte las particiones lógicas entre *particiones físicas*. Una buena llave tiene **alta cardinalidad** y reparte **parejo** las lecturas y escrituras. Una mala llave, como la fecha de hoy o un `estatus` con tres valores, crea una partición **caliente** que recibe todo el tráfico y limita el escalado sin importar cuántas RU pagues. Además, las consultas que incluyen la llave van a una sola partición (baratas); las que no, se reparten a todas (*cross-partition*, más caras). Cambiar la llave implica crear otro contenedor y migrar datos, así que se decide al principio.

**2. Request Units (RU/s).** Es la moneda de rendimiento: cada operación cuesta RU según tamaño del documento, índices y complejidad. Leer un documento de 1 KB por id y partition key cuesta alrededor de 1 RU; una consulta con filtros cuesta más. Si en un segundo consumes más RU de las disponibles, recibes **HTTP 429** (*too many requests*) con un tiempo de espera sugerido; el SDK reintenta solo unas veces. Modos de capacidad: **provisionado manual**, **autoscale** (escala entre 10 % y 100 % de un máximo) y **serverless** (pagas por RU consumida, ideal para cargas esporádicas).

**3. Consistencia.** Cinco niveles, de más fuerte a más débil:

| Nivel | Qué garantiza |
|---|---|
| Strong | Siempre lees la última escritura confirmada |
| Bounded staleness | Atraso máximo acotado (por versiones o por tiempo) |
| **Session** (predeterminado) | Dentro de tu sesión lees lo que escribiste, en orden |
| Consistent prefix | Nunca ves escrituras fuera de orden, pero pueden faltar las últimas |
| Eventual | Sin garantía de orden; converge con el tiempo |

Más fuerte significa más latencia, más costo en RU de lectura y menor disponibilidad ante fallas regionales. Session cubre la mayoría de apps.

**Nivel gratuito** (verificado en Microsoft Learn): una cuenta por suscripción puede activar *free tier* al crearse y recibe **1,000 RU/s y 25 GB** gratis de por vida; no aplica a cuentas serverless.

### Ejemplo
Guardar eventos de auditoría multi-tenant de Ocelotl.Hermes, con partition key `/tenantId`:

```csharp
using Azure.Identity;
using Microsoft.Azure.Cosmos;

var cliente = new CosmosClient(
    "https://cosmos-hermes-dev.documents.azure.com:443/",
    new DefaultAzureCredential(),
    new CosmosClientOptions
    {
        UseSystemTextJsonSerializerWithOptions = new JsonSerializerOptions(JsonSerializerDefaults.Web)
    });

Container auditoria = cliente.GetContainer("hermes", "auditoria");

var evento = new EventoAuditoria(Guid.NewGuid().ToString(), "tenant-017", "LoginExitoso", DateTime.UtcNow);
ItemResponse<EventoAuditoria> r = await auditoria.CreateItemAsync(evento, new PartitionKey(evento.TenantId));
Console.WriteLine($"Costo: {r.RequestCharge} RU");

// Consulta dentro de UNA partición (barata)
var q = new QueryDefinition("SELECT * FROM c WHERE c.tipo = @tipo ORDER BY c.fecha DESC")
    .WithParameter("@tipo", "LoginFallido");
using var it = auditoria.GetItemQueryIterator<EventoAuditoria>(q,
    requestOptions: new QueryRequestOptions { PartitionKey = new PartitionKey("tenant-017") });
while (it.HasMoreResults)
    foreach (var e in await it.ReadNextAsync()) Console.WriteLine(e.Fecha);

public record EventoAuditoria(string Id, string TenantId, string Tipo, DateTime Fecha);
```

Con RBAC de datos de Cosmos (rol *Cosmos DB Built-in Data Contributor*) evitas usar la llave primaria.

### Cuándo usarlo y cuándo no
Úsalo cuando tienes alto volumen con acceso por llave, esquemas flexibles, necesitas distribución global o escrituras en varias regiones, o patrones como catálogos, perfiles, carritos, telemetría por dispositivo. Si tu modelo es relacional con muchos joins, transacciones entre entidades y reportes ad hoc (como Itzamna.Dionisio o Pochteca.Nomisma), quédate en SQL Server o Postgres. Si solo necesitas llave-valor barato, Table Storage puede bastar (ver [[Queue Storage y Table Storage]]).

### Errores comunes
- **Elegir la partition key sin pensar en las consultas**: después todo es *cross-partition* y caro.
- **Ignorar los 429** y subir RU a ciegas. Primero revisa qué consultas consumen más (el `RequestCharge` de cada respuesta) y el índice.
- **Indexar todo por defecto en documentos grandes**: la política de indexación predeterminada indexa todas las rutas; excluir las que nunca filtras baja el costo de escritura.
- **Crear un `CosmosClient` por petición**: debe ser singleton.
- **Pedir Strong "por si acaso"**: duplica el costo de lectura y no siempre es posible con varias regiones de escritura.

### Para profundizar
- [Introducción a Azure Cosmos DB](https://learn.microsoft.com/azure/cosmos-db/introduction)
- [Particionamiento y escalado horizontal](https://learn.microsoft.com/azure/cosmos-db/partitioning-overview)
- [Niveles de consistencia](https://learn.microsoft.com/azure/cosmos-db/consistency-levels)
- [Nivel gratuito de Cosmos DB](https://learn.microsoft.com/azure/cosmos-db/free-tier)

## Autoevaluación

Contesta primero en voz alta o en papel; luego despliega la respuesta.

> [!question]- ¿Qué es Cosmos DB y qué APIs ofrece?
> Base NoSQL distribuida globalmente con baja latencia. APIs: **NoSQL** (nativa, documentos JSON), MongoDB, Cassandra, Gremlin, Table, y PostgreSQL (distribuido).

> [!question]- ¿Qué es la *partition key* y por qué importa tanto?
> Define cómo se reparten los datos y la carga entre particiones físicas. Una mala elección (poca cardinalidad o «caliente», p. ej. la fecha de hoy) concentra tráfico y limita el escalado. Es difícil de cambiar después.

> [!question]- ¿Qué son las RU/s?
> *Request Units*: la moneda de rendimiento. Cada operación consume RU según tamaño e índices; si superas las provisionadas, recibes **429** y debes reintentar o subir capacidad (manual, autoscale o serverless).

> [!question]- ¿Cuáles son los 5 niveles de consistencia?
> Strong, Bounded staleness, **Session** (por defecto: lees lo que escribiste en tu sesión), Consistent prefix y Eventual. Más fuerte = más latencia/costo y menos disponibilidad ante fallas.

