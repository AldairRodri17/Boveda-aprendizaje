---
proyecto: Conocimiento
tipo: laboratorio
nivel: basico
progreso: pendiente
estado: borrador
area: Seguridad
herramienta: .NET SDK, dotnet user-secrets, variables de entorno
tags: [seguridad, laboratorio]
contenido: completo
---
# Lab 1 - Sacar secretos del código en una API .NET

Parte de: [[12-Seguridad/01 Fundamentos de seguridad/_Índice Fundamentos de seguridad|Fundamentos de seguridad]]

**Cubre:** [[Manejo de secretos]] · [[Logs y datos sensibles]] · [[Secretos en repositorios]]

> [!info] Al terminar
> Si todos los criterios quedan marcados, cambia a `dominado` el `progreso` de este laboratorio y de las notas que cubre. Guarda tus archivos de práctica en esta misma carpeta.

## Objetivo
Tomar una API mínima con la cadena de conexión y una llave de API escritas en `appsettings.json`, moverlas a User Secrets en desarrollo y a variables de entorno para "producción", y evitar que se impriman en los logs.

## Pasos
1. Crea la API: `dotnet new webapi -n LabSecretos` y agrega en `appsettings.json` una sección `Proveedor:ApiKey` con un valor inventado (`clave-de-prueba-123`).
2. Crea un endpoint `GET /config-check` que devuelva solo si la llave está configurada (`true/false`), **no** su valor.
3. Inicializa User Secrets (`dotnet user-secrets init`) y guarda ahí la llave. Borra el valor de `appsettings.json` (deja la clave vacía).
4. Ejecuta y confirma que `/config-check` devuelve `true`.
5. Simula producción: ejecuta con `ASPNETCORE_ENVIRONMENT=Production` y la llave en una variable de entorno con la convención de doble guion bajo.
6. Agrega un log que registre la petición entrante y verifica que **no** salga el header `Authorization` (usa `AddHttpLogging` con los campos mínimos o un log manual que excluya headers).
7. Agrega al `.gitignore` cualquier archivo local de configuración y revisa con `git status` que no haya secretos por subir.

## Criterios de terminado
- [ ] `appsettings.json` no contiene ningún valor secreto (solo claves vacías o marcadores).
- [ ] En Development la llave se lee desde User Secrets; en Production, desde la variable de entorno.
- [ ] `/config-check` responde `{"configurada": true}` en ambos entornos sin mostrar el valor.
- [ ] Enviando `Authorization: Bearer prueba` el valor no aparece en la consola.
- [ ] Sabes explicar dónde guarda User Secrets los valores y por qué no son cifrado.

## Solución

> [!success]- Comandos de User Secrets
> ```powershell
> dotnet user-secrets init
> dotnet user-secrets set "Proveedor:ApiKey" "clave-de-prueba-123"
> dotnet user-secrets list
> ```
> Los valores quedan en `%APPDATA%\Microsoft\UserSecrets\<UserSecretsId>\secrets.json`, fuera del repositorio. **No están cifrados**: solo evitan que lleguen a Git.

> [!success]- Variable de entorno para Production
> ```powershell
> $env:ASPNETCORE_ENVIRONMENT = "Production"
> $env:Proveedor__ApiKey = "clave-de-prueba-123"
> dotnet run
> ```
> El doble guion bajo `__` equivale a `:` en la jerarquía de configuración.

> [!success]- Endpoint que no expone el valor
> ```csharp
> app.MapGet("/config-check", (IConfiguration cfg) =>
>     Results.Ok(new { configurada = !string.IsNullOrWhiteSpace(cfg["Proveedor:ApiKey"]) }));
> ```

> [!success]- Logging sin headers sensibles
> ```csharp
> builder.Services.AddHttpLogging(o =>
> {
>     o.LoggingFields = HttpLoggingFields.RequestMethod | HttpLoggingFields.RequestPath
>                     | HttpLoggingFields.ResponseStatusCode;
> });
> app.UseHttpLogging();
> ```
> Si en algún momento incluyes headers, `Authorization` y `Cookie` se muestran como `[Redacted]` salvo que los agregues a la lista permitida; no lo hagas.

## Notas personales

_Qué me costó, errores que cometí, dudas._
