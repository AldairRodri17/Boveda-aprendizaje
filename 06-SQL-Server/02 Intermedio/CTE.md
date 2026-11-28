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
# CTE

Parte de: [[06-SQL-Server/02 Intermedio/_Índice Intermedio|Intermedio]]

> [!info] Cómo avanzar
> `progreso`: **pendiente** → **viendo** (estudiando) → **visto** (contestaste la autoevaluación sin abrir las respuestas) → **dominado** (hiciste el laboratorio o lo aplicaste de verdad).

## Contenido

### Qué es
Una **CTE** (*common table expression*, expresión de tabla común) es una consulta con nombre que defines con `WITH Nombre AS (SELECT ...)` justo antes de una instrucción y que puedes usar en ella como si fuera una tabla. Su propósito principal es la **legibilidad**: das nombre a un paso intermedio ("ventas por cliente") y luego lo consultas, en lugar de anidar una subconsulta dentro de otra. También permite cosas que de otro modo serían incómodas, como filtrar sobre el resultado de una función de ventana o borrar duplicados.

### Cómo funciona
1. Escribes `WITH NombreCte [(col1, col2)] AS ( consulta )`.
2. Inmediatamente después va **una** instrucción (`SELECT`, `INSERT`, `UPDATE`, `DELETE` o `MERGE`) que usa `NombreCte`.
3. Al terminar esa instrucción, la CTE deja de existir.

Lo que **no** hace es guardar resultados. SQL Server trata la CTE como una vista en línea: la sustituye por su definición y optimiza la consulta completa. No hay una tabla en memoria con los datos de la CTE. Si la referencias dos veces, la lógica se evalúa dos veces. Si necesitas calcular algo caro una sola vez y reutilizarlo, guárdalo en una [[Tablas temporales y variables de tabla|tabla temporal]].

```mermaid
flowchart LR
    A["WITH VentasCliente AS (...)"] --> B["SELECT ... FROM VentasCliente"]
    B --> C["El optimizador expande la CTE en línea"]
    C --> D["Un solo plan para todo"]
```

**El punto y coma**: `WITH` tiene otros usos en T-SQL (por ejemplo, `WITH (NOLOCK)` o `WITH XMLNAMESPACES`), así que la instrucción anterior debe terminar en `;` para que el parser sepa que empieza una CTE. Por eso ves `;WITH` en código ajeno: es un parche para cuando no terminan las instrucciones. Lo correcto es terminar **todas** las instrucciones con `;`, algo que Microsoft ya marca como obligatorio a futuro.

**Modificar a través de una CTE**: si la CTE se basa en una sola tabla, puedes hacer `UPDATE` o `DELETE` contra ella y el cambio se aplica a la tabla base. Es el truco clásico para borrar duplicados con `ROW_NUMBER`.

### Ejemplo
Con la base de [[Lab 1 - Base de práctica Tienda en SQL Server]]:

```sql
-- Importe por cliente y filtro sobre el resultado agregado
WITH VentasCliente AS (
    SELECT p.ClienteId, SUM(d.Cantidad * d.PrecioUnitario) AS Importe
    FROM   ventas.Pedidos p
    JOIN   ventas.PedidoDetalle d ON d.PedidoId = p.PedidoId
    WHERE  p.Estado <> 'Cancelado'
    GROUP  BY p.ClienteId
)
SELECT c.Nombre, v.Importe
FROM   VentasCliente v
JOIN   ventas.Clientes c ON c.ClienteId = v.ClienteId
WHERE  v.Importe > 10000;

-- Borrar duplicados dejando uno de cada valor
WITH Dup AS (
    SELECT Email,
           ROW_NUMBER() OVER (PARTITION BY Email ORDER BY (SELECT NULL)) AS rn
    FROM   #Correos
)
DELETE FROM Dup WHERE rn > 1;
```

En el segundo caso, la CTE numera las filas de cada correo; el `DELETE` sobre `Dup` borra en `#Correos` todas menos la primera. Es imposible escribir `WHERE ROW_NUMBER() ... > 1` directamente (las funciones de ventana no se permiten en `WHERE`), y la CTE resuelve justo eso.

Desde Dapper, la consulta con CTE se guarda en su archivo `.sql` y se ejecuta igual que cualquier otra: `await conn.QueryAsync<ClienteImporte>(sql, new { Minimo = 10000m })`. Si el umbral viene como parámetro (`WHERE v.Importe > @Minimo`), la CTE no cambia.

