---
proyecto: Conocimiento
tipo: concepto
nivel: basico
progreso: pendiente
estado: pendiente
area: SQL Server
tags: [sql-server]
contenido: completo
---
# Tipos de datos (varchar vs nvarchar, decimal vs float)

Parte de: [[06-SQL-Server/01 Básico/_Índice Básico|Básico]]

> [!info] Cómo avanzar
> `progreso`: **pendiente** → **viendo** (estudiando) → **visto** (contestaste la autoevaluación sin abrir las respuestas) → **dominado** (hiciste el laboratorio o lo aplicaste de verdad).

## Contenido

### Qué es
Cada columna, variable y parámetro tiene un **tipo de dato** que define qué valores admite, cuánto ocupa y cómo se compara. Elegir bien el tipo afecta la **corrección** (centavos que no cuadran, acentos que se pierden), el **almacenamiento** y el **rendimiento** (índices más pequeños, sin conversiones implícitas). Las dos decisiones más frecuentes y más malentendidas son texto (`varchar` vs `nvarchar`) y números con decimales (`decimal` vs `float`).

### Cómo funciona
**Texto**

| Tipo | Codificación | Tamaño |
|---|---|---|
| `varchar(n)` | Página de códigos de la collation (p. ej. Latin1), o UTF-8 con collations `_UTF8` (2019+) | `n` es en **bytes**, hasta 8,000 |
| `nvarchar(n)` | UTF-16 (Unicode) | `n` es en **pares de bytes**, hasta 4,000; casi siempre 1 par = 1 carácter |
| `varchar(max)` / `nvarchar(max)` | Igual que arriba | Hasta 2 GB |
| `char(n)` / `nchar(n)` | Longitud fija, se rellena con espacios | Útil para códigos de largo fijo |

- La **`n`** de `nvarchar` viene de *national*: es el tipo Unicode. En `nvarchar(50)` el 50 son unidades de 2 bytes; caracteres fuera del plano básico (muchos emojis) usan dos unidades.
- `N'Ana López'` ocupa 18 bytes (9 caracteres × 2) y `'Ana López'` en `varchar` Latin1 ocupa 9.
- Usa `nvarchar` para texto de personas (nombres, direcciones, comentarios) y `varchar` para códigos controlados en ASCII (RFC, SKU, estados como `'Enviado'`).
- `max` solo cuando de verdad puede pasar de 4,000/8,000; no se puede usar como clave de índice.

**Números**

- **Exactos**: `int` (±2.1 mil millones, 4 bytes), `bigint` (8 bytes), `smallint`, `tinyint` (0–255), `bit`, y **`decimal(p, s)`** (= `numeric`): `p` dígitos en total y `s` decimales, aritmética exacta en base 10.
- **Aproximados**: `float` y `real` siguen IEEE 754 en **binario**; 0.1 no tiene representación exacta en binario, así que `0.1 + 0.2` da 0.30000000000000004 y la comparación con 0.3 resulta **distinto**. Sirven para mediciones científicas o estadísticas, no para dinero.
- `money` existe, pero tiene redondeos sorpresivos en multiplicaciones y divisiones; `decimal(19,4)` o `decimal(12,2)` es más predecible.

**Otros**: `date`, `datetime2`, `datetimeoffset` (ver [[datetime2 y fechas en SQL Server]]); `uniqueidentifier` (ver [[uniqueidentifier]]); `rowversion` para concurrencia (ver [[Concurrencia optimista]]).

**Elección práctica**:

| Dato | Tipo |
|---|---|
| Precio | `decimal(12,2)` |
| Nombre con acentos | `nvarchar(100)` |
| Código postal mexicano (`01234`) | `char(5)` o `varchar(5)`, **no** `int` (perderías el cero) |
| Activo / inactivo | `bit NOT NULL` |
| Porcentaje con 2 decimales | `decimal(5,2)` |
| Contador que puede pasar de 3 mil millones | `bigint` |

### Ejemplo
```sql
SELECT CASE WHEN CAST(0.1 AS float) + CAST(0.2 AS float) = CAST(0.3 AS float)
            THEN 'igual' ELSE 'distinto' END            AS ConFloat,     -- distinto
       CASE WHEN CAST(0.1 AS decimal(4,2)) + CAST(0.2 AS decimal(4,2)) = 0.3
            THEN 'igual' ELSE 'distinto' END            AS ConDecimal;   -- igual

SELECT DATALENGTH(N'Ana López') AS BytesNvarchar,   -- 18
       DATALENGTH('Ana López')  AS BytesVarchar;    -- 9 (collation Latin1)
```

**El caso Dapper**: por defecto Dapper envía un `string` de C# como **`nvarchar(4000)`**. Si la columna es `varchar(20)` con índice, el motor compara `varchar` contra `nvarchar`; como `nvarchar` tiene mayor precedencia, convierte **la columna** (`CONVERT_IMPLICIT`) en cada fila. Con collations SQL (`SQL_Latin1_General_CP1_CI_AS`) eso suele impedir el *Index Seek* y provoca un escaneo; con collations Windows el motor a veces logra un seek por rango, pero con más costo. El plan muestra una advertencia de conversión implícita (ver [[Advertencias del plan (conversiones implícitas y spills)]]).

