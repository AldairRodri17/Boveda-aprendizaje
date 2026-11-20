---
proyecto: Conocimiento
tipo: laboratorio
nivel: intermedio
progreso: pendiente
estado: borrador
area: SQL Server
herramienta: SQL Server Developer o Express + SSMS
tags: [sql-server, laboratorio]
---
# Lab 10 - Usuario de aplicación con permisos mínimos por esquema

Parte de: [[06-SQL-Server/05 Administración y avanzado/_Índice Administración y avanzado|Administración y avanzado]]

**Cubre:** [[Roles y permisos]] · [[Esquemas (schemas) en SQL Server]] · [[Stored procedures]] · [[SQL dinámico y sp_executesql]] · [[Row-level security]]

> [!info] Al terminar
> Si todos los criterios quedan marcados, cambia a `dominado` el `progreso` de este laboratorio y de las notas que cubre. Guarda tus archivos de práctica en esta misma carpeta.

## Objetivo

Crear la identidad con la que una app debería conectarse a la base Tienda ([[Lab 1 - Base de práctica Tienda en SQL Server]]): solo puede **ejecutar procedimientos** del esquema `ventas`, no leer ni modificar tablas directamente, y comprobar cómo el encadenamiento de propiedad y el SQL dinámico afectan esos permisos.

## Pasos

1. Crea un usuario de prueba **sin login** `app_tienda` y un rol `rol_app_ventas`.
2. Concede al rol `EXECUTE` sobre el esquema `ventas` y agrega el usuario al rol.
3. Suplanta al usuario con `EXECUTE AS USER = 'app_tienda';` y prueba:
   - `SELECT * FROM ventas.Clientes;`
   - `EXEC ventas.Productos_Listar;` (créalo si no existe; ver [[CREATE OR ALTER y scripts idempotentes]])
   - `DELETE FROM ventas.Pedidos;`
   Vuelve con `REVERT;`.
4. Crea `ventas.Clientes_Buscar @Ciudad nvarchar(60)` con SQL dinámico (`sp_executesql`) y pruébalo como `app_tienda`. Explica el resultado.
5. Arregla el paso 4 sin dar `SELECT` sobre la tabla (pista: `WITH EXECUTE AS OWNER` en el procedimiento) y vuelve a probar.
6. Revisa los permisos efectivos con `fn_my_permissions` mientras suplantas.
7. **Reto (RLS)**: con `SESSION_CONTEXT`, haz que `app_tienda` solo vea pedidos de la ciudad que fije la sesión.
8. Escribe en notas personales cómo sería en Azure SQL (usuario de identidad administrada con `FROM EXTERNAL PROVIDER`).

## Criterios de terminado

- [ ] Como `app_tienda`, el `SELECT` directo falla con `Msg 229` (permiso denegado) y el `DELETE` también.
- [ ] Como `app_tienda`, `EXEC ventas.Productos_Listar` funciona y devuelve los 6 productos.
- [ ] El procedimiento con SQL dinámico falla con `Msg 229` antes del arreglo y funciona después.
- [ ] `SELECT * FROM fn_my_permissions('ventas', 'SCHEMA');` como `app_tienda` muestra `EXECUTE`.
- [ ] Reto: con `SESSION_CONTEXT` en `N'CDMX'`, el procedimiento de pedidos solo devuelve el pedido 5.

## Solución

> [!success]- Usuario, rol y pruebas
> ```sql
> CREATE USER app_tienda WITHOUT LOGIN;
> CREATE ROLE rol_app_ventas;
> GRANT EXECUTE ON SCHEMA::ventas TO rol_app_ventas;
> ALTER ROLE rol_app_ventas ADD MEMBER app_tienda;
> GO
> EXECUTE AS USER = 'app_tienda';
> SELECT * FROM ventas.Clientes;           -- Msg 229: The SELECT permission was denied...
> EXEC ventas.Productos_Listar;            -- OK por encadenamiento de propiedad
> SELECT * FROM fn_my_permissions('ventas', 'SCHEMA');
> REVERT;
> ```
>
> Para una app real: `CREATE LOGIN app_tienda WITH PASSWORD = '...'; CREATE USER app_tienda FOR LOGIN app_tienda;` o, en Azure SQL, `CREATE USER [mi-app] FROM EXTERNAL PROVIDER;`.

> [!success]- SQL dinámico y EXECUTE AS OWNER
> ```sql
> CREATE OR ALTER PROCEDURE ventas.Clientes_Buscar @Ciudad nvarchar(60)
> WITH EXECUTE AS OWNER          -- quítalo para ver el Msg 229
> AS
> BEGIN
>     SET NOCOUNT ON;
>     EXEC sp_executesql N'SELECT ClienteId, Nombre FROM ventas.Clientes WHERE Ciudad = @Ciudad',
>                        N'@Ciudad nvarchar(60)', @Ciudad;
> END
> GO
> ```
>
> El SQL dinámico rompe el encadenamiento de propiedad: se revisan los permisos de quien ejecuta. `EXECUTE AS OWNER` hace que el procedimiento corra con el contexto del dueño. Alternativa más fina: firmar el procedimiento con un certificado.

> [!success]- Reto: RLS por ciudad
> ```sql
> CREATE SCHEMA seg;
> GO
> CREATE FUNCTION seg.fn_PedidoCiudad (@ClienteId int)
> RETURNS TABLE WITH SCHEMABINDING
> AS RETURN
>     SELECT 1 AS ok
>     FROM   ventas.Clientes c
>     WHERE  c.ClienteId = @ClienteId
>       AND (c.Ciudad = CAST(SESSION_CONTEXT(N'Ciudad') AS nvarchar(60))
>            OR IS_ROLEMEMBER('db_owner') = 1);
> GO
> CREATE SECURITY POLICY seg.PoliticaPedidos
>     ADD FILTER PREDICATE seg.fn_PedidoCiudad(ClienteId) ON ventas.Pedidos
>     WITH (STATE = ON);
> GO
> EXECUTE AS USER = 'app_tienda';
> EXEC sp_set_session_context @key = N'Ciudad', @value = N'CDMX';
> -- un procedimiento ventas.Pedidos_Listar que haga SELECT * FROM ventas.Pedidos devuelve solo el 5
> REVERT;
> ```
>
> Desactiva la política al terminar (`ALTER SECURITY POLICY seg.PoliticaPedidos WITH (STATE = OFF);`) para que no afecte a los otros laboratorios.

## Notas personales

_Qué me costó, errores que cometí, dudas._
