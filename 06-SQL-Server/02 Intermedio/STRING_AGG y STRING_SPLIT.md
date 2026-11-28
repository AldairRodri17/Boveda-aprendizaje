---
proyecto: Conocimiento
tipo: concepto
nivel: intermedio
progreso: pendiente
estado: pendiente
area: SQL Server
tags: [sql-server]
contenido: completo
---
# STRING_AGG y STRING_SPLIT

Parte de: [[06-SQL-Server/02 Intermedio/_Índice Intermedio|Intermedio]]

> [!info] Cómo avanzar
> `progreso`: **pendiente** → **viendo** (estudiando) → **visto** (contestaste la autoevaluación sin abrir las respuestas) → **dominado** (hiciste el laboratorio o lo aplicaste de verdad).

## Contenido

### Qué es
Son dos funciones de texto complementarias:

- `STRING_AGG(expresión, separador)` es una **función de agregación** que concatena los valores de un grupo en un solo texto: los productos de un pedido como "Laptop 14, Mouse, Teclado". Existe desde SQL Server 2017.
- `STRING_SPLIT(texto, separador)` es una **función de tabla** que hace lo contrario: parte un texto en filas, una por cada elemento. Existe desde SQL Server 2016 y requiere nivel de compatibilidad 130 o más (ver [[Nivel de compatibilidad de la base]]).

### Cómo funciona
**`STRING_AGG`** se usa como `SUM` o `COUNT`: con `GROUP BY` agrega por grupo, sin él agrega todo. Para controlar el orden de los elementos dentro del texto se agrega `WITHIN GROUP (ORDER BY ...)`; sin esa cláusula el orden es el que salga del plan, no necesariamente alfabético. Ignora los `NULL` (no deja separadores dobles).

El tipo del resultado sigue al de la entrada: si concatenas `nvarchar(100)`, el resultado es `nvarchar(4000)` como máximo (8,000 bytes). Si la concatenación supera ese límite, la consulta **falla** con un error que indica que el resultado de `STRING_AGG` excedió los 8,000 bytes. La solución es convertir la entrada a `nvarchar(max)`: `STRING_AGG(CAST(Nombre AS nvarchar(max)), N', ')`.

**`STRING_SPLIT`** devuelve una columna llamada `value`. El separador debe ser **un solo carácter**. El **orden de las filas no está garantizado**: normalmente coincide con el del texto, pero la documentación no lo promete. Desde SQL Server 2022 (y en Azure SQL) existe un tercer argumento, `enable_ordinal`: con `1` devuelve también una columna `ordinal` con la posición de cada elemento, que sí puedes usar para ordenar. Los elementos vacíos (`'1,,3'`) producen filas con texto vacío, y los espacios no se recortan.

```mermaid
flowchart LR
    A["Filas: Laptop, Mouse, Teclado"] -->|"STRING_AGG"| B["Laptop, Mouse, Teclado"]
    B -->|"STRING_SPLIT"| A
```

Antes de 2017, la concatenación se hacía con el truco `STUFF((SELECT ', ' + Nombre ... FOR XML PATH('')), 1, 2, '')`, que verás en código viejo (ver [[XML en SQL Server]]).

### Ejemplo
Con la base de [[Lab 1 - Base de práctica Tienda en SQL Server]]:

```sql
-- Cada pedido con sus productos en orden alfabético
SELECT d.PedidoId,
       STRING_AGG(pr.Nombre, N', ') WITHIN GROUP (ORDER BY pr.Nombre) AS Productos
FROM   ventas.PedidoDetalle d
JOIN   ventas.Productos pr ON pr.ProductoId = d.ProductoId
GROUP  BY d.PedidoId;

-- Por categoría, con nvarchar(max) para no topar en 8,000 bytes
SELECT Categoria,
       STRING_AGG(CAST(Nombre AS nvarchar(max)), N' | ') WITHIN GROUP (ORDER BY Nombre) AS Lista
FROM   ventas.Productos
GROUP  BY Categoria;

-- Lista de ids que viene de la aplicación
DECLARE @Ids varchar(100) = '3,1,5';
SELECT p.ProductoId, p.Nombre, p.Precio
FROM   STRING_SPLIT(@Ids, ',') AS s
JOIN   ventas.Productos p ON p.ProductoId = TRY_CAST(s.value AS int);

-- SQL Server 2022+: conservar el orden en que llegaron
SELECT s.ordinal, p.Nombre
FROM   STRING_SPLIT(@Ids, ',', 1) AS s
JOIN   ventas.Productos p ON p.ProductoId = TRY_CAST(s.value AS int)
ORDER  BY s.ordinal;
```

