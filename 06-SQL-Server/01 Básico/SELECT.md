---
proyecto: Conocimiento
tipo: concepto
nivel: basico
progreso: pendiente
estado: pendiente
area: SQL Server
tags: [sql-server]
---
# SELECT

Parte de: [[06-SQL-Server/01 Básico/_Índice Básico|Básico]]

> [!info] Cómo avanzar
> `progreso`: **pendiente** → **viendo** (estudiando) → **visto** (contestaste la autoevaluación sin abrir las respuestas) → **dominado** (hiciste el laboratorio o lo aplicaste de verdad).

## Contenido

### Qué es
`SELECT` es la instrucción para **leer datos**: describe qué columnas o expresiones quieres, de qué tablas, con qué filtros, agrupaciones y orden. Es declarativa: dices *qué* resultado quieres y el optimizador decide *cómo* obtenerlo. Casi todo lo que se aprende en esta sección son cláusulas de `SELECT`.

### Cómo funciona
La sintaxis se escribe en un orden, pero se **evalúa en otro** (orden lógico). Este orden explica qué puedes usar en cada cláusula:

| Orden lógico | Cláusula | Qué hace |
|---|---|---|
| 1 | `FROM` y `JOIN ... ON` | Arma el conjunto de filas de origen |
| 2 | `WHERE` | Filtra filas |
| 3 | `GROUP BY` | Forma grupos |
| 4 | `HAVING` | Filtra grupos |
| 5 | `SELECT` | Calcula columnas y alias |
| 6 | `DISTINCT` | Quita duplicados |
| 7 | `ORDER BY` | Ordena |
| 8 | `TOP` / `OFFSET FETCH` | Limita filas |

```mermaid
flowchart LR
  F[FROM/JOIN] --> W[WHERE] --> G[GROUP BY] --> H[HAVING] --> S[SELECT] --> D[DISTINCT] --> O[ORDER BY] --> T[TOP/OFFSET]
```

Consecuencias:

- **Un alias del `SELECT` no existe en el `WHERE`** porque el `WHERE` se evalúa antes. `SELECT Precio * 1.16 AS ConIva ... WHERE ConIva > 1000` da error 207 ("nombre de columna no válido"). Repite la expresión (`WHERE Precio * 1.16 > 1000`), o calcula el alias en una subconsulta, CTE o `CROSS APPLY` y filtra afuera.
- **El alias sí existe en `ORDER BY`**, que va después.
- Una expresión del `SELECT` puede usar columnas de todas las tablas del `FROM`.

**Otras piezas**:

- **Expresiones y alias**: `Precio * 1.16 AS PrecioConIva`. Usa `AS` para que se lea claro.
- **`SELECT` sin `FROM`**: válido en T-SQL para evaluar expresiones: `SELECT SYSUTCDATETIME() AS AhoraUtc, DB_NAME() AS BaseActual;`.
- **Asignar a variables**: `SELECT @Total = SUM(Precio) FROM ...`.
- **Literales de texto** con `N'...'` cuando son `nvarchar`, para no perder caracteres.

**¿Por qué evitar `SELECT *` en código de aplicación?**

1. Traes columnas que no usas (más red, más memoria, más lecturas de disco), sobre todo columnas grandes como `nvarchar(max)`.
2. Impides que un índice "cubra" la consulta y fuerzas *Key Lookups* (ver [[Columnas incluidas y filtered indexes]]).
3. El contrato se rompe cuando alguien agrega, quita o reordena columnas: un mapeo por posición falla o una columna nueva sensible (un hash de contraseña) sale hacia la API sin querer.
4. Las vistas con `SELECT *` guardan la lista de columnas al crearse y quedan desfasadas.

`SELECT *` es aceptable en exploración manual, en `EXISTS (SELECT * ...)` (ahí no se leen columnas) y en `COUNT(*)`.

### Ejemplo
```sql
-- Columnas explícitas, expresión con alias y orden por alias
SELECT p.ProductoId,
       p.Nombre,
       p.Precio,
       CAST(p.Precio * 1.16 AS decimal(10,2)) AS PrecioConIva
FROM   ventas.Productos AS p
WHERE  p.Precio * 1.16 > 1000          -- el alias aún no existe aquí
ORDER  BY PrecioConIva DESC;           -- aquí sí

-- Reutilizar un cálculo sin repetirlo: CROSS APPLY
SELECT p.Nombre, x.PrecioConIva
FROM   ventas.Productos AS p
CROSS  APPLY (SELECT CAST(p.Precio * 1.16 AS decimal(10,2)) AS PrecioConIva) AS x
WHERE  x.PrecioConIva > 1000;

-- Sin FROM: diagnóstico rápido
SELECT SYSUTCDATETIME() AS AhoraUtc, DB_NAME() AS Base, @@VERSION AS Version;
```

