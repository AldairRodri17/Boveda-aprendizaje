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
# DISTINCT

Parte de: [[06-SQL-Server/01 Básico/_Índice Básico|Básico]]

> [!info] Cómo avanzar
> `progreso`: **pendiente** → **viendo** (estudiando) → **visto** (contestaste la autoevaluación sin abrir las respuestas) → **dominado** (hiciste el laboratorio o lo aplicaste de verdad).

## Contenido

### Qué es
`DISTINCT` elimina las filas **duplicadas** del resultado de un `SELECT`. Sirve para obtener listas de valores únicos (categorías, ciudades) y, dentro de un agregado, para contar o sumar valores distintos (`COUNT(DISTINCT ClienteId)`). Es sencillo, pero también es una de las palabras que más esconden errores de diseño de consultas.

### Cómo funciona
- `SELECT DISTINCT a, b` compara la **fila completa** del resultado (la combinación `a, b`), no solo la primera columna. No existe "DISTINCT de una columna y las demás como sea".
- Para decidir si dos filas son iguales, `DISTINCT` **trata los `NULL` como iguales entre sí**: todas las filas con `Ciudad = NULL` se colapsan en una sola. Esto es distinto de la comparación `=` en `WHERE`, donde `NULL = NULL` no es verdadero.
- La igualdad de textos depende de la **collation**: con una collation `CI` (insensible a mayúsculas), `'cdmx'` y `'CDMX'` cuentan como el mismo valor y se muestra uno de ellos.
- En el orden lógico ([[SELECT]]), `DISTINCT` se aplica **después** del `SELECT` y **antes** del `ORDER BY` y el `TOP`. Por eso, con `DISTINCT`, el `ORDER BY` solo puede usar columnas que estén en la lista del `SELECT`.
- Físicamente el motor lo resuelve ordenando (operador *Sort* + *Stream Aggregate*) o con una tabla hash (*Hash Match*). En tablas grandes eso cuesta memoria y CPU; si hay un índice sobre las columnas, puede leerlas ya ordenadas.
- `COUNT(DISTINCT col)` cuenta valores distintos **sin incluir `NULL`** (como todo agregado sobre columna, ver [[Funciones de agregación]]).

```mermaid
flowchart LR
  A[FROM / WHERE] --> B[SELECT columnas] --> C[DISTINCT quita filas repetidas] --> D[ORDER BY] --> E[TOP / OFFSET]
```

### Ejemplo
```sql
-- Valores únicos: combinación Categoria + Precio redondeado
SELECT DISTINCT Categoria, CAST(ROUND(Precio, -3) AS int) AS PrecioMiles
FROM   ventas.Productos
ORDER  BY Categoria, PrecioMiles;

-- Dentro de un agregado: cuántos productos distintos se han vendido y a cuántos clientes
SELECT COUNT(DISTINCT d.ProductoId) AS ProductosVendidos,
       COUNT(DISTINCT p.ClienteId)  AS ClientesConCompra
FROM   ventas.PedidoDetalle AS d
JOIN   ventas.Pedidos       AS p ON p.PedidoId = d.PedidoId
WHERE  p.Estado <> 'Cancelado';

-- DISTINCT que oculta un problema: el JOIN multiplica filas y se "arregla" con DISTINCT
SELECT DISTINCT c.ClienteId, c.Nombre
FROM   ventas.Clientes AS c
JOIN   ventas.Pedidos  AS p ON p.ClienteId = c.ClienteId;

-- Mejor: expresar la intención ("clientes que tienen pedidos") con EXISTS
SELECT c.ClienteId, c.Nombre
FROM   ventas.Clientes AS c
WHERE  EXISTS (SELECT 1 FROM ventas.Pedidos AS p WHERE p.ClienteId = c.ClienteId);
```

La segunda versión no genera duplicados que luego haya que eliminar y el optimizador puede detenerse en el primer pedido encontrado (semi join). Ver [[EXISTS vs IN]].

Desde Dapper, una lista de valores únicos para un combo del front (Itzamna.Clio) se lee directo como `IEnumerable<string>`:

