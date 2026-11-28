---
proyecto: Conocimiento
tipo: laboratorio
nivel: basico
progreso: pendiente
estado: borrador
area: SQL Server
herramienta: SQL Server Developer o Express + SSMS (o VS Code con la extensión MSSQL)
tags: [sql-server, laboratorio]
contenido: completo
---
# Lab 1 - Base de práctica Tienda en SQL Server

Parte de: [[06-SQL-Server/01 Básico/_Índice Básico|Básico]]

**Cubre:** [[SQL Server]] · [[Modelo relacional]] · [[Tipos de datos (varchar vs nvarchar, decimal vs float)]] · [[Esquemas (schemas) en SQL Server]] · [[CREATE, ALTER y DROP (DDL)]] · [[INSERT, UPDATE y DELETE (DML)]] · [[Claves primarias y foráneas]] · [[UNIQUE, CHECK y DEFAULT]] · [[IDENTITY vs SEQUENCE]] · [[Normalización]]

> [!info] Al terminar
> Si todos los criterios quedan marcados, cambia a `dominado` el `progreso` de este laboratorio y de las notas que cubre. Guarda tus archivos de práctica en esta misma carpeta.

## Objetivo

Crear la base de práctica **Tienda** que usan los ejercicios y laboratorios de SQL Server: dos esquemas (`ventas`, `rh`), cinco tablas con PK, FK, `UNIQUE`, `CHECK`, `DEFAULT`, `IDENTITY` y `rowversion`, y datos de ejemplo.

## Modelo

```
ventas.Clientes (ClienteId PK, Nombre, Ciudad NULL, FechaAlta)
ventas.Productos (ProductoId PK, Nombre UNIQUE, Categoria, Precio >= 0)
ventas.Pedidos (PedidoId PK, ClienteId FK, Fecha, Estado CHECK, RowVer rowversion)
ventas.PedidoDetalle (PedidoId FK + ProductoId FK = PK, Cantidad 1..100, PrecioUnitario)
rh.Empleados (EmpleadoId PK, Nombre, JefeId FK -> rh.Empleados, Puesto, Salario)
```

Datos: 6 clientes (Elena Soto sin ciudad, Fernando Gil sin pedidos), 6 productos (Webcam HD nunca vendida), 8 pedidos de enero a abril de 2025 (el 4 cancelado), 12 renglones y 7 empleados en 4 niveles.

## Pasos

1. Instala SQL Server 2022 Developer (o Express) y SSMS. Conéctate a `localhost` (o `localhost\SQLEXPRESS`) con autenticación de Windows.
2. Escribe tú el script `01_tienda.sql` a partir del modelo: crea la base, los esquemas y las tablas con restricciones **con nombre**. Usa `nvarchar` para textos de personas, `decimal(10,2)` para dinero y `date` para fechas.
3. Agrega índices en las columnas FK que no son el inicio de una PK (`Pedidos.ClienteId`, `PedidoDetalle.ProductoId`).
4. Inserta los datos (compara con la solución para que los resultados de los ejercicios coincidan).
5. Comprueba las restricciones intentando insertar datos inválidos (ver criterios).
6. Guarda el script en esta carpeta: lo volverás a usar para recrear la base cuando quieras empezar de cero (`DROP DATABASE Tienda;`).

## Criterios de terminado

- [ ] `SELECT COUNT(*)` devuelve 6 clientes, 6 productos, 8 pedidos, 12 renglones y 7 empleados.
- [ ] `INSERT INTO ventas.Productos (Nombre, Categoria, Precio) VALUES (N'Cable HDMI', N'Accesorios', 10);` falla con el error 2627 (UNIQUE).
- [ ] Insertar un pedido con `ClienteId = 99` falla con el error 547 (FK).
- [ ] Insertar un renglón con `Cantidad = 0` o `Cantidad = 500` falla con el error 547 (CHECK).
- [ ] `INSERT INTO ventas.Pedidos (ClienteId, Fecha) VALUES (6, '2025-05-01');` deja `Estado = 'Pendiente'` y un `RowVer` distinto de cero (luego bórralo).
- [ ] `SELECT name, is_not_trusted FROM sys.foreign_keys;` muestra todas las FK con `is_not_trusted = 0`.
- [ ] Puedo explicar por qué `PedidoDetalle` guarda `PrecioUnitario` aunque `Productos` ya tenga `Precio`.

