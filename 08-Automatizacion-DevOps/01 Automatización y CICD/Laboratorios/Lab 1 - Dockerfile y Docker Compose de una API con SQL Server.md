---
proyecto: Conocimiento
tipo: laboratorio
nivel: intermedio
progreso: pendiente
estado: borrador
area: Automatización, DevOps y operación
herramienta: Docker Desktop, .NET SDK y SQL Server en contenedor
tags: [automatizacion-devops, laboratorio]
contenido: completo
---
# Lab 1 - Dockerfile y Docker Compose de una API con SQL Server

Parte de: [[08-Automatizacion-DevOps/01 Automatización y CICD/_Índice Automatización y CICD|Automatización y CICD]]

**Cubre:** [[Docker]] · [[Docker Compose]]

> [!info] Al terminar
> Si todos los criterios quedan marcados, cambia a `dominado` el `progreso` de este laboratorio y de las notas que cubre. Guarda tus archivos de práctica en esta misma carpeta.

## Objetivo

Contenerizar una API .NET con un Dockerfile multi-etapa y levantarla junto con SQL Server usando Compose: red por nombre de servicio, healthcheck, volumen persistente y secretos fuera del archivo.

## Pasos

1. Crea una API (`dotnet new webapi -n LabApi`), agrega el paquete `Microsoft.Data.SqlClient` y dos endpoints: `GET /health` y `GET /db` que ejecute `SELECT @@VERSION` con la cadena `ConnectionStrings:Default`.
2. Escribe un `.dockerignore` (bin, obj) y un `Dockerfile` multi-etapa: SDK para publicar, `aspnet` para correr, usuario no root, puerto 8080.
3. Construye la imagen y córrela sola publicando el puerto 5000 → 8080; prueba `/health`.
4. Crea un archivo `.env` con `SA_PASSWORD` (contraseña fuerte) y agrégalo a `.gitignore`.
5. Escribe `compose.yaml` con los servicios `db` (SQL Server 2022, volumen `sqldata`, healthcheck con `sqlcmd`) y `api` (build local, cadena apuntando a `db`, `depends_on` con `service_healthy`).
6. Levanta todo con `docker compose up -d --build` y prueba `/db`.
7. Crea una base `LabDb` con `sqlcmd` dentro del contenedor, haz `docker compose down` y `up -d` y comprueba que la base sigue ahí.
8. Ahora `docker compose down -v`, vuelve a levantar y observa la diferencia.
9. Compara el tamaño de la imagen final con la del SDK (`docker images`).

## Criterios de terminado

- [ ] `curl http://localhost:5000/health` devuelve `ok` desde el contenedor.
- [ ] `curl http://localhost:5000/db` devuelve la versión de SQL Server.
- [ ] `docker compose ps` muestra `db` como *healthy* antes de que arranque `api`.
- [ ] `LabDb` sobrevive a `down`/`up` y desaparece con `down -v`.
- [ ] La contraseña no aparece en `compose.yaml` ni en Git (`git status` no muestra `.env`).
- [ ] La imagen final es varias veces más pequeña que la del SDK.

## Solución

> [!success]- Solución: Program.cs (antes de app.Run())
> ```csharp
> using Microsoft.Data.SqlClient;
>
> app.MapGet("/health", () => "ok");
> app.MapGet("/db", async (IConfiguration cfg) =>
> {
>     await using var cn = new SqlConnection(cfg.GetConnectionString("Default"));
>     await cn.OpenAsync();
>     await using var cmd = new SqlCommand("SELECT @@VERSION", cn);
>     return (string?)await cmd.ExecuteScalarAsync();
> });
> ```

> [!success]- Solución: Dockerfile
> ```dockerfile
> FROM mcr.microsoft.com/dotnet/sdk:10.0 AS build
> WORKDIR /src
> COPY LabApi.csproj .
> RUN dotnet restore
> COPY . .
> RUN dotnet publish -c Release -o /app --no-restore
>
> FROM mcr.microsoft.com/dotnet/aspnet:10.0
> WORKDIR /app
> COPY --from=build /app .
> USER $APP_UID
> EXPOSE 8080
> ENTRYPOINT ["dotnet", "LabApi.dll"]
> ```
> Usa la etiqueta de la versión de .NET que tengas instalada. Prueba sola: `docker build -t labapi . && docker run --rm -p 5000:8080 labapi`.

> [!success]- Solución: compose.yaml
> ```yaml
> services:
>   db:
>     image: mcr.microsoft.com/mssql/server:2022-latest
>     environment:
>       ACCEPT_EULA: "Y"
>       MSSQL_SA_PASSWORD: ${SA_PASSWORD}
>     ports:
>       - "1433:1433"
>     volumes:
>       - sqldata:/var/opt/mssql
>     healthcheck:
>       test: ["CMD-SHELL", "/opt/mssql-tools18/bin/sqlcmd -S localhost -U sa -P \"$$MSSQL_SA_PASSWORD\" -C -Q 'SELECT 1' || exit 1"]
>       interval: 10s
>       retries: 10
>       start_period: 20s
>   api:
>     build: .
>     ports:
>       - "5000:8080"
>     environment:
>       ConnectionStrings__Default: "Server=db,1433;Database=master;User Id=sa;Password=${SA_PASSWORD};TrustServerCertificate=True"
>     depends_on:
>       db:
>         condition: service_healthy
> volumes:
>   sqldata:
> ```
> `${SA_PASSWORD}` lo toma Compose del `.env`; `$$` deja pasar un `$` literal al contenedor.

> [!success]- Solución: persistencia
> ```bash
> docker compose exec db /opt/mssql-tools18/bin/sqlcmd -S localhost -U sa -P "$SA_PASSWORD" -C -Q "CREATE DATABASE LabDb"
> docker compose down && docker compose up -d
> docker compose exec db /opt/mssql-tools18/bin/sqlcmd -S localhost -U sa -P "$SA_PASSWORD" -C -Q "SELECT name FROM sys.databases"
> docker compose down -v   # borra el volumen: LabDb desaparece
> ```
> (En PowerShell, carga la variable con el valor del `.env` o escríbela en el comando.)

## Notas personales

_Qué me costó, errores que cometí, dudas._
