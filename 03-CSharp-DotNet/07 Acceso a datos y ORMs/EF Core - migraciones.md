---
proyecto: Conocimiento
tipo: concepto
nivel: intermedio
progreso: pendiente
estado: pendiente
area: C# y .NET
tags: [csharp-dotnet]
contenido: completo
---
# EF Core - migraciones

Parte de: [[03-CSharp-DotNet/07 Acceso a datos y ORMs/_Índice Acceso a datos y ORMs|Acceso a datos y ORMs]]

> [!info] Cómo avanzar
> `progreso`: **pendiente** → **viendo** (estudiando) → **visto** (contestaste la autoevaluación sin abrir las respuestas) → **dominado** (hiciste el laboratorio o lo aplicaste de verdad).

## Contenido

### Qué es
Las **migraciones** de [[Entity Framework Core]] son la forma de evolucionar el esquema de la base de datos a partir de cambios en tu modelo de C#. Cada vez que agregas una entidad, una propiedad o un índice, generas una migración: una clase con métodos `Up` (aplicar el cambio) y `Down` (revertirlo), más una instantánea del modelo. Así el esquema queda versionado junto con el código en Azure DevOps y cada ambiente se puede llevar a la misma versión.

### Cómo funciona
```mermaid
flowchart LR
  M[Cambias el modelo C#] --> A[dotnet ef migrations add]
  A --> F[Migración: Up/Down + ModelSnapshot]
  F --> R[Revisas el código generado]
  R --> S[Script idempotente o bundle en el pipeline]
  S --> DB[(Base de datos)]
  DB --> H[__EFMigrationsHistory registra las aplicadas]
```

1. EF Core compara tu modelo actual con la **instantánea** (`ModelSnapshot`) de la última migración.
2. Genera las operaciones necesarias (`CreateTable`, `AddColumn`, `CreateIndex`...).
3. Al aplicarse, cada migración se registra en la tabla `__EFMigrationsHistory`; así EF sabe cuáles faltan.

Comandos principales (herramienta `dotnet-ef`, instalada con `dotnet tool install --global dotnet-ef`, y el paquete `Microsoft.EntityFrameworkCore.Design` en el proyecto de inicio):

```bash
dotnet ef migrations add AgregarRfcACliente --project Nomisma.Infra --startup-project Nomisma.Api
dotnet ef migrations list
dotnet ef migrations remove                    # quita la última si NO se ha aplicado
dotnet ef database update                      # aplica en la base local
dotnet ef migrations script --idempotent -o migraciones.sql
dotnet ef migrations bundle --self-contained -o efbundle
```

### Ejemplo
Agregas `Rfc` a `Cliente`. La migración generada:

```csharp
public partial class AgregarRfcACliente : Migration
{
    protected override void Up(MigrationBuilder mb)
    {
        mb.AddColumn<string>("Rfc", "Cliente", maxLength: 13, nullable: true);
        mb.CreateIndex("IX_Cliente_Rfc", "Cliente", "Rfc", unique: true, filter: "\"Rfc\" IS NOT NULL");
    }
    protected override void Down(MigrationBuilder mb)
    {
        mb.DropIndex("IX_Cliente_Rfc", "Cliente");
        mb.DropColumn("Rfc", "Cliente");
    }
}
```

### Renombrar una propiedad
Si cambias `NombreCompleto` a `RazonSocial` y generas la migración, EF Core **no puede saber** si quisiste renombrar o borrar una columna y crear otra. Normalmente genera `DropColumn` + `AddColumn` y avisa: *"An operation was scaffolded that may result in the loss of data"*. Si la aplicas tal cual, **pierdes los datos** de esa columna. La solución es editar la migración y reemplazar ambas operaciones por `mb.RenameColumn("NombreCompleto", "Cliente", "RazonSocial")`. Por eso las migraciones se revisan como cualquier código en el PR.

### Aplicar en producción de forma segura
- **No uses `Database.Migrate()` al arrancar la app** en producción: si hay varias instancias, compiten; la app necesita permisos de DDL; y un fallo deja la app sin arrancar a medio cambio.
- Genera un **script idempotente** (`--idempotent`, revisa qué migraciones faltan antes de aplicar cada una) o un **bundle** (ejecutable que aplica migraciones) como artefacto del pipeline de Azure DevOps.
- Revisa el script, haz respaldo, y aplícalo en un paso del *release* con una identidad que tenga permisos de esquema, separada del usuario de la app.
- Diseña cambios **compatibles hacia atrás** (expand/contract): primero agrega columnas nullable, despliega el código que las usa, migra datos y al final elimina lo viejo. Así puedes desplegar sin tiempo fuera.

### Cuándo usarlo y cuándo no
Usa migraciones si EF Core es dueño del esquema (por ejemplo, un servicio nuevo en Pochteca.Nomisma). Si la base la administra un DBA, o tu estándar son procedimientos y scripts SQL como en Itzamna.Dionisio con [[Dapper]], es mejor un enfoque *database-first*: scripts versionados (DbUp, proyectos SQL/DACPAC) y, si usas EF, `dotnet ef dbcontext scaffold` para generar el modelo.

### Errores comunes
- **Aplicar migraciones sin revisarlas**: pérdidas de datos por renombres o cambios de tipo.
- **Borrar o editar migraciones ya aplicadas en otros ambientes**: el historial queda inconsistente. Crea una nueva.
- **Conflictos de instantánea** cuando dos ramas agregan migraciones: rehaz la tuya sobre la rama actualizada.
- **Datos de catálogo con `HasData` cambiantes**: genera migraciones ruidosas; usa scripts de datos aparte si cambian seguido.
- **Ejecutar `database update` contra producción desde tu PC**: sin control ni auditoría.

### Para profundizar
- [Migraciones (Microsoft Learn)](https://learn.microsoft.com/ef/core/managing-schemas/migrations/)
- [Aplicar migraciones (Microsoft Learn)](https://learn.microsoft.com/ef/core/managing-schemas/migrations/applying)
- [Referencia de dotnet ef (Microsoft Learn)](https://learn.microsoft.com/ef/core/cli/dotnet)

## Autoevaluación

Contesta primero en voz alta o en papel; luego despliega la respuesta.

> [!question]- ¿Qué comandos usas?
> `dotnet ef migrations add NombreCambio` y `dotnet ef database update`. Requieren la herramienta `dotnet-ef`.

> [!question]- ¿Cómo aplicas migraciones en producción de forma segura?
> Genera un script revisable: `dotnet ef migrations script --idempotent -o migracion.sql`, o un *migration bundle*. Evita `Database.Migrate()` al arrancar con varias instancias.

> [!question]- ¿Qué pasa si renombras una propiedad y generas la migración?
> EF puede interpretarlo como borrar y crear columna (perdiendo datos). Revisa la migración y cámbiala a `RenameColumn`.

