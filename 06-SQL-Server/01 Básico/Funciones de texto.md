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
# Funciones de texto

Parte de: [[06-SQL-Server/01 Básico/_Índice Básico|Básico]]

> [!info] Cómo avanzar
> `progreso`: **pendiente** → **viendo** (estudiando) → **visto** (contestaste la autoevaluación sin abrir las respuestas) → **dominado** (hiciste el laboratorio o lo aplicaste de verdad).

## Contenido

### Qué es
Las **funciones de texto** (o de cadena) manipulan valores `char`, `varchar`, `nchar` y `nvarchar`: extraer partes, buscar, reemplazar, limpiar espacios, cambiar mayúsculas y concatenar. En una base bien diseñada se usan sobre todo para **presentación, limpieza de cargas y generación de códigos**; la lógica pesada de texto suele estar mejor en C#.

### Cómo funciona
| Necesidad | Funciones |
|---|---|
| Longitud | `LEN` (caracteres, sin espacios finales), `DATALENGTH` (bytes) |
| Extraer | `LEFT`, `RIGHT`, `SUBSTRING(txt, inicio, largo)` |
| Buscar | `CHARINDEX(buscado, txt [, inicio])`, `PATINDEX('%patrón%', txt)` |
| Cambiar | `REPLACE`, `STUFF`, `TRANSLATE`, `REPLICATE`, `REVERSE` |
| Mayúsculas | `UPPER`, `LOWER` |
| Espacios | `LTRIM`, `RTRIM`, `TRIM` |
| Concatenar | `+`, `CONCAT`, `CONCAT_WS`, `STRING_AGG` |
| Formato | `FORMAT`, `STR`, `CONVERT` |

Detalles clave:

- **`LEN` vs `DATALENGTH`**: `LEN(N'Ana ')` es 3 (ignora espacios finales) y `DATALENGTH(N'Ana ')` es 8, porque `nvarchar` usa 2 bytes por carácter (UTF-16). En `varchar` con collation Latin1, un carácter = 1 byte. `DATALENGTH` sirve para medir almacenamiento real.
- **Índices base 1**: `SUBSTRING(txt, 1, 3)` son los primeros tres caracteres. `CHARINDEX` devuelve 0 si no encuentra.
- **`NULL` en la concatenación**: con `+`, cualquier `NULL` vuelve todo el resultado `NULL` (`N'Ana ' + NULL` = `NULL`). `CONCAT` trata los `NULL` como cadena vacía. `CONCAT_WS(separador, ...)` además **omite** los `NULL` sin dejar separadores dobles.
- **Novedades de SQL Server 2017** que ahorran trucos: `TRIM` (antes `LTRIM(RTRIM(x))`), `CONCAT_WS`, `TRANSLATE` (varios reemplazos de un carácter por otro en una llamada) y `STRING_AGG` (antes `FOR XML PATH`). En 2022, `LTRIM`/`RTRIM`/`TRIM` aceptan los caracteres a quitar.
- Comparaciones y `REPLACE` respetan la **collation** (mayúsculas y acentos); ver [[Collation]].
- Si la cadena resultante pasa de 8,000 bytes y ninguna entrada es `max`, se trunca; convierte una entrada a `nvarchar(max)` para concatenaciones largas.

### Ejemplo
```sql
-- Separar nombre y apellido en "Nombre Apellido"
SELECT Nombre,
       LEFT(Nombre, CHARINDEX(N' ', Nombre + N' ') - 1)             AS NombrePila,
       LTRIM(SUBSTRING(Nombre, CHARINDEX(N' ', Nombre + N' ') + 1, 100)) AS Apellido
FROM   ventas.Clientes;
-- El truco "Nombre + ' '" evita que CHARINDEX devuelva 0 si no hay espacio

-- Concatenación con NULL: + vs CONCAT vs CONCAT_WS
SELECT Nombre + N' (' + Ciudad + N')'          AS ConMas,      -- NULL si Ciudad es NULL
       CONCAT(Nombre, N' (', Ciudad, N')')     AS ConConcat,   -- "Elena Soto ()"
       CONCAT_WS(N' - ', Nombre, Ciudad)       AS ConWs        -- "Elena Soto"
FROM   ventas.Clientes;

-- Folio con ceros a la izquierda: FAC-000042
SELECT CONCAT('FAC-', RIGHT(REPLICATE('0', 6) + CAST(42 AS varchar(10)), 6)) AS Folio;

-- Limpieza de una carga de Excel: espacios, guiones y paréntesis en teléfonos
SELECT TRIM(Telefono)                                       AS SinEspacios,
       REPLACE(TRANSLATE(Telefono, '()-', '   '), ' ', '')  AS SoloDigitos
FROM   (VALUES (' (33) 1234-5678 ')) AS t(Telefono);

-- Bytes vs caracteres
SELECT LEN(N'Ana López') AS Caracteres, DATALENGTH(N'Ana López') AS BytesN,
       DATALENGTH('Ana López') AS BytesVarchar;   -- 9, 18, 9
```

