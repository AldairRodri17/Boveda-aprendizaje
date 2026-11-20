---
proyecto: [Ocelotl.Hermes]
tipo: estructura
estado: vigente
tags: [estructura, reglas, backend, dotnet, sql, controllers]
actualizado: 2026-11-08
---
# Reglas Back - .NET

Índice: [[Estructura proyectos]] · Referencia viva: [[Ocelotl.Hermes]] · Aprendizajes: [[Ocelotl.Hermes - Aprendizajes]]

> [!important] Resumen para Claude
> 1. **Controllers** con `[ApiController]`, uno por funcionalidad / pantalla. **No Minimal APIs.**
> 2. La lógica vive en **slices** (`Application/Features/<Funcionalidad>/<CasoDeUso>.cs`): `Command`/`Query` + `Result` + `Handler`.
> 3. **No hay SQL escrito en C#.** Cada consulta es un archivo `.sql` con **su propia interfaz**: se le mandan sus campos y regresa su respuesta.
> 4. Errores = excepciones de negocio → **ProblemDetails con `codigo`** estable.
> 5. Si una regla no aplica o hay duda: **pregunta**.

## 1. Proyectos (solución)
```text
src/
├─ <App>.Domain          Entidades, enums y eventos. Sin dependencias.
├─ <App>.Application     Features/<Funcionalidad>/<Slice>.cs · Abstractions/ (puertos) · Common/ (excepciones, Validar)
├─ <App>.Infrastructure  Adaptadores: Persistence/ (SQLite/SQL Server + Dapper, Sql/**/*.sql) · clientes HTTP · hardware
└─ <App>.Api             Controllers/ · Errores/ · Security/ · Hubs/ · Program.cs
tests/
├─ <App>.Application.Tests   Slices con fakes de los puertos
└─ <App>.Integration.Tests   Base real temporal, adaptadores, SQL
```
Las dependencias van hacia adentro: Api → Infrastructure → Application → Domain ([[Clean Architecture]]). Puertos **solo** hacia lo externo (base, robot, otra API) ([[Arquitectura hexagonal]]); lo demás se organiza por funcionalidad ([[Vertical Slice Architecture]]).

## 2. Controllers
- Uno por funcionalidad (espejo de una pantalla del front): `ConexionController`, `IngresosController`…
- `[ApiController]`, `[Route("api/<recurso>")]`, `[Tags("<Funcionalidad>")]` y `[RequiereSesion]` si aplica.
- **Sin lógica.** Cada acción recibe la entrada y el `Handler` (`[FromServices]`) y regresa lo que regresa el slice.
- Atributos de origen **explícitos**: `[FromBody]`, `[FromQuery]`, `[FromServices]`. Un record en la query lleva `[FromQuery]` (si no, `[ApiController]` lo trata como body).
- El nombre de la acción no repite el del slice (`ConectarRobot`, no `Conectar`), para no confundirlo con el tipo `Conectar.Command`.
- Códigos distintos de 200 se declaran: `Accepted(...)` + `[ProducesResponseType<T>(202)]`, `NoContent()` para 204.
- En `Program.cs`:
```csharp
builder.Services
    .AddControllers(o => o.SuppressImplicitRequiredAttributeForNonNullableReferenceTypes = true)
    .AddJsonOptions(o => o.JsonSerializerOptions.Converters.Add(new JsonStringEnumConverter()))
    .ConfigureApiBehaviorOptions(o => o.InvalidModelStateResponseFactory = RespuestaModeloInvalido.Crear);
// ...
app.MapControllers();
```
  - `SuppressImplicitRequired…`: las reglas de negocio las valida **el slice** (código `validacion`), no el binding.
  - `RespuestaModeloInvalido`: un cuerpo o una query que no se pueden leer responden `400 peticion_invalida` con la misma forma ProblemDetails.
  - `AddProblemDetails(o => o.CustomizeProblemDetails = c => c.ProblemDetails.Extensions.TryAdd("codigo", "peticion_invalida"))`: los errores que MVC responde solo (por ejemplo, 415 por Content-Type) también llevan `codigo`.
- Un parámetro de query **obligatorio** se declara `[FromQuery, BindRequired] string x`. Si falta, responde `400 peticion_invalida`; no se usa `string?` + `?? ""`, porque eso convierte "falta el dato" en otro error, o en "sin filtro".

```csharp
[ApiController]
[Route("api/conexion")]
[Tags("Conexión")]
[RequiereSesion]
public sealed class ConexionController : ControllerBase
{
    [HttpPost("conectar")]
    public Task<Conectar.Result> ConectarRobot(
        [FromBody] Conectar.Command cmd, [FromServices] Conectar.Handler h, CancellationToken ct) =>
        h.Handle(cmd, ct);
}
```

