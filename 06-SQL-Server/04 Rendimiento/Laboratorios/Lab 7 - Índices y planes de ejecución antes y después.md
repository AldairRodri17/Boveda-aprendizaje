---
proyecto: Conocimiento
tipo: laboratorio
nivel: intermedio
progreso: pendiente
estado: borrador
area: SQL Server
herramienta: SQL Server Developer o Express + SSMS (plan de ejecución real)
tags: [sql-server, laboratorio]
contenido: completo
---
# Lab 7 - Índices y planes de ejecución antes y después

Parte de: [[06-SQL-Server/04 Rendimiento/_Índice Rendimiento|Rendimiento]]

**Cubre:** [[Índices]] · [[Índice clustered]] · [[Índice nonclustered]] · [[Columnas incluidas y filtered indexes]] · [[Planes de ejecución]] · [[Plan estimado vs plan real]] · [[Operadores de acceso (Scan, Seek y Key Lookup)]] · [[Advertencias del plan (conversiones implícitas y spills)]] · [[SARGability]] · [[Estadísticas]]

> [!info] Al terminar
> Si todos los criterios quedan marcados, cambia a `dominado` el `progreso` de este laboratorio y de las notas que cubre. Guarda tus archivos de práctica en esta misma carpeta.

## Objetivo

Medir el efecto de los índices con lecturas lógicas y planes reales: de *scan* a *seek* + *Key Lookup* a índice cubriente, y ver cómo un predicado no SARGable o una conversión implícita desperdician el índice. Usa la base Tienda ([[Lab 1 - Base de práctica Tienda en SQL Server]]).

## Pasos

1. Crea `ventas.Movimientos (MovimientoId int IDENTITY PK, ClienteId int, Fecha datetime2(0), Monto decimal(10,2), Referencia varchar(20))` y llénala con 500,000 filas aleatorias (5,000 clientes, fechas de 2025).
2. Activa `SET STATISTICS IO, TIME ON;` y el plan real (Ctrl+M).
3. **Línea base**: `SELECT MovimientoId, Fecha, Monto FROM ventas.Movimientos WHERE ClienteId = 42;` Anota lecturas lógicas y operador.
4. Crea `IX_Mov_Cliente (ClienteId)` y repite. Anota lecturas y operadores (busca el *Key Lookup*).
5. Recrea el índice con `INCLUDE (Fecha, Monto)` y repite.
6. **SARGability**: crea `IX_Mov_Fecha (Fecha) INCLUDE (Monto)` y compara `WHERE YEAR(Fecha) = 2025 AND MONTH(Fecha) = 6` contra el rango equivalente con `SUM(Monto)`.
7. **Conversión implícita**: crea `IX_Mov_Ref (Referencia)` y compara `WHERE Referencia = 'REF-123'` contra `WHERE Referencia = N'REF-123'`. Revisa la advertencia en el `SELECT` del plan.
8. **Estadísticas**: mira el histograma de `IX_Mov_Cliente` con `DBCC SHOW_STATISTICS` y compara filas estimadas y reales en el paso 5.
9. Registra los números en la tabla de la sección de notas.

## Criterios de terminado

- [ ] Tengo una tabla con lecturas lógicas de los pasos 3, 4 y 5, y la del paso 5 es la menor (de miles de páginas a una o pocas decenas).
- [ ] El plan del paso 3 muestra *Clustered Index Scan*; el del 4, *Index Seek* + *Key Lookup* + *Nested Loops*; el del 5, solo *Index Seek*.
- [ ] En el paso 6 la versión con `YEAR/MONTH` hace *scan* y la de rango hace *seek* con muchas menos lecturas.
- [ ] En el paso 7 identifiqué la advertencia `CONVERT_IMPLICIT` con el literal `N'...'` y anoté si el plan cambió de *seek* a *scan* (depende de la intercalación).
- [ ] Puedo explicar con el histograma por qué el optimizador estimó ~100 filas para el cliente 42.

## Solución

> [!success]- Tabla y datos
> ```sql
> CREATE TABLE ventas.Movimientos (
>     MovimientoId int IDENTITY(1,1) NOT NULL CONSTRAINT PK_Movimientos PRIMARY KEY,
>     ClienteId    int           NOT NULL,
>     Fecha        datetime2(0)  NOT NULL,
>     Monto        decimal(10,2) NOT NULL,
>     Referencia   varchar(20)   NOT NULL
> );
> INSERT INTO ventas.Movimientos (ClienteId, Fecha, Monto, Referencia)
> SELECT TOP (500000)
>        ABS(CHECKSUM(NEWID())) % 5000 + 1,
>        DATEADD(minute, ABS(CHECKSUM(NEWID())) % 525600, '2025-01-01'),
>        CAST(ABS(CHECKSUM(NEWID())) % 100000 / 100.0 AS decimal(10,2)),
>        CONCAT('REF-', ROW_NUMBER() OVER (ORDER BY (SELECT NULL)))
> FROM   sys.all_objects a CROSS JOIN sys.all_objects b;
> ```
>
> 525,600 minutos = 1 año. Cada cliente queda con ~100 movimientos.

> [!success]- Índices de los pasos 4 a 7
> ```sql
> CREATE INDEX IX_Mov_Cliente ON ventas.Movimientos (ClienteId);
> -- paso 5
> CREATE INDEX IX_Mov_Cliente ON ventas.Movimientos (ClienteId) INCLUDE (Fecha, Monto)
>     WITH (DROP_EXISTING = ON);
> -- paso 6
> CREATE INDEX IX_Mov_Fecha ON ventas.Movimientos (Fecha) INCLUDE (Monto);
> SELECT SUM(Monto) FROM ventas.Movimientos WHERE YEAR(Fecha) = 2025 AND MONTH(Fecha) = 6;      -- scan
> SELECT SUM(Monto) FROM ventas.Movimientos WHERE Fecha >= '2025-06-01' AND Fecha < '2025-07-01'; -- seek
> -- paso 7
> CREATE INDEX IX_Mov_Ref ON ventas.Movimientos (Referencia);
> SELECT MovimientoId FROM ventas.Movimientos WHERE Referencia = 'REF-123';
> SELECT MovimientoId FROM ventas.Movimientos WHERE Referencia = N'REF-123';
> -- paso 8
> DBCC SHOW_STATISTICS ('ventas.Movimientos', 'IX_Mov_Cliente');
> ```

> [!success]- Qué deberías observar
> - Paso 3: *Clustered Index Scan* con lecturas del tamaño de la tabla (miles de páginas).
> - Paso 4: *Index Seek* en `IX_Mov_Cliente` + ~100 *Key Lookups* (unas centenas de lecturas). Si el cliente tuviera decenas de miles de filas, el optimizador preferiría el scan.
> - Paso 5: *Index Seek* sin lookup (unas pocas lecturas).
> - Paso 7: con intercalaciones SQL (`SQL_Latin1_General_CP1_CI_AS`) el literal `nvarchar` obliga a convertir la columna y suele producir *scan*; con intercalaciones de Windows SQL Server puede hacer un *seek* por rango con advertencia. En los dos casos la corrección es enviar el tipo correcto (Dapper: `DbString { IsAnsi = true }`).
> - Paso 8: el histograma tiene hasta 200 pasos; para un valor que no es límite de paso, la estimación sale de la densidad promedio (≈ 500,000 / 5,000 = 100).

## Notas personales

_Qué me costó, errores que cometí, dudas._
