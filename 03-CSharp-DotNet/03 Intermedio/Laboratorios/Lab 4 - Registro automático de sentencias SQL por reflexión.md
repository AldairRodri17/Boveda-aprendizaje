---
proyecto: Conocimiento
tipo: laboratorio
nivel: intermedio
progreso: pendiente
estado: borrador
area: C# y .NET
herramienta: .NET 10 SDK, VS Code y xUnit
tags: [csharp-dotnet, laboratorio]
contenido: completo
---
# Lab 4 - Registro automático de sentencias SQL por reflexión

Parte de: [[03-CSharp-DotNet/03 Intermedio/_Índice Intermedio|Intermedio]]

**Cubre:** [[Recursos embebidos (EmbeddedResource)]] · [[Reflexión]] · [[Atributos]] · [[Genéricos]] · [[Interfaces]] · [[Clases abstractas]]

> [!info] Al terminar
> Si todos los criterios quedan marcados, cambia a `dominado` el `progreso` de este laboratorio y de las notas que cubre. Guarda tus archivos de práctica en esta misma carpeta.

## Objetivo

Reproducir en un proyecto pequeño la mecánica de tus sentencias SQL: archivos `.sql` embebidos, una clase por consulta y registro en DI por reflexión, con pruebas que impidan archivos huérfanos.

## Pasos

1. Crea `Datos` (classlib) y `Datos.Tests` (xunit). Agrega `Microsoft.Extensions.DependencyInjection` a `Datos`.
2. Crea `Sql/Clientes/Listar.sql` y `Sql/Clientes/Obtener.sql` (cualquier `SELECT`). Embébelos con `LogicalName` para que el nombre sea `Sql/Clientes/Listar.sql`.
3. Crea `ScriptsSql` con `static string Leer(string ruta)` que lea el recurso y lance una excepción clara si no existe. Normaliza `\` a `/`.
4. Crea `interface ISentenciaSql { string Script { get; } }` y una base abstracta `SentenciaSql(string ruta) : ISentenciaSql` que lea su script en el constructor.
5. Crea dos sentencias con interfaz propia: `IListarClientesSql`, `IObtenerClienteSql`.
6. Escribe `AddSentenciasSql(this IServiceCollection s)` que, por reflexión, registre cada clase concreta que implemente `ISentenciaSql` como singleton por su clase y por sus interfaces propias.
7. Escribe pruebas: (a) se resuelven las dos interfaces; (b) cada sentencia tiene archivo; (c) cada archivo tiene sentencia.

## Criterios de terminado

- [ ] `GetManifestResourceNames()` lista `Sql/Clientes/Listar.sql` y `Sql/Clientes/Obtener.sql` (con `/`).
- [ ] `provider.GetRequiredService<IListarClientesSql>().Script` regresa el contenido del archivo.
- [ ] Si borras `Obtener.sql`, la prueba (b) falla con un mensaje que nombra el archivo faltante.
- [ ] Si agregas `Sql/Clientes/Huerfano.sql` sin sentencia, la prueba (c) falla y lo nombra.
- [ ] Ninguna clase de sentencia se registra a mano.

## Solución

> [!success]- `.csproj`
> ```xml
> <ItemGroup>
>   <EmbeddedResource Include="Sql\**\*.sql" LogicalName="Sql/%(RecursiveDir)%(Filename)%(Extension)" />
> </ItemGroup>
> ```
> Aun con `LogicalName`, `%(RecursiveDir)` puede traer `\` en Windows; por eso `ScriptsSql` normaliza.

> [!success]- Lectura y registro
> ```csharp
> public static class ScriptsSql
> {
>     private static readonly Assembly Asm = typeof(ScriptsSql).Assembly;
>     public static readonly IReadOnlyDictionary<string, string> Nombres =
>         Asm.GetManifestResourceNames().ToDictionary(n => n.Replace('\\', '/'), n => n);
>
>     public static string Leer(string ruta)
>     {
>         var clave = $"Sql/{ruta}.sql";
>         if (!Nombres.TryGetValue(clave, out var real)) throw new InvalidOperationException($"Falta el recurso {clave}");
>         using var s = Asm.GetManifestResourceStream(real)!;
>         using var r = new StreamReader(s);
>         return r.ReadToEnd();
>     }
> }
>
> public static IServiceCollection AddSentenciasSql(this IServiceCollection s)
> {
>     var tipos = typeof(ISentenciaSql).Assembly.GetTypes()
>         .Where(t => t is { IsClass: true, IsAbstract: false } && typeof(ISentenciaSql).IsAssignableFrom(t));
>     foreach (var t in tipos)
>     {
>         s.AddSingleton(t);
>         foreach (var i in t.GetInterfaces().Where(i => i != typeof(ISentenciaSql) && typeof(ISentenciaSql).IsAssignableFrom(i)))
>             s.AddSingleton(i, sp => sp.GetRequiredService(t));
>     }
>     return s;
> }
> ```

> [!success]- Prueba de huérfanos
> ```csharp
> [Fact]
> public void No_hay_sql_sin_sentencia()
> {
>     var sp = new ServiceCollection().AddSentenciasSql().BuildServiceProvider();
>     var rutas = typeof(ISentenciaSql).Assembly.GetTypes()
>         .Where(t => t is { IsClass: true, IsAbstract: false } && typeof(ISentenciaSql).IsAssignableFrom(t))
>         .Select(t => ((SentenciaSql)sp.GetRequiredService(t)).Ruta).ToHashSet();
>     var huerfanos = ScriptsSql.Nombres.Keys.Where(k => !rutas.Contains(k)).ToList();
>     Assert.True(huerfanos.Count == 0, "Sin sentencia: " + string.Join(", ", huerfanos));
> }
> ```
> (Guarda en `SentenciaSql` la propiedad `Ruta` con el nombre completo `Sql/....sql`).

## Notas personales

_Qué me costó, errores que cometí, dudas._
