---
proyecto: Conocimiento
tipo: concepto
nivel: intermedio
progreso: pendiente
estado: pendiente
area: C# y .NET
tags: [csharp-dotnet]
---
# Analizadores de Roslyn y warnings como errores

Parte de: [[03-CSharp-DotNet/05 ASP.NET Core/_Índice ASP.NET Core|ASP.NET Core]]

> [!info] Cómo avanzar
> `progreso`: **pendiente** → **viendo** (estudiando) → **visto** (contestaste la autoevaluación sin abrir las respuestas) → **dominado** (hiciste el laboratorio o lo aplicaste de verdad).

## Contenido

### Qué es
Los **analizadores de Roslyn** son reglas que corren **durante la compilación**, dentro del compilador de C# (Roslyn), y reportan problemas como warnings, errores o sugerencias. No esperan a que ejecutes la app: si escribes algo dudoso, el build te avisa. Hay varias familias:
- **CAxxxx** (*code analysis*, calidad): diseño, rendimiento, seguridad, confiabilidad. Ejemplos: CA2007 (`ConfigureAwait`), CA1848 (usar `[LoggerMessage]`), CA2100 (SQL armado con strings), CA1062 (validar argumentos).
- **IDExxxx** (estilo de código): `var`, llaves, nombres, usings sin usar (IDE0005).
- **CSxxxx** del compilador, como CS8618/CS8602 de *nullable reference types*.
- **De terceros** como paquetes NuGet: analizadores de xUnit, de seguridad, `Meziantou.Analyzer`, `SonarAnalyzer.CSharp`.

Los analizadores CA vienen incluidos en el SDK de .NET; no hay que instalar nada para empezar.

### Cómo funciona
1. El SDK carga los analizadores del SDK y los que vengan en paquetes NuGet.
2. Cada analizador inspecciona el árbol de sintaxis y el modelo semántico mientras compilas.
3. Cada diagnóstico tiene una **severidad**: `none`, `silent`, `suggestion`, `warning`, `error`. La severidad se ajusta por regla.
4. Con `TreatWarningsAsErrors`, cualquier warning rompe el build, así que nada "temporal" llega a `main`.

**Endurecer el build** se hace en un solo lugar, el `Directory.Build.props` de la raíz ([[Directory.Build.props y Central Package Management]]):

```xml
<Project>
  <PropertyGroup>
    <Nullable>enable</Nullable>
    <TreatWarningsAsErrors>true</TreatWarningsAsErrors>
    <AnalysisLevel>latest-recommended</AnalysisLevel>
    <EnforceCodeStyleInBuild>true</EnforceCodeStyleInBuild>
    <GenerateDocumentationFile>true</GenerateDocumentationFile>
    <NoWarn>$(NoWarn);CS1591</NoWarn>
  </PropertyGroup>
</Project>
```

- `AnalysisLevel` elige qué conjunto de reglas CA se activa: `latest-minimum`, `latest-recommended` o `latest-all` (o fijar una versión como `10.0-recommended` para que una actualización del SDK no te sorprenda con reglas nuevas).
- `EnforceCodeStyleInBuild` hace que las reglas IDE también corran en `dotnet build` y en el pipeline, no solo en el editor.
- `GenerateDocumentationFile` habilita IDE0005 (usings sin usar) en build; `CS1591` (falta comentario XML) se silencia porque no todo necesita doc.

**Severidades por regla en `.editorconfig`**, versionado en la raíz:

```ini
root = true

[*.cs]
dotnet_diagnostic.CA2007.severity = none
dotnet_diagnostic.CA1848.severity = suggestion
dotnet_diagnostic.CA2100.severity = error
dotnet_diagnostic.IDE0005.severity = warning
csharp_style_var_for_built_in_types = true:suggestion
```

(CA2007 no aplica a ASP.NET Core porque no hay `SynchronizationContext`; CA2100 se vuelve error porque en tus proyectos el SQL va en archivos `.sql` con parámetros, nunca concatenado.)

### Ejemplo
Silenciar una regla **en un solo lugar**, sin apagarla para todo el proyecto:

```csharp
// Opción 1: pragma alrededor de las líneas exactas
#pragma warning disable CA1031 // Se atrapa todo a propósito: el health check nunca debe lanzar
try { await cn.OpenAsync(ct); }
catch (Exception ex) { return HealthCheckResult.Unhealthy("SQL no responde", ex); }
#pragma warning restore CA1031

// Opción 2: atributo con justificación, a nivel de miembro
[System.Diagnostics.CodeAnalysis.SuppressMessage("Design", "CA1024:Use properties where appropriate",
    Justification = "Lee del recurso embebido en cada llamada a propósito")]
public string ObtenerTexto() => RecursosSql.Leer("Egresos.Listar.sql");
```

Regla práctica: el `#pragma` siempre con `restore` y un comentario del porqué; el `[SuppressMessage]` siempre con `Justification`. En revisión de PR en Azure DevOps, una supresión sin motivo se rechaza.

### Cuándo usarlo y cuándo no
- **Siempre** en proyectos nuevos: activar desde el día uno cuesta poco; en un proyecto viejo, activa por etapas (primero `warning`, luego errores por regla).
- **Sí** en el pipeline de CI, para que el estándar no dependa del editor de cada quien.
- **No** subas todas las reglas a error en un proyecto heredado de golpe: tendrás cientos de errores y la tentación de desactivar todo.
- Las auditorías de NuGet (vulnerabilidades, NU190x) también son warnings: con `TreatWarningsAsErrors` rompen el build cuando un paquete tiene una vulnerabilidad conocida, lo cual normalmente quieres.

### Errores comunes
- **`<NoWarn>` global** para callar una regla molesta en un solo archivo: escondes futuros casos reales.
- **`#pragma warning disable` sin `restore`**: apaga la regla en el resto del archivo.
- **Reglas solo en el editor** (sin `EnforceCodeStyleInBuild`): el pipeline no las respeta.
- **`latest-all` sin criterio**: muchas reglas no aplican a una API; ajústalas en `.editorconfig`.
- **Cambiar la severidad con el bombillo del IDE** y no versionar el `.editorconfig`.

### Para profundizar
- [Análisis de código en .NET (Microsoft Learn)](https://learn.microsoft.com/dotnet/fundamentals/code-analysis/overview)
- [Configuración de reglas de análisis (Microsoft Learn)](https://learn.microsoft.com/dotnet/fundamentals/code-analysis/configuration-options)
- [Suprimir advertencias de análisis (Microsoft Learn)](https://learn.microsoft.com/dotnet/fundamentals/code-analysis/suppress-warnings)

## Autoevaluación

Contesta primero en voz alta o en papel; luego despliega la respuesta.

> [!question]- ¿Qué son los analizadores de Roslyn?
> Reglas que corren durante la compilación (CAxxxx, IDExxxx y de terceros) y reportan problemas de diseño, rendimiento, seguridad o estilo.

> [!question]- ¿Cómo endureces el build?
> En `Directory.Build.props`: `<TreatWarningsAsErrors>true</TreatWarningsAsErrors>`, `<AnalysisLevel>latest-recommended</AnalysisLevel>` y `<EnforceCodeStyleInBuild>true</EnforceCodeStyleInBuild>`. Las severidades se ajustan por regla en `.editorconfig`.

> [!question]- ¿Cómo silencias una regla en un solo lugar sin apagarla?
> `#pragma warning disable CA1234` / `restore` o `[SuppressMessage]` con justificación.

