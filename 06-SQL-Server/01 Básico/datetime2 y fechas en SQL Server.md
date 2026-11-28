---
proyecto: Conocimiento
tipo: concepto
nivel: basico
progreso: pendiente
estado: pendiente
area: SQL Server
tags: [sql-server]
contenido: completo
---
# datetime2 y fechas en SQL Server

Parte de: [[06-SQL-Server/01 Básico/_Índice Básico|Básico]]

> [!info] Cómo avanzar
> `progreso`: **pendiente** → **viendo** (estudiando) → **visto** (contestaste la autoevaluación sin abrir las respuestas) → **dominado** (hiciste el laboratorio o lo aplicaste de verdad).

## Contenido

### Qué es
SQL Server tiene varios tipos para fechas y horas. Los modernos (desde SQL Server 2008) son `date`, `time`, `datetime2` y `datetimeoffset`; los antiguos, `datetime` y `smalldatetime`, se conservan por compatibilidad. **`datetime2`** es el tipo recomendado para fecha y hora: más preciso, con mayor rango y, con la precisión adecuada, igual o más pequeño que `datetime`.

### Cómo funciona
| Tipo | Rango | Precisión | Bytes |
|---|---|---|---|
| `date` | 0001-01-01 a 9999-12-31 | 1 día | 3 |
| `time(n)` | 00:00:00 a 23:59:59.9999999 | hasta 100 ns | 3–5 |
| `datetime2(n)` | 0001 a 9999 | hasta 100 ns (n = 0 a 7) | 6–8 |
| `datetimeoffset(n)` | igual que `datetime2` + desfase de zona horaria | hasta 100 ns | 8–10 |
| `datetime` | 1753-01-01 a 9999-12-31 | ~3.33 ms (redondea a .000, .003, .007) | 8 |
| `smalldatetime` | 1900 a 2079 | 1 minuto | 4 |

**¿Por qué `datetime2` sobre `datetime`?**

- **Precisión configurable**: `datetime2(0)` para segundos, `datetime2(3)` para milisegundos, `datetime2(7)` (predeterminado) para 100 ns. `datetime` redondea de forma rara: `'2025-03-31 23:59:59.999'` se convierte en `2025-04-01 00:00:00.000`.
- **Mayor rango** (desde el año 1) y alineado con el estándar SQL.
- **Espacio**: `datetime2(0..2)` ocupa 6 bytes, menos que los 8 de `datetime`.
- **Se mapea directo** a `DateTime` de .NET sin perder precisión (con `datetime` sí se pierde).

**Funciones de fecha actual**:

| Función | Tipo devuelto | Zona |
|---|---|---|
| `GETDATE()` | `datetime` | hora local del servidor |
| `SYSDATETIME()` | `datetime2(7)` | hora local del servidor |
| `SYSUTCDATETIME()` | `datetime2(7)` | UTC |
| `GETUTCDATE()` | `datetime` | UTC |
| `SYSDATETIMEOFFSET()` | `datetimeoffset(7)` | local con desfase |

La "hora local" es la del sistema operativo del servidor; en **Azure SQL Database es UTC**. Para registrar *cuándo pasó algo* (auditoría, creación, cambios de estado), guarda **UTC** con `SYSUTCDATETIME()` y convierte a hora de México al mostrar.

**`datetimeoffset`** guarda la fecha y hora **junto con su desfase** respecto a UTC (`2025-03-01 10:00 -06:00`). Úsalo cuando el desfase original es información de negocio: una cita agendada por un usuario en Tijuana y otra en Mérida, o eventos que llegan de varios países y hay que mostrarlos "en la hora de quien los generó". Con `AT TIME ZONE` conviertes entre zonas usando las reglas de Windows (`'Central Standard Time (Mexico)'`), que consideran cambios de horario; México eliminó el horario de verano en la mayor parte del país en 2022, y esas reglas se actualizan con el sistema operativo.

