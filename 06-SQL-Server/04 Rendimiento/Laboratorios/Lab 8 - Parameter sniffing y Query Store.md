---
proyecto: Conocimiento
tipo: laboratorio
nivel: avanzado
progreso: pendiente
estado: borrador
area: SQL Server
herramienta: SQL Server 2022 Developer + SSMS (informes de Query Store)
tags: [sql-server, laboratorio]
contenido: completo
---
# Lab 8 - Parameter sniffing y Query Store

Parte de: [[06-SQL-Server/04 Rendimiento/_Índice Rendimiento|Rendimiento]]

**Cubre:** [[Parameter sniffing]] · [[Query Store]] · [[Planes de ejecución]] · [[Estadísticas]] · [[Nivel de compatibilidad de la base]] · [[DMVs]]

> [!info] Al terminar
> Si todos los criterios quedan marcados, cambia a `dominado` el `progreso` de este laboratorio y de las notas que cubre. Guarda tus archivos de práctica en esta misma carpeta.

## Objetivo

Provocar un problema de *parameter sniffing* con datos sesgados, detectarlo en Query Store y corregirlo de tres formas (recompilar, `OPTIMIZE FOR` y forzar plan). Requiere haber hecho [[Lab 7 - Índices y planes de ejecución antes y después]] (tabla `ventas.Movimientos`).

## Pasos

1. Sesga los datos: agrega 200,000 movimientos del cliente 1. Deja **solo** el índice `IX_Mov_Cliente (ClienteId)` sin `INCLUDE` (recréalo así).
2. Activa Query Store en Tienda. Si tu base está en compatibilidad 160, desactiva temporalmente PSP para ver el problema clásico: `ALTER DATABASE SCOPED CONFIGURATION SET PARAMETER_SENSITIVE_PLAN_OPTIMIZATION = OFF;`
3. Crea `ventas.Movimientos_PorCliente @ClienteId int` que devuelva `MovimientoId, Fecha, Monto, Referencia` del cliente.
4. Ejecuta primero con `@ClienteId = 42` (pocas filas) y luego con `1` (200 mil). Compara lecturas lógicas y el plan del segundo.
5. Limpia el plan de ese procedimiento con `sp_recompile` y ejecuta en orden inverso (primero 1, luego 42). Compara.
6. En SSMS abre **Query Store → Top Resource Consuming Queries** (o consulta las vistas `sys.query_store_*`) y localiza los dos planes del procedimiento.
7. Corrige de tres formas y mide cada una: `OPTION (RECOMPILE)`; `OPTIMIZE FOR (@ClienteId UNKNOWN)`; y forzar con `sp_query_store_force_plan` el plan que prefieras. Deshaz cada cambio antes del siguiente.
8. Vuelve a activar PSP (`= ON`) y repite el paso 4: anota si SQL Server generó variantes del plan.

## Criterios de terminado

- [ ] Con el plan de 42 (seek + lookup), la ejecución con 1 hace cientos de miles de lecturas lógicas (más que un scan de la tabla).
- [ ] En el orden inverso, el cliente 42 usa un *scan* completo aunque solo tiene ~100 filas.
- [ ] Encontré en Query Store el `query_id` del procedimiento con al menos 2 `plan_id`.
- [ ] Con `OPTION (RECOMPILE)` cada valor obtiene su plan adecuado (lo veo en el plan real).
- [ ] Forcé un plan y `sys.query_store_plan.is_forced_plan = 1`; luego lo deshice.
- [ ] Anoté qué solución elegiría para este caso y por qué.

## Solución

> [!success]- Datos sesgados, índice y procedimiento
> ```sql
> INSERT INTO ventas.Movimientos (ClienteId, Fecha, Monto, Referencia)
> SELECT TOP (200000) 1,
>        DATEADD(minute, ABS(CHECKSUM(NEWID())) % 525600, '2025-01-01'),
>        CAST(ABS(CHECKSUM(NEWID())) % 100000 / 100.0 AS decimal(10,2)),
>        CONCAT('RX-', ROW_NUMBER() OVER (ORDER BY (SELECT NULL)))
> FROM   sys.all_objects a CROSS JOIN sys.all_objects b;
>
> CREATE INDEX IX_Mov_Cliente ON ventas.Movimientos (ClienteId) WITH (DROP_EXISTING = ON);
> UPDATE STATISTICS ventas.Movimientos WITH FULLSCAN;
>
> ALTER DATABASE Tienda SET QUERY_STORE = ON (OPERATION_MODE = READ_WRITE);
> GO
> CREATE OR ALTER PROCEDURE ventas.Movimientos_PorCliente @ClienteId int
> AS
> BEGIN
>     SET NOCOUNT ON;
>     SELECT MovimientoId, Fecha, Monto, Referencia
>     FROM   ventas.Movimientos
>     WHERE  ClienteId = @ClienteId;
> END
> GO
> SET STATISTICS IO ON;
> EXEC ventas.Movimientos_PorCliente 42;
> EXEC ventas.Movimientos_PorCliente 1;
> EXEC sp_recompile N'ventas.Movimientos_PorCliente';
> EXEC ventas.Movimientos_PorCliente 1;
> EXEC ventas.Movimientos_PorCliente 42;
> ```

> [!success]- Query Store
> ```sql
> SELECT q.query_id, p.plan_id, p.is_forced_plan, rs.count_executions, rs.avg_logical_io_reads
> FROM   sys.query_store_query q
> JOIN   sys.query_store_plan p          ON p.query_id = q.query_id
> JOIN   sys.query_store_runtime_stats rs ON rs.plan_id = p.plan_id
> WHERE  q.object_id = OBJECT_ID('ventas.Movimientos_PorCliente');
>
> EXEC sp_query_store_force_plan   @query_id = 1, @plan_id = 1;   -- usa tus ids
> EXEC sp_query_store_unforce_plan @query_id = 1, @plan_id = 1;
> ```
>
> Las estadísticas de Query Store se escriben en la base cada cierto tiempo; si no ves datos aún, espera unos minutos o ejecuta `EXEC sp_query_store_flush_db;`.

> [!success]- Correcciones
> ```sql
> -- 1) plan por ejecución
> SELECT ... WHERE ClienteId = @ClienteId OPTION (RECOMPILE);
> -- 2) plan "promedio" (usa la densidad, no el valor olfateado)
> SELECT ... WHERE ClienteId = @ClienteId OPTION (OPTIMIZE FOR (@ClienteId UNKNOWN));
> ```
>
> `RECOMPILE` da el mejor plan para cada valor a costa de CPU por compilación: bueno si el procedimiento se llama poco. `OPTIMIZE FOR UNKNOWN` da un plan estable pero mediocre para los extremos. Forzar plan en Query Store no requiere cambiar código. Con PSP (SQL Server 2022, compatibilidad 160) el motor puede mantener varios planes según el rango del valor.

## Notas personales

_Qué me costó, errores que cometí, dudas._
