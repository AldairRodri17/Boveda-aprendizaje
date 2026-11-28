---
proyecto: Conocimiento
tipo: laboratorio
nivel: intermedio
progreso: pendiente
estado: borrador
area: SQL Server
herramienta: SQL Server Developer o Express + SSMS (tres ventanas de consulta)
tags: [sql-server, laboratorio]
contenido: completo
---
# Lab 6 - Bloqueos, niveles de aislamiento y deadlock en dos sesiones

Parte de: [[06-SQL-Server/03 Transacciones y concurrencia/_Índice Transacciones y concurrencia|Transacciones y concurrencia]]

**Cubre:** [[Bloqueos (locks)]] · [[Niveles de aislamiento]] · [[READ UNCOMMITTED y NOLOCK]] · [[READ COMMITTED (nivel por defecto)]] · [[REPEATABLE READ]] · [[SERIALIZABLE y bloqueos de rango]] · [[Snapshot y RCSI]] · [[Deadlocks]] · [[Transacciones (BEGIN, COMMIT y ROLLBACK)]]

> [!info] Al terminar
> Si todos los criterios quedan marcados, cambia a `dominado` el `progreso` de este laboratorio y de las notas que cubre. Guarda tus archivos de práctica en esta misma carpeta.

## Objetivo

Ver con tus propios ojos un bloqueo, una lectura sucia, el efecto de RCSI, un fantasma y un deadlock, y diagnosticarlos con DMVs. Usa la base Tienda ([[Lab 1 - Base de práctica Tienda en SQL Server]]) en un SQL Server **local** (en Azure SQL Database RCSI ya viene activado y el paso 1 no bloquearía).

## Pasos

Abre tres ventanas: **A**, **B** y **C** (monitor). Ejecuta cada bloque en la ventana indicada y en orden.

1. **Bloqueo**. A: `BEGIN TRAN; UPDATE ventas.Productos SET Precio = 999 WHERE ProductoId = 1;` (sin `COMMIT`). B: `SELECT Precio FROM ventas.Productos WHERE ProductoId = 1;` → B se queda esperando. C: consulta `sys.dm_exec_requests` y `sys.dm_tran_locks` para ver quién bloquea a quién.
2. **Lectura sucia**. Cancela B. En B: `SELECT Precio FROM ventas.Productos WITH (NOLOCK) WHERE ProductoId = 1;` → ve 999. En A: `ROLLBACK`. El 999 que vio B nunca existió.
3. **RCSI**. Activa `READ_COMMITTED_SNAPSHOT` en la base y repite el paso 1: ahora B no espera y ve el último valor confirmado.
4. **Fantasma**. En A: `SET TRANSACTION ISOLATION LEVEL REPEATABLE READ; BEGIN TRAN; SELECT COUNT(*) FROM ventas.Productos WHERE Precio < 1000;`. En B inserta un producto de 100. En A repite el `SELECT` → el conteo cambió (fantasma). `ROLLBACK` en A. Repite con `SERIALIZABLE`: el `INSERT` de B espera.
5. **Deadlock**. A: `BEGIN TRAN; UPDATE ventas.Productos SET Precio = Precio WHERE ProductoId = 1;` B: `BEGIN TRAN; UPDATE ventas.Productos SET Precio = Precio WHERE ProductoId = 2;` A: actualiza el 2. B: actualiza el 1. Una de las dos recibe el error 1205.
6. **Diagnóstico**. Recupera el grafo del deadlock desde la sesión `system_health` y ábrelo en SSMS.
7. Limpieza: `ROLLBACK` donde haya transacciones abiertas, borra el producto de prueba y decide si dejas RCSI activado (anótalo).

## Criterios de terminado

- [ ] En el paso 1, C muestra a la sesión B con `blocking_session_id` = la sesión de A y espera `LCK_M_S`.
- [ ] En el paso 2, B leyó 999 con `NOLOCK` y después del `ROLLBACK` el precio sigue en 1200.00.
- [ ] Con RCSI activo (`is_read_committed_snapshot_on = 1`), B lee 1200.00 sin esperar mientras A tiene el `UPDATE` abierto.
- [ ] En `REPEATABLE READ` el segundo conteo cambia; en `SERIALIZABLE` el `INSERT` de B queda bloqueado hasta que A termina.
- [ ] En el paso 5 una sesión recibe `Msg 1205` y la otra continúa.
- [ ] Obtuve el XML `xml_deadlock_report` con las dos sesiones y los recursos (llaves de `PK_Productos`).

## Solución

> [!success]- Monitor (ventana C)
> ```sql
> SELECT r.session_id, r.blocking_session_id, r.wait_type, r.wait_time, t.text
> FROM   sys.dm_exec_requests r
> CROSS  APPLY sys.dm_exec_sql_text(r.sql_handle) t
> WHERE  r.blocking_session_id <> 0;
>
> SELECT request_session_id, resource_type, resource_description, request_mode, request_status
> FROM   sys.dm_tran_locks
> WHERE  resource_database_id = DB_ID('Tienda')
> ORDER  BY request_session_id;
> ```
>
> Verás un `X` concedido (GRANT) de A sobre la llave (`KEY`) y un `S` en espera (WAIT) de B, además de `IX` en página y tabla.

> [!success]- Activar RCSI
> ```sql
> ALTER DATABASE Tienda SET READ_COMMITTED_SNAPSHOT ON WITH ROLLBACK IMMEDIATE;
> SELECT is_read_committed_snapshot_on FROM sys.databases WHERE name = 'Tienda';
> ```
>
> Para desactivarlo: `SET READ_COMMITTED_SNAPSHOT OFF WITH ROLLBACK IMMEDIATE`. Con RCSI, la lectura de B ve la versión confirmada al inicio de su instrucción; los escritores siguen bloqueándose entre sí.

> [!success]- Grafo del deadlock
> ```sql
> SELECT CAST(event_data AS xml) AS Grafo
> FROM   sys.fn_xe_file_target_read_file('system_health*.xel', NULL, NULL, NULL)
> WHERE  object_name = 'xml_deadlock_report';
> ```
>
> Haz clic en el XML y guárdalo como `.xdl` para verlo gráfico. La prevención: actualizar siempre en el mismo orden (primero el 1 y luego el 2) y mantener las transacciones cortas. La víctima debe reintentar la transacción completa.

## Notas personales

_Qué me costó, errores que cometí, dudas._
