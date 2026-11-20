---
proyecto: Conocimiento
tipo: laboratorio
nivel: intermedio
progreso: pendiente
estado: borrador
area: SQL Server
herramienta: SQL Server Developer o Express, sqlcmd y PowerShell (módulo SqlServer opcional)
tags: [sql-server, laboratorio]
---
# Lab 9 - Script de migración idempotente con tabla de versiones

Parte de: [[06-SQL-Server/05 Administración y avanzado/_Índice Administración y avanzado|Administración y avanzado]]

**Cubre:** [[Migraciones de base de datos (scripts versionados, DACPAC, DbUp y Flyway)]] · [[CREATE OR ALTER y scripts idempotentes]] · [[Lotes con GO, sqlcmd e Invoke-Sqlcmd]] · [[Migraciones sin caídas (expand y contract)]] · [[Esquemas (schemas) en SQL Server]] · [[Tablas temporales del sistema (temporal tables)]]

> [!info] Al terminar
> Si todos los criterios quedan marcados, cambia a `dominado` el `progreso` de este laboratorio y de las notas que cubre. Guarda tus archivos de práctica en esta misma carpeta.

## Objetivo

Construir un mini sistema de migraciones como el que usas en tus proyectos: scripts `V1__`, `V2__`… idempotentes, una tabla de versiones y un runner en PowerShell que se pueda ejecutar dos veces sin errores. Usa la base Tienda ([[Lab 1 - Base de práctica Tienda en SQL Server]]).

## Pasos

1. Crea la carpeta `migraciones` en esta carpeta del laboratorio.
2. `V0__tabla_versiones.sql`: crea, si no existe, `dbo.SchemaVersions (Version int PK, Descripcion nvarchar(200), AplicadoEn datetime2(0) DEFAULT SYSUTCDATETIME())`.
3. `V1__catalogo_activo.sql` (fase *expand*): agrega `Activo bit NOT NULL DEFAULT 1` a `ventas.Productos`, crea un índice filtrado de productos activos y el procedimiento `ventas.Productos_ListarActivos` con `CREATE OR ALTER`. Registra la versión 1 solo si no existe.
4. `V2__productos_temporal.sql`: convierte `ventas.Productos` en tabla temporal del sistema con historial `ventas.ProductosHistorial`, solo si aún no lo es. Registra la versión 2.
5. Escribe `aplicar.ps1`: ordena los archivos por el número de versión (como número, para que `V10` vaya después de `V2`), los ejecuta con `sqlcmd -b` y se detiene al primer error.
6. Ejecuta `aplicar.ps1` **dos veces** seguidas.
7. Simula un error: crea `V3__roto.sql` con una instrucción inválida y ejecuta el runner. Luego corrígelo (sin editar V1/V2) y vuelve a correr.
8. **Reto**: agrega una variable sqlcmd `$(Esquema)` en V1 y pásala desde el runner con `-v`.

## Criterios de terminado

- [ ] La segunda ejecución del runner termina sin errores ni cambios (todo «ya existe»).
- [ ] `SELECT * FROM dbo.SchemaVersions ORDER BY Version;` muestra 0, 1, 2 (y 3 tras corregir), una vez cada una.
- [ ] `SELECT temporal_type_desc FROM sys.tables WHERE name = 'Productos';` devuelve `SYSTEM_VERSIONED_TEMPORAL_TABLE`.
- [ ] Con `V3__roto.sql` el runner sale con código distinto de 0 y no ejecuta scripts posteriores.
- [ ] En V1, el `UPDATE`/uso de la columna nueva está en un lote separado (`GO`) de su `ALTER TABLE`.
- [ ] Puedo explicar por qué no se edita un script ya aplicado y qué haría Flyway si lo hiciera (checksum distinto).

## Solución