Cuando la API recibe texto para buscar, pásalo como parámetro y deja que el SQL arme el patrón; así no hay inyección:

```csharp
var clientes = await conexion.QueryAsync<ClienteDto>(
    "SELECT ClienteId, Nombre FROM ventas.Clientes WHERE Nombre LIKE @Patron + N'%';",
    new { Patron = texto.Trim() });
```

### Cuándo usarlo y cuándo no
- **Úsalas** en ETL y limpieza de datos, para generar códigos o folios, y para preparar texto de reportes.
- **Evítalas sobre columnas en el `WHERE`** (`WHERE UPPER(Nombre) = ...`, `WHERE LEFT(Rfc, 4) = ...`): impiden el uso de índices. Si una expresión se filtra mucho, crea una columna calculada persistida e indexada.
- **No reemplaces una tabla con un texto separado por comas**: si necesitas `SUBSTRING` y `CHARINDEX` para leer una lista guardada en una columna, el diseño viola la [[Primera forma normal (1FN)]].
- El formato regional (moneda, fechas) es responsabilidad del front (React `Intl`) o de .NET, no de la base.

### Errores comunes
1. **Concatenar con `+` sin prever `NULL`**: filas completas en `NULL`. Usa `CONCAT` o `CONCAT_WS`.
2. **Confundir `LEN` y `DATALENGTH`** al validar tamaños de columna `nvarchar`.
3. **`CHARINDEX` que devuelve 0** y produce un largo negativo en `LEFT`/`SUBSTRING` (error 537).
4. **Truncamiento silencioso** al asignar a variables sin tamaño (`DECLARE @x varchar` es `varchar(1)`). Declara siempre la longitud.
5. **Usar `FORMAT` en consultas masivas**: lento; prefiere `CONVERT` con estilo o formatea fuera.

### Para profundizar
- [Funciones de cadena (Transact-SQL)](https://learn.microsoft.com/sql/t-sql/functions/string-functions-transact-sql)
- [CONCAT_WS (Transact-SQL)](https://learn.microsoft.com/sql/t-sql/functions/concat-ws-transact-sql)
- [TRANSLATE (Transact-SQL)](https://learn.microsoft.com/sql/t-sql/functions/translate-transact-sql)

## Ejercicios

Resuélvelos en papel o en la herramienta; la solución está plegada.

> [!question]- Con la base Tienda ([[Lab 1 - Base de práctica Tienda en SQL Server]]): separa el nombre de cada cliente en `NombrePila` (en mayúsculas) y `Apellido`.
> ```sql
> SELECT UPPER(LEFT(Nombre, CHARINDEX(N' ', Nombre) - 1))    AS NombrePila,
>        SUBSTRING(Nombre, CHARINDEX(N' ', Nombre) + 1, 100)   AS Apellido,
>        LEN(Nombre) AS Largo
> FROM   ventas.Clientes;
> ```
> ANA / López / 9, BRUNO / Díaz / 10, CARLA / Méndez / 12, …

> [!question]- Arma el texto `López, Ana` (apellido, nombre) para cada cliente.
> ```sql
> SELECT CONCAT(SUBSTRING(Nombre, CHARINDEX(N' ', Nombre) + 1, 100), N', ',
>               LEFT(Nombre, CHARINDEX(N' ', Nombre) - 1)) AS Formal
> FROM   ventas.Clientes;
> ```

> [!question]- Genera un código de producto con la forma `PER-0001` (3 primeras letras de la categoría en mayúsculas + id con ceros a la izquierda).
> ```sql
> SELECT CONCAT(UPPER(LEFT(Categoria, 3)), N'-', RIGHT(CONCAT('0000', ProductoId), 4)) AS Codigo, Nombre
> FROM   ventas.Productos;
> ```
> `PER-0001` (Teclado), `MON-0003`, `COM-0004`, `ACC-0005`… Evita `FORMAT` en consultas grandes: es mucho más lenta.

## Autoevaluación

Contesta primero en voz alta o en papel; luego despliega la respuesta.

> [!question]- ¿Qué diferencia hay entre `LEN` y `DATALENGTH`?
> `LEN` cuenta caracteres **sin** los espacios finales; `DATALENGTH` cuenta bytes (incluye espacios finales). `LEN(N'abc  ')` = 3, `DATALENGTH(N'abc  ')` = 10.

> [!question]- ¿Qué pasa al concatenar con `+` cuando un valor es `NULL`? ¿Y con `CONCAT`?
> `'Hola ' + NULL` da `NULL`. `CONCAT('Hola ', NULL)` trata el `NULL` como cadena vacía y da `'Hola '`. `CONCAT_WS(', ', a, b)` además pone separador y omite los `NULL` (SQL Server 2017+).

> [!question]- ¿Qué funciones de texto llegaron en SQL Server 2017 y te ahorran trucos?
> `TRIM`, `CONCAT_WS`, `TRANSLATE` y `STRING_AGG`. Antes se usaban `LTRIM(RTRIM())`, varios `REPLACE` y `FOR XML PATH` para concatenar.