### Ejemplo
```sql
CREATE TABLE seguridad.Sesiones (
    SesionId        bigint IDENTITY(1,1) CONSTRAINT PK_Sesiones PRIMARY KEY,
    UsuarioId       int NOT NULL,
    InicioUtc       datetime2(3) NOT NULL CONSTRAINT DF_Sesiones_Inicio DEFAULT (SYSUTCDATETIME()),
    ExpiraUtc       datetime2(3) NOT NULL,
    FechaNacimiento date NULL,                       -- sin hora: date
    CitaLocal       datetimeoffset(0) NULL           -- conserva el desfase del usuario
);

-- Convertir UTC a hora del centro de México para mostrar
SELECT SesionId,
       InicioUtc,
       InicioUtc AT TIME ZONE 'UTC' AT TIME ZONE 'Central Standard Time (Mexico)' AS InicioMexico
FROM   seguridad.Sesiones;

-- El error de BETWEEN con hora: se pierden las filas del 31 después de las 00:00
SELECT COUNT(*) FROM seguridad.Sesiones
WHERE  InicioUtc BETWEEN '2025-03-01' AND '2025-03-31';           -- incompleto
SELECT COUNT(*) FROM seguridad.Sesiones
WHERE  InicioUtc >= '2025-03-01' AND InicioUtc < '2025-04-01';    -- correcto
```

En Xolotl.Hecate, las fechas de emisión y expiración de tokens se guardan en UTC (`datetime2(0)`), igual que los claims `iat`/`exp` del JWT, que son segundos desde la época Unix en UTC. En C#, mapea `datetime2` a `DateTime` con `Kind` UTC o a `DateTimeOffset`; `date` se mapea bien a `DateOnly` en .NET moderno (en Dapper puede requerir un `TypeHandler` según la versión que uses; (Pendiente de confirmar)).

### Cuándo usarlo y cuándo no
- **`date`** cuando no hay hora (nacimientos, fechas de pedido de Tienda).
- **`datetime2(n)`** en UTC para momentos; elige `n` según la precisión que necesitas (0 o 3 suele bastar).
- **`datetimeoffset`** cuando la zona original importa o recibes datos de varias zonas.
- **`datetime`** solo por compatibilidad con sistemas existentes.

### Errores comunes
1. **`BETWEEN '...-01' AND '...-31'`** sobre columnas con hora: pierde casi todo el último día. Usa rango semiabierto.
2. **Guardar hora local del servidor** con `GETDATE()`: al migrar a Azure (UTC) o cambiar de servidor, las fechas cambian de significado.
3. **Usar `datetime`** y sufrir redondeos a 3.33 ms o el límite de 1753.
4. **Literales ambiguos** (`'01/03/2025'`): dependen del idioma; usa formato ISO `'2025-03-01'`.
5. **Mezclar tipos** al comparar (`datetime2` contra `datetime`, o `date` contra texto): conversiones implícitas que pueden impedir el uso de índices.

### Para profundizar
- [datetime2 (Transact-SQL)](https://learn.microsoft.com/sql/t-sql/data-types/datetime2-transact-sql)
- [datetimeoffset (Transact-SQL)](https://learn.microsoft.com/sql/t-sql/data-types/datetimeoffset-transact-sql)
- [AT TIME ZONE (Transact-SQL)](https://learn.microsoft.com/sql/t-sql/queries/at-time-zone-transact-sql)

## Autoevaluación

Contesta primero en voz alta o en papel; luego despliega la respuesta.

> [!question]- ¿Por qué preferir `datetime2` sobre `datetime`?
> `datetime2` tiene rango 0001–9999, precisión de hasta 100 ns configurable (`datetime2(0)` a `(7)`) y ocupa 6–8 bytes. `datetime` va de 1753 a 9999, redondea a incrementos de .000/.003/.007 s y ocupa 8 bytes. Para columnas nuevas, `datetime2`.

> [!question]- ¿Qué diferencia hay entre `GETDATE()`, `SYSDATETIME()` y `SYSUTCDATETIME()`?
> `GETDATE()` devuelve hora local del servidor como `datetime`; `SYSDATETIME()` hora local como `datetime2(7)`; `SYSUTCDATETIME()` hora UTC como `datetime2(7)`. En apps en la nube guarda **UTC** (`SYSUTCDATETIME()`); en Azure SQL la hora del servidor ya es UTC.

> [!question]- ¿Cuándo usarías `datetimeoffset`?
> Cuando necesitas conservar el desfase de la zona en que ocurrió el evento (p. ej. una cita agendada desde CDMX). Para convertir: `SYSUTCDATETIME() AT TIME ZONE 'UTC' AT TIME ZONE 'Central Standard Time (Mexico)'` (los nombres de zona están en `sys.time_zone_info`).

> [!question]- Error común: `WHERE Fecha BETWEEN '2025-03-01' AND '2025-03-31'` sobre una columna `datetime2`. ¿Qué falla?
> `'2025-03-31'` equivale a `2025-03-31 00:00:00`, así que se pierden los registros del 31 después de medianoche. Usa un rango semiabierto: `WHERE Fecha >= '2025-03-01' AND Fecha < '2025-04-01'`.

