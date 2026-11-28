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
# WHERE, LIKE, IN y BETWEEN

Parte de: [[06-SQL-Server/01 Básico/_Índice Básico|Básico]]

> [!info] Cómo avanzar
> `progreso`: **pendiente** → **viendo** (estudiando) → **visto** (contestaste la autoevaluación sin abrir las respuestas) → **dominado** (hiciste el laboratorio o lo aplicaste de verdad).

## Contenido

### Qué es
`WHERE` filtra las filas que entran al resultado (o que se actualizan o borran) según una condición. `LIKE`, `IN` y `BETWEEN` son operadores que se usan dentro de esa condición: búsqueda por patrón de texto, pertenencia a una lista y rango de valores. Junto con `AND`, `OR`, `NOT` e `IS NULL` permiten expresar casi cualquier filtro.

### Cómo funciona
**WHERE** conserva solo las filas cuya condición es **TRUE**; FALSE y UNKNOWN (por `NULL`) se descartan (ver [[NULL y lógica de tres valores]]).

**Precedencia lógica**: `NOT` se evalúa antes que `AND`, y `AND` antes que `OR`. Así, `Estado = 'Entregado' OR Estado = 'Enviado' AND Ciudad = 'Guadalajara'` significa `Entregado OR (Enviado AND Guadalajara)`, probablemente no lo que querías. **Usa paréntesis** siempre que mezcles `AND` y `OR`.

**IN**: `Ciudad IN (N'CDMX', N'Monterrey')` equivale a varios `OR` con `=`. También acepta una subconsulta (`WHERE ClienteId IN (SELECT ...)`). `NOT IN` con una subconsulta que puede traer `NULL` devuelve cero filas; usa `NOT EXISTS`.

**BETWEEN**: `x BETWEEN a AND b` equivale a `x >= a AND x <= b`: **incluye ambos extremos**. Funciona bien con enteros y fechas sin hora (`date`). Con columnas que tienen hora (`datetime2`), `BETWEEN '2025-03-01' AND '2025-03-31'` deja fuera todo lo del día 31 después de medianoche; usa un **rango semiabierto** (`>= '2025-03-01' AND < '2025-04-01'`), ver [[Funciones de fecha]].

**LIKE** busca patrones con comodines:

| Comodín | Significa | Ejemplo |
|---|---|---|
| `%` | Cero o más caracteres | `'Tecl%'` empieza con "Tecl" |
| `_` | Exactamente un carácter | `'_ouse%'` |
| `[abc]` | Un carácter del conjunto | `'[MT]%'` empieza con M o T |
| `[a-f]` | Un carácter del rango | |
| `[^abc]` | Un carácter que no esté en el conjunto | |

Para buscar un `%` o `_` literal: enciérralo en corchetes (`LIKE '%[%]%'`) o usa `ESCAPE` (`LIKE '%10!%%' ESCAPE '!'`).

**Rendimiento de LIKE**: con un prefijo fijo (`LIKE 'Tecl%'`) el motor puede hacer un *Index Seek* sobre un rango del índice, igual que con `>=` y `<`. Con un comodín al inicio (`LIKE '%teclado%'`) no sabe dónde empezar en el índice ordenado y tiene que **revisar todas las filas** (*scan*). Para búsquedas de palabras dentro de textos largos, considera [[Búsqueda de texto completo]].

**Mayúsculas y acentos** dependen de la **collation** de la columna. Con la collation típica `SQL_Latin1_General_CP1_CI_AS`, `CI` significa *case insensitive* (`'ana lópez' = 'Ana López'` es verdadero) y `AS` *accent sensitive* (`'Lopez'` no es igual a `'López'`). Puedes forzar otra en una comparación con `COLLATE`, aunque eso suele impedir el uso del índice. Ver [[Collation]].

### Ejemplo
```sql
-- AND/OR con paréntesis explícitos
SELECT p.PedidoId, p.Estado, c.Ciudad
FROM   ventas.Pedidos p
JOIN   ventas.Clientes c ON c.ClienteId = p.ClienteId
WHERE  p.Estado IN ('Entregado', 'Enviado')
  AND  c.Ciudad = N'Guadalajara';

-- Patrones: empieza con M o T, o tiene exactamente "27" al final
SELECT Nombre FROM ventas.Productos
WHERE  Nombre LIKE N'[MT]%' OR Nombre LIKE N'% 27';

-- Buscar un % literal en una descripción
SELECT Nombre FROM ventas.Productos WHERE Nombre LIKE N'%[%]%';

-- Rango de fechas inclusivo en una columna date: BETWEEN está bien
SELECT PedidoId, Fecha FROM ventas.Pedidos
WHERE  Fecha BETWEEN '2025-02-01' AND '2025-03-15';

-- Distinguir mayúsculas solo en esta comparación
SELECT Nombre FROM ventas.Clientes
WHERE  Nombre = N'ana lópez' COLLATE Latin1_General_CS_AS;   -- 0 filas
```

**Desde Dapper**, una lista para `IN` se pasa como colección y Dapper la expande en parámetros (`@Ciudades1, @Ciudades2...`):

