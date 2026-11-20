---
proyecto: Conocimiento
tipo: indice
estado: en-progreso
area: C# y .NET
tags: [indice, csharp-dotnet]
---
# ASP.NET Core

Parte de: [[03-CSharp-DotNet/_Índice CSharp y .NET|C# y .NET]] · Avance general en [[Temario]].

> [!info] Cómo avanzar
> Cada nota tiene **Autoevaluación** (preguntas con la respuesta plegada). Márcala `visto` cuando la contestes sin abrir las respuestas. Los laboratorios llevan las notas a `dominado`.

## Temas

- [[Pipeline y middleware]]
  - [[Orden de los middleware en ASP.NET Core]]
  - [[Middleware personalizado en ASP.NET Core]]
- [[Generic host y Kestrel]]
- [[Routing]]
- [[ASP.NET Core Controllers]]
- [[ASP.NET Core Minimal APIs]]
- [[Model binding y validación]]
- [[Filtros]]
- [[Inyección de dependencias]]
- [[Ciclos de vida (Singleton, Scoped, Transient)]]
  - [[Servicios Singleton en DI]]
  - [[Servicios Scoped en DI]]
  - [[Servicios Transient en DI]]
  - [[Captive dependency]]
- [[Keyed services]]
- [[Configuración y appsettings]]
- [[Options pattern]]
- [[Secretos con User Secrets]]
- [[Logging con ILogger]]
- [[Serilog]]
- [[Manejo global de errores (IExceptionHandler)]]
- [[CORS en ASP.NET Core]]
- [[Autenticación con JWT en ASP.NET Core]]
- [[Autorización con policies]]
- [[Data Protection]]
- [[Versionado de API en ASP.NET Core]]
- [[Microsoft.AspNetCore.OpenApi vs Swashbuckle]]
- [[Health checks]]
- [[Output caching]]
- [[Caché en memoria, distribuida y HybridCache]]
  - [[IMemoryCache]]
  - [[IDistributedCache]]
  - [[HybridCache]]
- [[BackgroundService]]
- [[HttpClient e IHttpClientFactory]]
- [[Flurl]]
- [[Resiliencia con Polly]]
- [[MediatR]]
- [[FluentValidation]]
- [[AutoMapper y alternativas]]
- [[Subida y descarga de archivos]]
- [[Publicación y despliegue de .NET]]
- [[Publicar en IIS (FTP y FileZilla)]]
- [[Dockerfile para .NET]]
- [[Native AOT y rendimiento]]
- [[Directory.Build.props y Central Package Management]]
- [[Analizadores de Roslyn y warnings como errores]]
- [[Crear y publicar paquetes NuGet]]

## Laboratorios

- [[Lab 6 - API con controllers, FluentValidation, ProblemDetails y Options]]
- [[Lab 7 - Cliente HTTP tipado con resiliencia y health checks]]

## Progreso

```base
filters:
  and:
    - file.inFolder("03-CSharp-DotNet/05 ASP.NET Core")
    - file.name != "_Índice ASP.NET Core"
views:
  - type: table
    name: Temas
    filters:
      and:
        - tipo != "laboratorio"
    order:
      - file.name
      - progreso
      - nivel
  - type: table
    name: Laboratorios
    filters:
      and:
        - tipo == "laboratorio"
    order:
      - file.name
      - progreso
```