`TRY_CAST` evita que un elemento no numérico (por ejemplo un espacio o texto) haga fallar toda la consulta; simplemente no empata. Desde Dapper, para listas de ids normalmente es más limpio usar `WHERE ProductoId IN @Ids` (Dapper expande la lista) o un [[Table-valued parameters|TVP]]; `STRING_SPLIT` es útil cuando la lista ya llega como texto (un parámetro de query string, un campo importado).

### Cuándo usarlo y cuándo no
- **`STRING_AGG`** para mostrar listas en reportes, armar etiquetas o exportar a CSV. Si el consumidor es tu API, a veces es mejor devolver filas y que C# arme la lista, o usar `FOR JSON` para devolver un arreglo real.
- **`STRING_SPLIT`** para listas cortas que llegan como texto. Para listas largas o con estructura (id + cantidad), `OPENJSON` o un TVP son más robustos.
- **No guardes listas separadas por comas en una columna**: viola la [[Primera forma normal (1FN)]]; `STRING_SPLIT` no es excusa para ese diseño.

### Errores comunes
- **Error de 8,000 bytes en `STRING_AGG`**: convierte la entrada a `nvarchar(max)`.
- **Confiar en el orden de `STRING_SPLIT`**: usa `enable_ordinal` (2022+) o `OPENJSON` sobre un arreglo, cuyo `key` sí indica la posición.
- **Separador de varios caracteres** en `STRING_SPLIT`: no se admite.
- **Espacios**: `'1, 2'` produce `' 2'`; aplica `TRIM` antes de convertir.
- **Olvidar `WITHIN GROUP`** cuando el orden importa en la lista concatenada.

### Para profundizar
- [STRING_AGG (Transact-SQL)](https://learn.microsoft.com/en-us/sql/t-sql/functions/string-agg-transact-sql)
- [STRING_SPLIT (Transact-SQL)](https://learn.microsoft.com/en-us/sql/t-sql/functions/string-split-transact-sql)

## Ejercicios

Resuélvelos en papel o en la herramienta; la solución está plegada.

> [!question]- Con la base Tienda ([[Lab 1 - Base de práctica Tienda en SQL Server]]): cada pedido con la lista de sus productos en orden alfabético.
> ```sql
> SELECT p.PedidoId,
>        STRING_AGG(pr.Nombre, N', ') WITHIN GROUP (ORDER BY pr.Nombre) AS Productos
> FROM   ventas.Pedidos p
> JOIN   ventas.PedidoDetalle d ON d.PedidoId = p.PedidoId
> JOIN   ventas.Productos pr    ON pr.ProductoId = d.ProductoId
> GROUP  BY p.PedidoId;
> ```
> 1: «Mouse inalámbrico, Teclado mecánico»; 3: «Cable HDMI, Monitor 27»; 5: «Laptop 14, Mouse inalámbrico»…

> [!question]- Recibes `@Ids = '3,1,5'` desde la aplicación: devuelve esos productos.
> ```sql
> DECLARE @Ids nvarchar(200) = N'3,1,5';
> SELECT pr.ProductoId, pr.Nombre
> FROM   STRING_SPLIT(@Ids, N',') AS s
> JOIN   ventas.Productos pr ON pr.ProductoId = TRY_CAST(s.value AS int);
> ```
> Monitor 27, Teclado mecánico y Cable HDMI. Para listas grandes o tipadas, mejor un [[Table-valued parameters|TVP]].

> [!question]- Por categoría, la lista de nombres de producto separada por « | ».
> ```sql
> SELECT Categoria, STRING_AGG(Nombre, N' | ') WITHIN GROUP (ORDER BY Nombre) AS Productos
> FROM   ventas.Productos
> GROUP  BY Categoria;
> ```
> Periféricos: «Mouse inalámbrico | Teclado mecánico | Webcam HD».

## Autoevaluación

Contesta primero en voz alta o en papel; luego despliega la respuesta.

> [!question]- ¿Qué hacen `STRING_AGG` y `STRING_SPLIT`?
> `STRING_AGG(col, ', ')` concatena valores de varias filas en un texto (SQL Server 2017+); `STRING_SPLIT('a,b,c', ',')` parte un texto en filas con la columna `value` (2016+, compatibilidad 130).

> [!question]- ¿El orden de `STRING_SPLIT` está garantizado?
> No. Si lo necesitas, usa el tercer argumento `enable_ordinal` (`STRING_SPLIT(@s, ',', 1)` devuelve `ordinal`), disponible en SQL Server 2022 y Azure SQL. En versiones anteriores, `OPENJSON` sobre un arreglo te da `[key]` con la posición.

> [!question]- ¿Qué error común da `STRING_AGG` con textos largos?
> Si la entrada es `nvarchar(n)`/`varchar(n)`, el resultado se limita a 4000/8000 y falla con el error 9829 al pasarse. Convierte la entrada: `STRING_AGG(CAST(Nombre AS nvarchar(max)), ', ')`.

## Práctica

- [[Lab 3 - CTE y funciones de ventana en Tienda]]