## 3. Slices (Application)
```csharp
public static class Conectar
{
    public sealed record Command(string IpAddress, ushort? Puerto);
    public sealed record Result(...);

    public sealed class Handler(IRobotConexiones robots)
    {
        public async Task<Result> Handle(Command command, CancellationToken ct) { ... }
    }
}
```
- Un archivo = un caso de uso. Las validaciones de negocio van en el `Handler` (`Validar.Requerido`, `Validar.DireccionIp`, `ValidacionException`).
- Los `Handler` se registran solos (`AddApplication()` registra toda clase anidada `Handler`) ([[Inyección de dependencias]]).
- Reacciones a eventos: un slice que implementa `IReaccionEvento` ([[Eventos de dominio y reacciones]]).

## 4. SQL: un archivo `.sql` y una interfaz por consulta
**Regla: no hay SQL escrito en C#** (ni en strings, ni en constantes, ni en pruebas).

```text
Infrastructure/Persistence/
├─ Sql/<Área>/<Nombre>.sql      ← el SQL. Parámetros @Nombre = propiedades del record
├─ SentenciasSql.cs             ← bases: IComandoSql, IConsultaSql, IConsultaUnicaSql, IEscalarSql, ScriptsSql
├─ <Área>Sql.cs                 ← por consulta: record de campos + interfaz + clase de una línea
└─ <Área>Repositorio.cs         ← implementa el puerto; solo convierte dominio ↔ campos/filas
```

```sql
-- Sql/Egresos/Listar.sql
SELECT e.robot_id AS RobotId, e.orden AS Orden, ...
FROM egresos e
WHERE (@Desde IS NULL OR e.creado_en >= @Desde)
  AND (@RobotId IS NULL OR e.robot_id = @RobotId)
ORDER BY e.creado_en DESC
LIMIT @Limite;
```
```csharp
// EgresosSql.cs
public sealed record ListarEgresosCampos(long? Desde, long? Hasta, string? RobotId, string? Origen, int Limite);

public interface IListarEgresosSql : IConsultaSql<ListarEgresosCampos, EgresoListadoFila> { }

public sealed class ListarEgresosSql(ConexionSqlite db)
    : ConsultaSql<ListarEgresosCampos, EgresoListadoFila>(db, "Egresos/Listar"), IListarEgresosSql { }

// EgresoRepositorio.cs: sin SQL
var filas = await listar.ConsultarAsync(new ListarEgresosCampos(desde, hasta, robotId, origen, limite), ct);
```

| Base | Para | Regresa |
|---|---|---|
| `IComandoSql<TCampos>` | INSERT / UPDATE / DELETE / DDL | filas afectadas (`int`); acepta transacción |
| `IConsultaSql<TCampos, TFila>` | SELECT de varias filas | `IReadOnlyList<TFila>` |
| `IConsultaUnicaSql<TCampos, TFila>` | SELECT de una fila o ninguna | `TFila?` |
| `IEscalarSql<TCampos, TValor>` | COUNT / EXISTS / un valor | `TValor`; acepta transacción |

Reglas:
- **Nombres:** archivo `Sql/<Área>/<Verbo><Qué>.sql`; record `<Verbo><Qué>Campos`; interfaz `I<Verbo><Qué>Sql`; clase `<Verbo><Qué>Sql`. Sin campos: `SinCampos.Valor`.
- Los **parámetros** del `.sql` se llaman igual que las propiedades del record (`@RobotId` ↔ `RobotId`). En los comentarios no dejes un `?`, `@` o `:` suelto (por ejemplo `"x":`): Dapper lo toma como parámetro posicional y deja de filtrar las propiedades que sobran. Una prueba lo revisa. Las **columnas** se renombran con `AS` a las propiedades de la fila.
- La fila es una clase (o record posicional) **del adaptador**; el repositorio la convierte a la entidad del dominio. SQLite devuelve `INTEGER` como `long`.
- Fechas en unix ms (`long`), enums como texto, JSON como texto: la conversión va en el repositorio.
- **Transacciones:** el repositorio abre la conexión y la transacción y pasa `tx` a cada comando (`EjecutarAsync(campos, tx, ct)`).
- **Filtros opcionales** con `(@X IS NULL OR col = @X)`; nada de armar SQL concatenando.
- **Esquema:** `Sql/Esquema/Base.sql` (idempotente, `IF NOT EXISTS`) más una migración por archivo (`V4_<Qué>.sql`) que se aplica una sola vez, revisando `version_esquema`.
- **Registro:** `AddSentenciasSql()` registra todas las sentencias (por su clase y su interfaz). Al arrancar se construyen todas: si falta un `.sql`, la app **no arranca**.
- **.csproj:** `<EmbeddedResource Include="Persistence\Sql\**\*.sql" LogicalName="Sql/%(RecursiveDir)%(Filename)%(Extension)" />`.
- **Pruebas:** `SentenciasSqlTests` verifica que cada sentencia tenga su `.sql`, que no haya `.sql` sin sentencia y que cada una tenga su interfaz propia. Las pruebas también consultan por interfaz, sin SQL propio.
- **Consulta nueva:** 1) el `.sql`, 2) su record de campos (y su fila si es SELECT), 3) su interfaz, 4) su clase de una línea, 5) usarla desde el repositorio.

