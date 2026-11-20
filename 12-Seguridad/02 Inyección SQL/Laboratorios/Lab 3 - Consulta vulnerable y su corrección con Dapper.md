---
proyecto: Conocimiento
tipo: laboratorio
nivel: basico
progreso: pendiente
estado: borrador
area: Seguridad
herramienta: .NET SDK, Dapper, SQL Server Developer o Express (o LocalDB)
tags: [seguridad, laboratorio]
---
# Lab 3 - Consulta vulnerable y su corrección con Dapper

Parte de: [[12-Seguridad/02 Inyección SQL/_Índice Inyección SQL|Inyección SQL]]

**Cubre:** [[SQL injection]] · [[SQL injection - concatenación de cadenas, la causa raíz]] · [[SQL injection - consultas parametrizadas]] · [[SQL injection - Dapper y parámetros]] · [[SQL injection - listas blancas para ORDER BY y nombres de columnas]] · [[SQL injection - mínimo privilegio en la cuenta de base de datos]] · [[SQL injection - manejo de errores sin filtrar detalles]]

> [!info] Al terminar
> Si todos los criterios quedan marcados, cambia a `dominado` el `progreso` de este laboratorio y de las notas que cubre. Guarda tus archivos de práctica en esta misma carpeta.

> [!warning] Ética
> Haz esto solo en tu máquina, en tu propio código o en plataformas creadas para practicar. Probar sistemas ajenos sin autorización por escrito es ilegal. Ver [[Marco legal y autorización por escrito]].

## Objetivo
En tu propia base local, escribir a propósito un endpoint que concatena SQL, demostrar con una prueba automática que una comilla rompe la consulta, y corregirlo con parámetros de Dapper, lista blanca para el orden y un usuario de base con mínimo privilegio.

## Pasos
1. Crea la base `LabSqli` con una tabla `Productos(Id int identity, Nombre nvarchar(100), Precio decimal(10,2), Oculto bit)` y unos 10 registros, 3 con `Oculto = 1`.
2. Crea una minimal API con Dapper y un endpoint `GET /productos?nombre=&orden=` que **concatene** `nombre` en el `WHERE` (con `Oculto = 0`) y `orden` en el `ORDER BY`.
3. Escribe una prueba (xUnit o un archivo `.http`) que envíe un nombre con una comilla simple y observa que la API responde 500 con el error de SQL visible. Eso prueba que el valor del usuario está alterando la sintaxis. No necesitas construir un ataque.
4. Corrige el `WHERE` con un parámetro de Dapper.
5. Corrige el `ORDER BY` con una lista blanca.
6. Configura el manejo de errores para que fuera de Development se devuelva un ProblemDetails genérico.
7. Crea un usuario SQL `lab_api` con solo `SELECT` sobre `Productos` y cambia la cadena de conexión a ese usuario.

## Criterios de terminado
- [ ] Con el código vulnerable, un nombre con comilla produce un error de sintaxis SQL (anotado en notas personales).
- [ ] Con el código corregido, el mismo nombre devuelve 200 y lista vacía.
- [ ] `orden=lo-que-sea` usa el orden por defecto y nunca llega al SQL.
- [ ] Ninguna respuesta contiene texto de error de SQL Server en Production.
- [ ] `lab_api` no puede ejecutar `DELETE FROM Productos` (lo pruebas en SSMS con `EXECUTE AS USER`).
- [ ] Los 3 productos ocultos nunca aparecen en ninguna respuesta.

## Solución

> [!success]- Código vulnerable (solo para el laboratorio)
> ```csharp
> app.MapGet("/productos", async (string? nombre, string? orden, SqlConnection cn) =>
> {
>     var sql = $"SELECT Id, Nombre, Precio FROM Productos WHERE Oculto = 0 AND Nombre LIKE '%{nombre}%' ORDER BY {orden ?? "Id"}";
>     return await cn.QueryAsync(sql); // NO hagas esto en código real
> });
> ```

> [!success]- Código corregido
> ```csharp
> var columnas = new Dictionary<string, string>(StringComparer.OrdinalIgnoreCase)
> { ["nombre"] = "Nombre", ["precio"] = "Precio", ["id"] = "Id" };
>
> app.MapGet("/productos", async (string? nombre, string? orden, SqlConnection cn) =>
> {
>     var col = columnas.GetValueOrDefault(orden ?? "", "Id");
>     var sql = $"SELECT Id, Nombre, Precio FROM Productos WHERE Oculto = 0 AND Nombre LIKE @patron ORDER BY {col}";
>     return await cn.QueryAsync(sql, new { patron = $"%{nombre}%" });
> });
> ```
> Solo se interpola `col`, que viene de la lista blanca.

> [!success]- Errores genéricos
> ```csharp
> builder.Services.AddProblemDetails();
> if (!app.Environment.IsDevelopment()) app.UseExceptionHandler();
> ```

> [!success]- Usuario con mínimo privilegio
> ```sql
> CREATE LOGIN lab_api WITH PASSWORD = 'UnaContraseñaLarga-SoloLocal1';
> USE LabSqli;
> CREATE USER lab_api FOR LOGIN lab_api;
> GRANT SELECT ON dbo.Productos TO lab_api;
>
> EXECUTE AS USER = 'lab_api';
> DELETE FROM dbo.Productos WHERE Id = 1; -- debe fallar con error de permisos
> REVERT;
> ```

## Notas personales

_Qué me costó, errores que cometí, dudas._