En el proyecto, cada consulta vive en su archivo `.sql` con columnas explícitas que coinciden con las propiedades del DTO; Dapper mapea **por nombre**, así que el alias es el contrato:

```csharp
public sealed record ProductoPrecio(int ProductoId, string Nombre, decimal Precio, decimal PrecioConIva);
var lista = await conexion.QueryAsync<ProductoPrecio>(sql);   // sql leído del archivo embebido
```

### Cuándo usarlo y cuándo no
- **Usa `SELECT` con columnas explícitas** en todo código que se despliega.
- **Calcula en SQL** lo que reduce datos (filtros, agregados) y deja en C# o React lo que es presentación (formato de moneda, traducciones).
- Para lecturas complejas que se repiten, considera una vista o un procedimiento; para una sola pantalla, un archivo `.sql` por consulta está bien.

### Errores comunes
1. **Usar un alias del `SELECT` en `WHERE`, `GROUP BY` o `HAVING`**: error 207.
2. **`SELECT *` en producción**: contratos frágiles y planes peores.
3. **Asumir orden sin `ORDER BY`**.
4. **Literales sin `N`** para texto con acentos o emojis en columnas `nvarchar`, que pueden perder caracteres si la collation del literal no los soporta.
5. **Funciones escalares por fila** en el `SELECT` sobre millones de filas: pueden serializar el plan (ver [[Funciones escalares y de tabla]]).

### Para profundizar
- [SELECT (Transact-SQL)](https://learn.microsoft.com/sql/t-sql/queries/select-transact-sql)
- [Orden lógico de procesamiento de SELECT](https://learn.microsoft.com/sql/t-sql/queries/select-transact-sql#logical-processing-order-of-the-select-statement)

## Ejercicios

Resuélvelos en papel o en la herramienta; la solución está plegada.

> [!question]- Con la base Tienda ([[Lab 1 - Base de práctica Tienda en SQL Server]]): lista nombre, precio y precio con IVA (16 %) de cada producto, del más caro al más barato.
> ```sql
> SELECT Nombre, Precio, Precio * 1.16 AS PrecioConIva
> FROM   ventas.Productos
> ORDER  BY Precio DESC;
> ```
> Laptop 14 → 21460.00, Monitor 27 → 6032.00, Teclado → 1392.00, Webcam → 1044.00, Mouse → 522.00, Cable → 174.00.

> [!question]- Muestra los pedidos con una columna `Anio` y otra `Mes`, ordenados por `Anio, Mes` usando los alias.
> ```sql
> SELECT PedidoId, YEAR(Fecha) AS Anio, MONTH(Fecha) AS Mes
> FROM   ventas.Pedidos
> ORDER  BY Anio, Mes, PedidoId;
> ```
> El `ORDER BY` sí ve los alias porque se evalúa después del `SELECT`.

> [!question]- Escribe una consulta que devuelva la fecha y hora UTC actual y el nombre de la base en uso, sin `FROM`.
> ```sql
> SELECT SYSUTCDATETIME() AS AhoraUtc, DB_NAME() AS BaseActual;
> ```

## Autoevaluación

Contesta primero en voz alta o en papel; luego despliega la respuesta.

> [!question]- ¿En qué orden lógico se evalúan las cláusulas de un `SELECT`?
> `FROM` (y `JOIN`) → `WHERE` → `GROUP BY` → `HAVING` → `SELECT` → `DISTINCT` → `ORDER BY` → `TOP`/`OFFSET`. Por eso un alias del `SELECT` no existe en el `WHERE`, pero sí en el `ORDER BY`.

> [!question]- ¿Por qué evitar `SELECT *` en código de aplicación?
> Trae columnas que no usas (más E/S y red), impide índices cubrientes, y si alguien agrega o reordena columnas, el código que mapea por posición se rompe. En vistas, `SELECT *` además deja metadatos viejos hasta `sp_refreshview`.

> [!question]- ¿Qué pasa con `SELECT Precio * 1.16 AS ConIva FROM ventas.Productos WHERE ConIva > 1000`?
> Error 207 (columna inválida `ConIva`): el `WHERE` se evalúa antes que el `SELECT`. Repite la expresión (`WHERE Precio * 1.16 > 1000`) o usa una CTE/subconsulta.

## Práctica

- [[Lab 2 - Consultas de agregación y JOINs en Tienda]]