```csharp
// Consultas/Catalogos/CategoriasDistintas.sql contiene el SELECT DISTINCT ... ORDER BY
var categorias = await conexion.QueryAsync<string>(sql);
```

### Cuándo usarlo y cuándo no
- **Úsalo** cuando de verdad necesitas valores únicos de una columna o combinación: listas para filtros, conteos de entidades distintas.
- **Prefiere `GROUP BY`** cuando además de los valores únicos quieres agregados (cuántos productos por categoría). `SELECT DISTINCT Categoria` y `SELECT Categoria ... GROUP BY Categoria` suelen producir el mismo plan.
- **Prefiere `EXISTS`** cuando la pregunta es "filas de A que tienen al menos una fila en B".
- **Desconfía** de un `DISTINCT` agregado "porque salían repetidos": casi siempre indica un JOIN que multiplica filas (fan-out, ver [[JOINs]]) o una condición de JOIN incompleta. El DISTINCT tapa el síntoma, cuesta rendimiento y puede esconder errores de importes duplicados.

### Errores comunes
1. **Creer que aplica a una sola columna**: `SELECT DISTINCT Categoria, Nombre` devuelve cada producto, porque las combinaciones son únicas.
2. **Usarlo para corregir duplicados de un JOIN**: arregla la vista pero si además sumas importes, el total ya venía inflado. Corrige la causa.
3. **Esperar que `COUNT(DISTINCT Ciudad)` cuente el `NULL`**: no lo cuenta; `SELECT DISTINCT Ciudad` sí muestra una fila `NULL`.
4. **`ORDER BY` por una columna que no está en el SELECT** junto con DISTINCT: error 145.
5. **`DISTINCT` en consultas enormes** sin índice: un *Sort* gigante que puede derramar a tempdb (ver [[Advertencias del plan (conversiones implícitas y spills)]]).

### Para profundizar
- [SELECT - cláusula SELECT (DISTINCT)](https://learn.microsoft.com/sql/t-sql/queries/select-clause-transact-sql)
- [COUNT (Transact-SQL)](https://learn.microsoft.com/sql/t-sql/functions/count-transact-sql)

## Ejercicios

Resuélvelos en papel o en la herramienta; la solución está plegada.

> [!question]- Con la base Tienda ([[Lab 1 - Base de práctica Tienda en SQL Server]]): categorías distintas de productos, ordenadas.
> ```sql
> SELECT DISTINCT Categoria FROM ventas.Productos ORDER BY Categoria;
> ```
> Accesorios, Computadoras, Monitores, Periféricos.

> [!question]- ¿Cuántos clientes distintos han hecho al menos un pedido?
> ```sql
> SELECT COUNT(DISTINCT ClienteId) AS Clientes FROM ventas.Pedidos;
> ```
> 5 (todos menos Fernando Gil).

> [!question]- Ciudades distintas de clientes, incluyendo una fila para quienes no tienen ciudad. ¿Cuántas filas salen?
> ```sql
> SELECT DISTINCT Ciudad FROM ventas.Clientes;
> ```
> 4 filas: `NULL`, CDMX, Guadalajara, Monterrey.

## Autoevaluación

Contesta primero en voz alta o en papel; luego despliega la respuesta.

> [!question]- ¿`DISTINCT` se aplica a una columna o a toda la fila?
> A toda la fila del `SELECT`: `SELECT DISTINCT Ciudad, Nombre` quita solo las filas donde ambas columnas se repiten.

> [!question]- ¿Cuándo un `DISTINCT` es señal de un error en la consulta?
> Cuando lo agregas para «quitar duplicados» que aparecieron por un `JOIN` que multiplica filas (p. ej. unir pedidos con detalle para listar clientes). Mejor usar `EXISTS` o agregar antes de unir; el `DISTINCT` esconde el problema y cuesta un ordenamiento o hash.

> [!question]- ¿`DISTINCT` considera iguales dos `NULL`?
> Sí: para `DISTINCT`, `GROUP BY`, `UNION`, `INTERSECT` y `EXCEPT` los `NULL` se agrupan juntos, aunque `NULL = NULL` no sea verdadero en un `WHERE`.