### Cuándo usarlo y cuándo no
- **Úsala** para dar nombre a pasos intermedios, para filtrar resultados de [[Funciones de ventana]], para borrar duplicados y para recorrer jerarquías con [[CTE recursivas]].
- Si tienes varios pasos, encadénalos en un solo `WITH` (ver [[CTE múltiples]]).
- **No la uses como caché**: si el resultado se usa en varias instrucciones o varias veces dentro de una consulta pesada, una tabla `#` suele ser mejor.
- Frente a una [[Subconsultas|subconsulta]] o tabla derivada, el rendimiento es normalmente idéntico: elige la que se lea mejor.

### Errores comunes
- **Olvidar el `;` anterior**: error de sintaxis cerca de `WITH`.
- **Usar la CTE en la siguiente instrucción**: "nombre de objeto no válido"; solo vive una instrucción.
- **Columnas calculadas sin alias**: cada columna de la CTE necesita nombre.
- **`ORDER BY` dentro de la CTE** sin `TOP`: no está permitido y, aunque uses `TOP`, el orden final solo lo garantiza el `ORDER BY` del `SELECT` exterior.
- **Creer que mejora el rendimiento por sí misma**: es azúcar sintáctica; el plan es el mismo que con la subconsulta equivalente.

### Para profundizar
- [WITH common_table_expression (Transact-SQL)](https://learn.microsoft.com/en-us/sql/t-sql/queries/with-common-table-expression-transact-sql)
- [Transact-SQL syntax conventions (punto y coma)](https://learn.microsoft.com/en-us/sql/t-sql/language-elements/transact-sql-syntax-conventions-transact-sql)

## Ejercicios

Resuélvelos en papel o en la herramienta; la solución está plegada.

> [!question]- Con la base Tienda ([[Lab 1 - Base de práctica Tienda en SQL Server]]): con una CTE, calcula el importe por cliente (sin cancelados) y devuelve solo los que superan 10,000.
> ```sql
> WITH VentasCliente AS (
>     SELECT p.ClienteId, SUM(d.Cantidad * d.PrecioUnitario) AS Importe
>     FROM   ventas.Pedidos p
>     JOIN   ventas.PedidoDetalle d ON d.PedidoId = p.PedidoId
>     WHERE  p.Estado <> 'Cancelado'
>     GROUP  BY p.ClienteId
> )
> SELECT c.Nombre, v.Importe
> FROM   VentasCliente v
> JOIN   ventas.Clientes c ON c.ClienteId = v.ClienteId
> WHERE  v.Importe > 10000;
> ```
> Bruno (21100) y Diego (18450).

> [!question]- Con una CTE y `ROW_NUMBER`, borra duplicados de una tabla `#Correos(Email nvarchar(100))` dejando uno de cada valor.
> ```sql
> WITH d AS (
>     SELECT Email, ROW_NUMBER() OVER (PARTITION BY Email ORDER BY (SELECT NULL)) AS rn
>     FROM   #Correos
> )
> DELETE FROM d WHERE rn > 1;
> ```
> Se puede borrar a través de una CTE de una sola tabla.

> [!question]- Total por pedido en una CTE y luego el pedido de mayor importe.
> ```sql
> WITH Totales AS (
>     SELECT PedidoId, SUM(Cantidad * PrecioUnitario) AS Total
>     FROM   ventas.PedidoDetalle
>     GROUP  BY PedidoId
> )
> SELECT TOP (1) PedidoId, Total FROM Totales ORDER BY Total DESC;
> ```
> Pedido 2 (18500).

## Autoevaluación

Contesta primero en voz alta o en papel; luego despliega la respuesta.

> [!question]- ¿Qué es una CTE (`WITH ... AS (...)`)?
> Una consulta con nombre que existe solo durante **la siguiente instrucción**. Sirve para dar nombre a pasos intermedios y leer la consulta de arriba hacia abajo.

> [!question]- ¿SQL Server guarda (materializa) el resultado de una CTE?
> No: se expande en línea como una vista. Si la referencias dos veces, se evalúa dos veces. Si necesitas reutilizar un resultado caro, usa una [[Tablas temporales y variables de tabla|tabla temporal]].

> [!question]- ¿Por qué a veces ves `;WITH`?
> Porque la instrucción anterior a `WITH` debe terminar en `;`. Escribir `;WITH` es un parche; lo correcto es terminar todas las instrucciones con `;`.

## Práctica

- [[Lab 3 - CTE y funciones de ventana en Tienda]]

