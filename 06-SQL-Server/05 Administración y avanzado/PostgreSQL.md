---
proyecto:
  - Pochteca.Nomisma
proyectos:
  - "[[Pochteca.Nomisma]]"
tipo: herramienta
nivel: intermedio
estado: completo
tags:
  - conocimiento
  - backend
  - base-de-datos
  - postgres
progreso: pendiente
area: SQL Server
---
# PostgreSQL

> Base de datos relacional open source. [[Pochteca.Nomisma]] la usa en **Neon** (Postgres serverless) con el proveedor Npgsql y [[Dapper]], en el esquema `payment`.

## Qué es y qué problema resuelve
Una base relacional completa (transacciones, UNIQUE parciales, JSONB, funciones en PL/pgSQL) sin licencia. Neon la ofrece como servicio con escalado a cero y ramas de base de datos.

## Cómo funciona por dentro
- **Esquemas**: espacios de nombres dentro de una base (`payment.cargos`), útiles para separar módulos que comparten base.
- **Funciones y procedimientos**: `CREATE FUNCTION ... RETURNS TABLE(...)` se llama con `SELECT * FROM f(...)`; `CREATE PROCEDURE` se llama con `CALL p(...)`. Pueden atrapar errores: `EXCEPTION WHEN unique_violation THEN ...`.
- **Errores**: `SQLSTATE` estándar; `23505` = violación de UNIQUE (`PostgresErrorCodes.UniqueViolation` en Npgsql).
- **Identificadores**: sin comillas se guardan en minúsculas (`CargoId` → `cargoid`); Dapper empata nombres sin distinguir mayúsculas.
- **Npgsql**: `NpgsqlConnection` con pooling; los parámetros `@Nombre` funcionan con Dapper.
- **Neon**: cadena con `SSL Mode=VerifyFull` y `Channel Binding=Require`.

## Cuándo usarla y cuándo no
### Ventajas
- Open source, potente (JSONB, índices parciales, `ON CONFLICT`), barata en serverless.
### Desventajas
- En serverless, la primera conexión tras inactividad tarda (arranque en frío).
- Lógica en funciones = otro lugar que versionar y desplegar.

## Alternativas
| Alternativa | En qué se diferencia |
|---|---|
| [[SQL Server]] | Lo que usa [[Itzamna.Dionisio]]; licencia, T-SQL, stored procedures |
| [[SQLite]] | Archivo local, sin servidor: lo que usa [[Ocelotl.Hermes]] |

## Cómo se usó en Pochteca.Nomisma
- Esquema `payment` con funciones `payment.sp_*` (cargos, cuentas conectadas, sesiones de Checkout, inbox de webhooks). **No están en el repositorio.**
- Cada llamada es un `.sql` de una línea con su interfaz ([[SQL en archivos con una interfaz por consulta]]):

```sql
-- Pochteca.Nomisma.Infrastructure/Persistence/Sql/SesionesCheckout/SiguienteNumeroIntento.sql
-- Siguiente número de intento de la orden (empieza en 1).
SELECT payment.sp_sesion_checkout_siguiente_numero_intento(@OrdenId, @Livemode);
```

- Los UNIQUE de la base son la última defensa contra el doble cobro: `23505` → 409 `duplicado` en `ManejadorErrores`.
- El inbox de webhooks atrapa `unique_violation` dentro de la función y regresa `false` ([[Webhooks]]).

> [!note] Pendiente de confirmar
> Dónde viven los scripts de las funciones `payment.sp_*` (los comentarios viejos mencionaban `Persistence/Sql/00x_*.sql`, que no están en el repo).

## Errores comunes y buenas prácticas
- Concatenar SQL en lugar de parámetros.
- Atrapar `23505` en C# cuando la base ya lo resuelve con `ON CONFLICT`.
- No versionar las funciones junto al código que las llama.

## Qué estudiar después
- [[Dapper]], [[Idempotencia]], `ON CONFLICT`, índices parciales.

## Documentación oficial
- https://www.postgresql.org/docs/current/
- https://www.npgsql.org/doc/
- https://neon.tech/docs

## Autoevaluación

Contesta primero en voz alta o en papel; luego despliega la respuesta.

> [!question]- Viniendo de SQL Server, ¿qué equivalencias de sintaxis usas más en PostgreSQL?
> `TOP (n)` → `LIMIT n`; `ISNULL` → `COALESCE`; `+` para texto → `||`; `GETDATE()`/`SYSUTCDATETIME()` → `now()` (con `timestamptz`); `IDENTITY` → `GENERATED ALWAYS AS IDENTITY`; `OUTPUT inserted.*` → `RETURNING *`; `nvarchar` → `text`/`varchar` (UTF-8); `uniqueidentifier` → `uuid`; `bit` → `boolean`.

> [!question]- ¿Qué cambia con mayúsculas en los nombres de objetos?
> PostgreSQL pasa a minúsculas los identificadores sin comillas (`CargoId` → `cargoid`); con comillas (`"CargoId"`) distingue y obliga a usarlas siempre. Convención: `snake_case` sin comillas. Además, `LIKE` distingue mayúsculas; para ignorarlas, `ILIKE`.

> [!question]- ¿Qué equivale a `MERGE`/upsert y a los procedimientos de SQL Server?
> `INSERT ... ON CONFLICT (clave) DO UPDATE SET ...` es el upsert idiomático (y `MERGE` existe desde PostgreSQL 15). La lógica de servidor suele ir en funciones; ver [[Funciones y procedimientos en PostgreSQL]].

> [!question]- ¿Qué diferencia importante hay en el DDL transaccional?
> En PostgreSQL casi todo el DDL es transaccional (puedes revertir un `ALTER TABLE` en una migración fallida). SQL Server también lo permite para la mayoría del DDL, pero algunas operaciones (p. ej. `CREATE DATABASE`, algunos `ALTER DATABASE`) no pueden ir en transacción en ninguno de los dos.