## Solución

> [!success]- Script de estructura
> ```sql
> CREATE DATABASE Tienda;
> GO
> USE Tienda;
> GO
> CREATE SCHEMA ventas;
> GO
> CREATE SCHEMA rh;
> GO
> CREATE TABLE ventas.Clientes (
>     ClienteId int IDENTITY(1,1) NOT NULL CONSTRAINT PK_Clientes PRIMARY KEY,
>     Nombre    nvarchar(100) NOT NULL,
>     Ciudad    nvarchar(60)  NULL,
>     FechaAlta date NOT NULL CONSTRAINT DF_Clientes_FechaAlta DEFAULT (CAST(SYSUTCDATETIME() AS date))
> );
> CREATE TABLE ventas.Productos (
>     ProductoId int IDENTITY(1,1) NOT NULL CONSTRAINT PK_Productos PRIMARY KEY,
>     Nombre     nvarchar(100) NOT NULL CONSTRAINT UQ_Productos_Nombre UNIQUE,
>     Categoria  nvarchar(50)  NOT NULL,
>     Precio     decimal(10,2) NOT NULL CONSTRAINT CK_Productos_Precio CHECK (Precio >= 0)
> );
> CREATE TABLE ventas.Pedidos (
>     PedidoId  int IDENTITY(1,1) NOT NULL CONSTRAINT PK_Pedidos PRIMARY KEY,
>     ClienteId int  NOT NULL CONSTRAINT FK_Pedidos_Clientes REFERENCES ventas.Clientes (ClienteId),
>     Fecha     date NOT NULL,
>     Estado    varchar(20) NOT NULL
>               CONSTRAINT DF_Pedidos_Estado DEFAULT ('Pendiente')
>               CONSTRAINT CK_Pedidos_Estado CHECK (Estado IN ('Pendiente','Enviado','Entregado','Cancelado')),
>     RowVer    rowversion NOT NULL
> );
> CREATE TABLE ventas.PedidoDetalle (
>     PedidoId       int NOT NULL CONSTRAINT FK_Detalle_Pedidos REFERENCES ventas.Pedidos (PedidoId) ON DELETE CASCADE,
>     ProductoId     int NOT NULL CONSTRAINT FK_Detalle_Productos REFERENCES ventas.Productos (ProductoId),
>     Cantidad       int NOT NULL CONSTRAINT CK_Detalle_Cantidad CHECK (Cantidad BETWEEN 1 AND 100),
>     PrecioUnitario decimal(10,2) NOT NULL,
>     CONSTRAINT PK_PedidoDetalle PRIMARY KEY (PedidoId, ProductoId)
> );
> CREATE INDEX IX_Pedidos_ClienteId ON ventas.Pedidos (ClienteId);
> CREATE INDEX IX_PedidoDetalle_ProductoId ON ventas.PedidoDetalle (ProductoId);
> CREATE TABLE rh.Empleados (
>     EmpleadoId int NOT NULL CONSTRAINT PK_Empleados PRIMARY KEY,
>     Nombre     nvarchar(100) NOT NULL,
>     JefeId     int NULL CONSTRAINT FK_Empleados_Jefe REFERENCES rh.Empleados (EmpleadoId),
>     Puesto     nvarchar(50)  NOT NULL,
>     Salario    decimal(10,2) NOT NULL
> );
> GO
> ```