> [!note] Alternativas que se descartaron
> - **Una interfaz genérica** (`IBaseDatos.ConsultarAsync<T>(consulta, campos)`): menos archivos, pero sin contrato por consulta.
> - **EF Core:** cero SQL, pero más peso y "magia"; los upserts (`ON CONFLICT`) son menos directos.
> - **Stored procedures / funciones:** SQLite no los tiene. Cuando la base sí los tiene, cada llamada es un `.sql` de una línea que los invoca, con la misma interfaz por consulta (definido en [[Pochteca.Nomisma]], Postgres).

Ver [[Dapper]], [[SQLite]] y [[Repository]].

## 5. Errores
- Excepciones de negocio en `Application/Common` con un `Codigo` estable (`validacion`, `no_encontrado`, `sesion_requerida`, `argo_no_disponible`…).
- `ManejadorErrores` (`IExceptionHandler`) las convierte en **ProblemDetails** con la extensión `codigo`; el front decide por el código, no por el texto ([[ProblemDetails]]).
- 500: mensaje genérico y log con el detalle.

- Un **servicio externo** que falla (sin acceso, caído, respuesta rara) responde **502/503** con su propio código (`azure_devops_sin_acceso`, `azure_devops_no_disponible`), no 500. El cliente de infraestructura traduce sus excepciones HTTP a las de Application ([[ReporteriaAzure]]).

- Si Infrastructure no referencia Application, las excepciones con `Codigo` van en el proyecto de dominio, para que también las lancen los adaptadores ([[Pochteca.Nomisma]]).
- Un **rechazo** del proveedor (tarjeta declinada) es un resultado, no una excepción.

## 6. Seguridad
- Filtros por **atributo** (`[RequiereSesion]` = `IAuthorizationFilter`): corre antes del binding.
- Si la API es local: escuchar solo en loopback y exigir un secreto por arranque (header), comparado con `FixedTimeEquals`.
- Contraseñas con PBKDF2-SHA256 (210 000 iteraciones o más).
- *Pendiente:* roles por endpoint (hoy solo en el front).

## 7. Configuración, fondo y resiliencia
- Opciones tipadas por sección ([[Options pattern]]).
- Trabajo en segundo plano con [[BackgroundService]] y [[System.Threading.Channels]].
- Integración con otra API que puede no estar: [[Patrón Outbox]] + [[Idempotencia]] (`Idempotency-Key`, deduplicación por id de evento).
- Simulados (`XSimulado`) **solo** en Development; `Program.cs` truena si se encienden en otro ambiente.

- **Secretos** (tokens, PAT, llaves): dónde van lo decide la forma de publicar. Publicación por copia de carpeta (FileZilla) → `appsettings.json` con un token de alcance mínimo; si el servidor tiene variables de entorno, ahí ([[Secretos con User Secrets]]). Preguntar antes de moverlos.
- Lo obligatorio se valida con `ValidateOnStart`, y el mensaje dice cómo arreglarlo.

## 8. Estilo C#
- `sealed` por defecto, primary constructors, records para datos, `CancellationToken` en todo lo asíncrono.
- Nombres en español, como el dominio (`Egreso`, `Caja`, `Handler`, `Result`).
- Comentarios solo para un contrato o un porqué.

## 9. Checklist antes de entregar
- [ ] Ningún endpoint con `Map*`: todo en Controllers.
- [ ] Ningún `SELECT`/`INSERT`/`UPDATE`/`DELETE` en archivos `.cs` (buscar con `grep`).
- [ ] Cada consulta nueva: `.sql` + campos + interfaz + clase; `SentenciasSqlTests` pasa.
- [ ] Errores con `codigo`; la forma de la respuesta no cambió para el front.
- [ ] `dotnet build` y `dotnet test` pasan. Reportar qué se probó y qué falta probar contra el sistema real.

- [ ] La configuración obligatoria se valida al arrancar; los tokens tienen el alcance mínimo.