> [!success]- V0 y V1
> ```sql
> -- V0__tabla_versiones.sql
> IF OBJECT_ID('dbo.SchemaVersions', 'U') IS NULL
>     CREATE TABLE dbo.SchemaVersions (
>         Version     int           NOT NULL CONSTRAINT PK_SchemaVersions PRIMARY KEY,
>         Descripcion nvarchar(200) NOT NULL,
>         AplicadoEn  datetime2(0)  NOT NULL CONSTRAINT DF_SchemaVersions_AplicadoEn DEFAULT SYSUTCDATETIME()
>     );
> IF NOT EXISTS (SELECT 1 FROM dbo.SchemaVersions WHERE Version = 0)
>     INSERT INTO dbo.SchemaVersions (Version, Descripcion) VALUES (0, N'Tabla de versiones');
> GO
>
> -- V1__catalogo_activo.sql
> IF COL_LENGTH('ventas.Productos', 'Activo') IS NULL
>     ALTER TABLE ventas.Productos ADD Activo bit NOT NULL CONSTRAINT DF_Productos_Activo DEFAULT (1);
> GO
> IF NOT EXISTS (SELECT 1 FROM sys.indexes WHERE name = 'IX_Productos_Activos' AND object_id = OBJECT_ID('ventas.Productos'))
>     CREATE INDEX IX_Productos_Activos ON ventas.Productos (Categoria) INCLUDE (Nombre, Precio) WHERE Activo = 1;
> GO
> CREATE OR ALTER PROCEDURE ventas.Productos_ListarActivos
> AS
> BEGIN
>     SET NOCOUNT ON;
>     SELECT ProductoId, Nombre, Categoria, Precio FROM ventas.Productos WHERE Activo = 1;
> END
> GO
> IF NOT EXISTS (SELECT 1 FROM dbo.SchemaVersions WHERE Version = 1)
>     INSERT INTO dbo.SchemaVersions (Version, Descripcion) VALUES (1, N'Productos.Activo');
> GO
> ```

> [!success]- V2 (tabla temporal)
> ```sql
> IF NOT EXISTS (SELECT 1 FROM sys.tables WHERE object_id = OBJECT_ID('ventas.Productos') AND temporal_type = 2)
> BEGIN
>     IF COL_LENGTH('ventas.Productos', 'ValidFrom') IS NULL
>         ALTER TABLE ventas.Productos ADD
>             ValidFrom datetime2(0) GENERATED ALWAYS AS ROW START HIDDEN NOT NULL
>                 CONSTRAINT DF_Productos_ValidFrom DEFAULT SYSUTCDATETIME(),
>             ValidTo   datetime2(0) GENERATED ALWAYS AS ROW END HIDDEN NOT NULL
>                 CONSTRAINT DF_Productos_ValidTo DEFAULT CONVERT(datetime2(0), '9999-12-31 23:59:59'),
>             PERIOD FOR SYSTEM_TIME (ValidFrom, ValidTo);
>     ALTER TABLE ventas.Productos SET (SYSTEM_VERSIONING = ON (HISTORY_TABLE = ventas.ProductosHistorial));
> END
> GO
> IF NOT EXISTS (SELECT 1 FROM dbo.SchemaVersions WHERE Version = 2)
>     INSERT INTO dbo.SchemaVersions (Version, Descripcion) VALUES (2, N'Productos temporal');
> GO
> ```
>
> `temporal_type = 2` significa *system-versioned*. Prueba: cambia un precio y consulta `FOR SYSTEM_TIME ALL`.

> [!success]- aplicar.ps1
> ```powershell
> param(
>     [string]$Servidor = "localhost",
>     [string]$Base     = "Tienda"
> )
> $ErrorActionPreference = "Stop"
> $scripts = Get-ChildItem "$PSScriptRoot\migraciones\V*__*.sql" |
>     Sort-Object { [int]($_.BaseName -replace '^V(\d+)__.*$', '$1') }
>
> foreach ($s in $scripts) {
>     Write-Host "Aplicando $($s.Name)..."
>     sqlcmd -S $Servidor -d $Base -E -b -i $s.FullName   # agrega -C si falla por certificado
>     if ($LASTEXITCODE -ne 0) { throw "Falló $($s.Name) (código $LASTEXITCODE)" }
> }
> Write-Host "Listo."
> ```
>
> Con el módulo `SqlServer`: `Invoke-Sqlcmd -ServerInstance $Servidor -Database $Base -InputFile $s.FullName -ErrorAction Stop`. Reto: en V1 usa `$(Esquema).Productos` y llama `sqlcmd ... -v Esquema=ventas`.

## Notas personales

_Qué me costó, errores que cometí, dudas._