```csharp
// Opción 1: por parámetro
var p = new { Sku = new DbString { Value = sku, IsAnsi = true, Length = 20 } };
var producto = await conexion.QuerySingleOrDefaultAsync<ProductoDto>(sqlPorSku, p);

// Opción 2: global, si casi todas tus columnas de texto son varchar
SqlMapper.AddTypeMap(typeof(string), DbType.AnsiString);
```

Y al revés: si la columna es `nvarchar` y envías `varchar`, no hay problema de índice (se convierte el parámetro, no la columna).

### Cuándo usarlo y cuándo no
- **`decimal`** para dinero, cantidades fiscales y todo lo que deba cuadrar al centavo. En C#, `decimal`.
- **`float`** para coordenadas, mediciones físicas, resultados estadísticos o de ML.
- **`nvarchar`** por defecto en texto libre; **`varchar`** para códigos ASCII controlados o con collation UTF-8 si quieres ahorrar espacio en texto mayormente latino.
- Elige **el tipo más pequeño que cubra el crecimiento razonable**: un `bigint` donde basta `int` duplica el tamaño de la clave en todos los índices.

### Errores comunes
1. **`float` para precios**: sumas que no cuadran por centavos.
2. **`varchar` para nombres** con caracteres fuera de la página de códigos: aparecen `?`.
3. **Mezclar `varchar` y `nvarchar`** en JOIN o parámetros: conversiones implícitas y escaneos.
4. **Números como texto** (o al revés): códigos postales en `int` pierden ceros; importes en `varchar` no se suman.
5. **`nvarchar(max)` para todo**: no se puede indexar y el optimizador estima mal la memoria.

### Para profundizar
- [Tipos de datos (Transact-SQL)](https://learn.microsoft.com/sql/t-sql/data-types/data-types-transact-sql)
- [char y varchar](https://learn.microsoft.com/sql/t-sql/data-types/char-and-varchar-transact-sql)
- [decimal y numeric](https://learn.microsoft.com/sql/t-sql/data-types/decimal-and-numeric-transact-sql)
- [float y real](https://learn.microsoft.com/sql/t-sql/data-types/float-and-real-transact-sql)

## Ejercicios

Resuélvelos en papel o en la herramienta; la solución está plegada.

> [!question]- Elige el tipo de dato para: precio, nombre de cliente con acentos, código postal mexicano (`01234`), indicador activo/inactivo, porcentaje de descuento con 2 decimales y conteo de visitas que puede pasar de 3 mil millones.
> - Precio: `decimal(10,2)`
> - Nombre: `nvarchar(100)`
> - Código postal: `char(5)` (es texto: un `int` perdería el cero inicial)
> - Activo: `bit`
> - Descuento: `decimal(5,2)` (0.00–999.99)
> - Visitas: `bigint` (`int` llega hasta 2,147,483,647)

> [!question]- ¿Qué devuelve `SELECT CASE WHEN CAST(0.1 AS float) + CAST(0.2 AS float) = CAST(0.3 AS float) THEN 'igual' ELSE 'distinto' END;` y por qué?
> Devuelve `distinto`: con `float` la suma es 0.30000000000000004. Con `decimal` (`CAST(0.1 AS decimal(5,2)) + CAST(0.2 AS decimal(5,2))`) da `igual`.

> [!question]- ¿Cuántos bytes ocupa `N'Ana López'` y cuántos `'Ana López'` (`varchar` Latin1)? Usa `DATALENGTH` para comprobarlo.
> ```sql
> SELECT DATALENGTH(N'Ana López') AS bytes_nvarchar,  -- 18
>        DATALENGTH('Ana López')  AS bytes_varchar;   -- 9
> ```
> 9 caracteres: 18 bytes en `nvarchar`, 9 en `varchar` con intercalación Latin1.

## Autoevaluación

Contesta primero en voz alta o en papel; luego despliega la respuesta.

> [!question]- ¿Qué diferencia hay entre `varchar` y `nvarchar`?
> `nvarchar` guarda Unicode (UTF-16, 2 bytes por carácter en la mayoría de los casos) y acepta cualquier idioma y emojis; `varchar` usa la página de códigos de la intercalación (1 byte por carácter en Latin1), o UTF-8 si la intercalación termina en `_UTF8` (SQL Server 2019+). Para nombres con acentos y texto de usuarios, `nvarchar` es lo seguro.

> [!question]- ¿Por qué no debes usar `float` para precios?
> `float` es aproximado (binario IEEE 754): `0.1 + 0.2` no da exactamente `0.3`. Para dinero usa `decimal(p,s)`, que es exacto; p. ej. `decimal(10,2)` guarda hasta 99,999,999.99.

> [!question]- Caso Dapper: tu columna es `varchar(20)` con índice y pasas un `string` de C#. ¿Qué puede pasar?
> Dapper manda los `string` como `nvarchar(4000)` por defecto. Comparar `varchar` contra `nvarchar` obliga a convertir la columna (`CONVERT_IMPLICIT`) y puede convertir un *seek* en *scan*. Solución: `new DbString { Value = x, IsAnsi = true, Length = 20 }` o usar `nvarchar` en la columna.

> [!question]- ¿Qué significa la `n` en `nvarchar(50)` y qué es `nvarchar(max)`?
> `50` es la longitud en pares de bytes (unidades UTF-16), no bytes; un carácter fuera del plano básico (emoji) ocupa 2 unidades. `max` permite hasta 2 GB; úsalo solo cuando de verdad lo necesites (JSON, documentos), porque no puede ser clave de índice.

## Práctica

- [[Lab 1 - Base de práctica Tienda en SQL Server]]