> [!success]- Script de datos
> ```sql
> -- Ids explícitos para que coincidan con los ejercicios (el IDENTITY continúa después del mayor)
> SET IDENTITY_INSERT ventas.Clientes ON;
> INSERT INTO ventas.Clientes (ClienteId, Nombre, Ciudad, FechaAlta) VALUES
>   (1, N'Ana López', N'Guadalajara', '2024-01-15'),
>   (2, N'Bruno Díaz', N'Monterrey', '2024-03-02'),
>   (3, N'Carla Méndez', N'Guadalajara', '2024-06-20'),
>   (4, N'Diego Ruiz', N'CDMX', '2025-01-10'),
>   (5, N'Elena Soto', NULL, '2025-02-01'),
>   (6, N'Fernando Gil', N'CDMX', '2025-05-05');
> SET IDENTITY_INSERT ventas.Clientes OFF;
>
> SET IDENTITY_INSERT ventas.Productos ON;
> INSERT INTO ventas.Productos (ProductoId, Nombre, Categoria, Precio) VALUES
>   (1, N'Teclado mecánico', N'Periféricos', 1200.00),
>   (2, N'Mouse inalámbrico', N'Periféricos', 450.00),
>   (3, N'Monitor 27', N'Monitores', 5200.00),
>   (4, N'Laptop 14', N'Computadoras', 18500.00),
>   (5, N'Cable HDMI', N'Accesorios', 150.00),
>   (6, N'Webcam HD', N'Periféricos', 900.00);
> SET IDENTITY_INSERT ventas.Productos OFF;
>
> SET IDENTITY_INSERT ventas.Pedidos ON;
> INSERT INTO ventas.Pedidos (PedidoId, ClienteId, Fecha, Estado) VALUES
>   (1, 1, '2025-01-05', 'Entregado'),
>   (2, 2, '2025-01-18', 'Entregado'),
>   (3, 1, '2025-02-10', 'Entregado'),
>   (4, 3, '2025-02-14', 'Cancelado'),
>   (5, 4, '2025-03-01', 'Entregado'),
>   (6, 2, '2025-03-15', 'Enviado'),
>   (7, 1, '2025-03-20', 'Pendiente'),
>   (8, 5, '2025-04-02', 'Entregado');
> SET IDENTITY_INSERT ventas.Pedidos OFF;
>
> INSERT INTO ventas.PedidoDetalle (PedidoId, ProductoId, Cantidad, PrecioUnitario) VALUES
>   (1, 1, 1, 1200.00),
>   (1, 2, 2, 450.00),
>   (2, 4, 1, 18500.00),
>   (3, 3, 1, 5200.00),
>   (3, 5, 3, 150.00),
>   (4, 1, 1, 1200.00),
>   (5, 4, 1, 18000.00),
>   (5, 2, 1, 450.00),
>   (6, 1, 2, 1150.00),
>   (6, 5, 2, 150.00),
>   (7, 2, 1, 450.00),
>   (8, 3, 2, 5000.00);
>
> INSERT INTO rh.Empleados (EmpleadoId, Nombre, JefeId, Puesto, Salario) VALUES
>   (1, N'Laura Torres', NULL, N'Directora', 90000.00),
>   (2, N'Mario Vega', 1, N'Gerente TI', 60000.00),
>   (3, N'Sofía Ramos', 1, N'Gerente Ventas', 58000.00),
>   (4, N'Iván Cruz', 2, N'Desarrollador', 40000.00),
>   (5, N'Paula Núñez', 2, N'Desarrolladora', 42000.00),
>   (6, N'Raúl Ortiz', 3, N'Vendedor', 30000.00),
>   (7, N'Tania Flores', 4, N'Practicante', 15000.00);
> GO
> ```

> [!success]- Comprobación rápida
> ```sql
> SELECT (SELECT COUNT(*) FROM ventas.Clientes)      AS Clientes,
>        (SELECT COUNT(*) FROM ventas.Productos)     AS Productos,
>        (SELECT COUNT(*) FROM ventas.Pedidos)       AS Pedidos,
>        (SELECT COUNT(*) FROM ventas.PedidoDetalle) AS Renglones,
>        (SELECT COUNT(*) FROM rh.Empleados)         AS Empleados;  -- 6, 6, 8, 12, 7
> ```
>
> `PrecioUnitario` es el precio pactado en la venta; `Productos.Precio` es el precio de lista actual. Si cambias el precio de lista, los pedidos viejos no deben cambiar.

## Notas personales

_Qué me costó, errores que cometí, dudas._