```csharp
var clientes = await conexion.QueryAsync<ClienteDto>(
    "SELECT ClienteId, Nombre FROM ventas.Clientes WHERE Ciudad IN @Ciudades;",
    new { Ciudades = new[] { "CDMX", "Monterrey" } });
```

Para listas grandes (cientos de valores) es mejor un [[Table-valued parameters]] o `STRING_SPLIT`/`OPENJSON`, porque cada tamaño distinto de lista genera un plan distinto en caché y hay un límite de 2,100 parámetros por comando.

### Cuándo usarlo y cuándo no
- **`IN`** para listas cortas y fijas; **`EXISTS`** para condiciones contra otra tabla.
- **`BETWEEN`** para enteros y `date`; **rango semiabierto** para fechas con hora.
- **`LIKE 'prefijo%'`** para autocompletar; evita `'%texto%'` en tablas grandes.
- **No envuelvas la columna en funciones** (`WHERE UPPER(Nombre) = ...`, `WHERE YEAR(Fecha) = ...`): impiden el uso de índices ([[SARGability]]).

### Errores comunes
1. **Mezclar `AND` y `OR` sin paréntesis**.
2. **`BETWEEN` con `datetime2`**: se pierde el último día.
3. **`NOT IN` con `NULL`** en la subconsulta: cero filas.
4. **Comodín inicial** en tablas grandes: escaneo completo.
5. **Concatenar el valor del usuario** en el `LIKE` desde C#: inyección SQL. Pásalo como parámetro y arma el patrón con `@Texto + N'%'`.

### Para profundizar
- [WHERE (Transact-SQL)](https://learn.microsoft.com/sql/t-sql/queries/where-transact-sql)
- [LIKE (Transact-SQL)](https://learn.microsoft.com/sql/t-sql/language-elements/like-transact-sql)
- [BETWEEN (Transact-SQL)](https://learn.microsoft.com/sql/t-sql/language-elements/between-transact-sql)
- [IN (Transact-SQL)](https://learn.microsoft.com/sql/t-sql/language-elements/in-transact-sql)

## Ejercicios

Resuélvelos en papel o en la herramienta; la solución está plegada.

> [!question]- Con la base Tienda ([[Lab 1 - Base de práctica Tienda en SQL Server]]): productos cuyo nombre termina en «o» o empieza con «M».
> ```sql
> SELECT Nombre FROM ventas.Productos
> WHERE  Nombre LIKE N'%o' OR Nombre LIKE N'M%';
> ```
> Teclado mecánico, Mouse inalámbrico, Monitor 27.

> [!question]- Pedidos con fecha del 1 de febrero al 15 de marzo de 2025 (inclusive).
> ```sql
> SELECT PedidoId, Fecha FROM ventas.Pedidos
> WHERE  Fecha BETWEEN '2025-02-01' AND '2025-03-15';
> ```
> Pedidos 3, 4, 5 y 6. Funciona porque `Fecha` es `date` (sin hora).

> [!question]- Clientes de CDMX o Monterrey.
> ```sql
> SELECT Nombre FROM ventas.Clientes
> WHERE  Ciudad IN (N'CDMX', N'Monterrey');
> ```
> Bruno Díaz, Diego Ruiz, Fernando Gil.

> [!question]- Pedidos no cancelados de clientes de Guadalajara, combinando `AND`/`OR` correctamente: «estado Entregado o Enviado, y cliente de Guadalajara».
> ```sql
> SELECT p.PedidoId
> FROM   ventas.Pedidos p
> JOIN   ventas.Clientes c ON c.ClienteId = p.ClienteId
> WHERE  (p.Estado = 'Entregado' OR p.Estado = 'Enviado')
>   AND  c.Ciudad = N'Guadalajara';
> ```
> Pedidos 1 y 3 (de Ana). Sin paréntesis, `AND` se evalúa antes que `OR` y el resultado cambia.

## Autoevaluación

Contesta primero en voz alta o en papel; luego despliega la respuesta.

> [!question]- ¿`BETWEEN` incluye los extremos?
> Sí: `x BETWEEN a AND b` equivale a `x >= a AND x <= b`. Con fechas y hora es fácil equivocarse; prefiere `>= inicio AND < siguiente`.

> [!question]- ¿Qué comodines tiene `LIKE` en SQL Server y cómo buscas un `%` literal?
> `%` (cualquier cadena), `_` (un carácter), `[abc]` o `[a-f]` (uno del conjunto) y `[^a]` (que no sea). Para un `%` literal: `LIKE '%50[%]%'` o `LIKE '%50!%%' ESCAPE '!'`.

> [!question]- ¿Por qué `WHERE Nombre LIKE '%teclado%'` puede ser lento y `LIKE 'Tecl%'` no?
> Con comodín al inicio el índice no sirve para buscar (hay que revisar todas las filas). Con prefijo fijo, SQL Server convierte el `LIKE` en un rango y puede hacer *seek*. Para búsquedas de palabras en texto largo: [[Búsqueda de texto completo]].

> [!question]- ¿Distingue mayúsculas `WHERE Nombre = 'ana lópez'`?
> Depende de la intercalación: con una `_CI_` (case-insensitive, la habitual) no distingue; con `_CS_` sí. Ver [[Collation]].

## Práctica

- [[Lab 2 - Consultas de agregación y JOINs en Tienda]]

