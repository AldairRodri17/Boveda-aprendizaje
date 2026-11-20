---
proyecto: Conocimiento
tipo: concepto
nivel: basico
progreso: pendiente
estado: pendiente
area: Fundamentos
tags: [fundamentos]
---
# GUID y UUID

Parte de: [[02-Fundamentos/06 Datos y formatos/_Índice Datos y formatos|Datos y formatos]]

> [!info] Cómo avanzar
> `progreso`: **pendiente** → **viendo** (estudiando) → **visto** (contestaste la autoevaluación sin abrir las respuestas) → **dominado** (hiciste el laboratorio o lo aplicaste de verdad).

## Contenido

### Qué es
Un **UUID** (Universally Unique Identifier, RFC 9562) es un identificador de **128 bits** pensado para generarse en cualquier lugar sin coordinación central y con probabilidad prácticamente nula de repetirse. Se escribe como 32 dígitos hexadecimales en 5 grupos: `550e8400-e29b-41d4-a716-446655440000`. **GUID** es el nombre que usa Microsoft para lo mismo (`System.Guid`, `uniqueidentifier` en SQL Server).

### Cómo funciona
El UUID tiene campos de **versión** (el primer dígito del tercer grupo) y **variante**. Las versiones más relevantes:

| Versión | Cómo se genera | Uso |
|---|---|---|
| v1 | Marca de tiempo + MAC de la máquina | Heredado; filtra la MAC |
| v4 | **122 bits aleatorios** | El más común: `Guid.NewGuid()`, `crypto.randomUUID()` |
| v5 | Hash SHA-1 de un namespace + nombre | Id determinístico a partir de un nombre |
| v7 | **Marca de tiempo Unix en milisegundos** (48 bits) + aleatoriedad | Ids **ordenables** por creación; recomendado para claves de base de datos |

En `550e8400-e29b-41d4-...`, el `4` de `41d4` indica versión 4. Un v7 se ve como `01934f4e-8c2a-7xxx-...`: los primeros dígitos crecen con el tiempo.

**.NET 9** agregó `Guid.CreateVersion7()` (y una sobrecarga que recibe un `DateTimeOffset`); `Guid.NewGuid()` sigue generando v4.

**El problema de GUID v4 como clave clusterizada en SQL Server.** Una tabla con clave primaria clusterizada guarda las filas **ordenadas** por esa clave. Con v4, cada inserción cae en una **posición aleatoria** del índice:
- **Divisiones de página** (page splits): la página destino está llena y SQL Server la parte en dos.
- **Fragmentación** y páginas medio vacías: más espacio, más lecturas, peor uso de caché.
- Más escritura al log de transacciones.

Con claves crecientes (`int IDENTITY`, `bigint`, secuencias), las inserciones van siempre **al final**. Alternativas:
1. `int`/`bigint` como clave clusterizada y el GUID como columna aparte con índice único (para exponer en APIs sin revelar el conteo).
2. `NEWSEQUENTIALID()` como valor por defecto en SQL Server: genera GUID crecientes en esa máquina (solo como `DEFAULT` de columna, y se reinicia la secuencia al reiniciar el servidor).
3. **UUID v7**: ordenados por tiempo. **Ojo**: SQL Server **no** compara `uniqueidentifier` byte a byte de izquierda a derecha; ordena empezando por los **últimos 6 bytes** (el último grupo). Un v7 generado en .NET, que es creciente al leerlo como texto, **no** queda en orden creciente en un índice de SQL Server. Verifica el orden real o reordena los bytes antes de guardarlo. En PostgreSQL (Pochteca.Nomisma), el tipo `uuid` sí compara byte a byte, y v7 funciona bien directamente.

**¿Sirve como token secreto?** No es lo ideal. Un v4 tiene 122 bits aleatorios, pero no todas las implementaciones garantizan un generador **criptográfico**; un v7 tiene una parte **predecible** (la hora); y un GUID se percibe como "identificador", así que acaba en logs y URLs. Para secretos (tokens de restablecimiento, API keys) usa `RandomNumberGenerator.GetBytes(32)` codificado en Base64URL (ver [[Base64]]).

### Ejemplo
Tabla de pedidos en Itzamna.Dionisio con clave interna `bigint` y id público GUID:

```sql
CREATE TABLE dbo.Pedido (
    PedidoId   bigint IDENTITY(1,1) NOT NULL CONSTRAINT PK_Pedido PRIMARY KEY CLUSTERED,
    PublicId   uniqueidentifier NOT NULL CONSTRAINT DF_Pedido_PublicId DEFAULT NEWSEQUENTIALID(),
    ClienteId  int NOT NULL,
    Total      decimal(19,4) NOT NULL,
    CreadoUtc  datetime2(3) NOT NULL CONSTRAINT DF_Pedido_CreadoUtc DEFAULT SYSUTCDATETIME(),
    CONSTRAINT UQ_Pedido_PublicId UNIQUE NONCLUSTERED (PublicId)
);
```

La API expone `PublicId` (`/api/pedidos/3f2a...`) y nunca el `bigint`, así nadie deduce cuántos pedidos tienes ni prueba ids consecutivos.

Generar v7 en .NET y comprobar la versión:

```csharp
var id = Guid.CreateVersion7();
Console.WriteLine(id);            // 0193....-....-7...-....-............
Console.WriteLine(id.Version);    // 7 (propiedad disponible desde .NET 9)
```

En el navegador: `crypto.randomUUID()` devuelve un v4 generado con el generador criptográfico del navegador.

### Cuándo usarlo y cuándo no
- **UUID**: ids generados en el cliente o en varios servicios sin coordinar (offline, sistemas distribuidos), ids públicos no adivinables por secuencia, llaves de idempotencia.
- **v7**: cuando el UUID será clave de índice en bases que comparan byte a byte (PostgreSQL) o cuando quieres ordenar por creación.
- **Enteros**: claves clusterizadas internas en SQL Server, relaciones y joins de alto volumen.
- **No**: como secreto o token de seguridad.

### Errores comunes
1. **GUID v4 como clave clusterizada** en tablas grandes. Fragmentación y page splits.
2. **Asumir que v7 queda ordenado en SQL Server.** Su orden de comparación de `uniqueidentifier` es distinto.
3. **Guardar GUID como `varchar(36)`.** Ocupa más del doble que `uniqueidentifier` (16 bytes) y compara más lento.
4. **Usar GUID como token secreto.**
5. **Mezclar formatos** (con llaves, sin guiones, mayúsculas) entre sistemas. Normaliza a minúsculas con guiones en APIs.

### Para profundizar
- [RFC 9562: Universally Unique IDentifiers (UUIDs)](https://www.rfc-editor.org/rfc/rfc9562)
- [Microsoft Learn: Guid.CreateVersion7](https://learn.microsoft.com/dotnet/api/system.guid.createversion7)
- [Microsoft Learn: NEWSEQUENTIALID (Transact-SQL)](https://learn.microsoft.com/sql/t-sql/functions/newsequentialid-transact-sql)

## Autoevaluación

Contesta primero en voz alta o en papel; luego despliega la respuesta.

> [!question]- ¿Qué es un UUID/GUID?
> Un identificador de 128 bits, escrito como 32 hexadecimales en 5 grupos (`550e8400-e29b-41d4-a716-446655440000`). GUID es el nombre de Microsoft para lo mismo.

> [!question]- ¿Qué diferencia hay entre UUID v4 y v7?
> **v4**: aleatorio. **v7**: empieza con una marca de tiempo en milisegundos y luego aleatoriedad, así que se ordena por creación y fragmenta menos los índices. .NET 9 agregó `Guid.CreateVersion7()`.

> [!question]- ¿Por qué un GUID v4 como clave clusterizada en SQL Server puede ser un problema?
> Las inserciones caen en posiciones aleatorias del índice: divisiones de página y fragmentación. Alternativas: `NEWSEQUENTIALID()`, UUID v7 (ojo: SQL Server ordena `uniqueidentifier` por los últimos bytes primero, así que hay que verificar el orden), o clave `int`/`bigint` con GUID como columna aparte.

> [!question]- ¿Un GUID sirve como token secreto?
> No es lo ideal: v4 tiene 122 bits aleatorios pero su generador no siempre está garantizado como criptográfico y v7 es parcialmente predecible. Para secretos usa `RandomNumberGenerator`.

