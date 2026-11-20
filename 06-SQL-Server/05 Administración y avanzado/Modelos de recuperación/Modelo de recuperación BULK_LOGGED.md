---
proyecto: Conocimiento
tipo: concepto
nivel: avanzado
progreso: pendiente
estado: pendiente
area: SQL Server
tags: [sql-server]
---
# Modelo de recuperación BULK_LOGGED

Parte de: [[Modelos de recuperación]]

> [!info] Cómo avanzar
> `progreso`: **pendiente** → **viendo** (estudiando) → **visto** (contestaste la autoevaluación sin abrir las respuestas) → **dominado** (hiciste el laboratorio o lo aplicaste de verdad).

## Contenido

### Qué es
**BULK_LOGGED** es un modelo de recuperación intermedio entre FULL y SIMPLE. Funciona como FULL (necesitas backups de log y el log no se trunca solo), pero ciertas **operaciones masivas** se registran **mínimamente**: en lugar de escribir cada fila al log, SQL Server anota solo qué extensiones cambiaron. Esto hace que esas operaciones sean más rápidas y generen mucho menos log.

Operaciones que pueden registrarse mínimamente: `BULK INSERT`, `bcp`, `INSERT ... SELECT` con `TABLOCK` hacia un heap o tabla vacía, `SELECT INTO`, `CREATE INDEX`, `ALTER INDEX REBUILD` y escrituras de tipos grandes con `.WRITE`.

### Cómo funciona
```mermaid
flowchart LR
  F[FULL: operación normal] --> B[Cambiar a BULK_LOGGED]
  B --> O[Carga masiva o rebuild de índice<br/>registro mínimo]
  O --> V[Volver a FULL]
  V --> L[Backup de log inmediato]
```
Cuando haces el siguiente backup de log, SQL Server copia también las **extensiones de datos** modificadas por la operación masiva (por eso ese backup de log puede ser grande aunque el log no lo sea).

**La limitación**: si un backup de log contiene una operación mínimamente registrada, **no puedes restaurar a un punto en el tiempo dentro de ese intervalo**. Solo puedes restaurar el backup de log completo (hasta su final) o detenerte antes de él. Además, si los archivos de datos se dañan antes de hacer ese backup de log, el **backup de la cola del log** no puede recuperar los cambios masivos, porque el log no tiene las filas.

### Ejemplo
Carga nocturna de 20 millones de movimientos en una base que normalmente está en FULL:
```sql
-- 1. Respaldo de log justo antes (punto seguro)
BACKUP LOG Reporteria TO DISK = N'D:\Backups\Reporteria_log_antes_carga.trn';

-- 2. Cambiar el modelo
ALTER DATABASE Reporteria SET RECOVERY BULK_LOGGED;

-- 3. Carga masiva con registro mínimo
BULK INSERT stg.Movimientos
FROM N'D:\Cargas\movimientos_20261109.csv'
WITH (FORMAT = 'CSV', FIRSTROW = 2, TABLOCK, BATCHSIZE = 100000);

ALTER INDEX ALL ON stg.Movimientos REBUILD;

-- 4. Regresar a FULL y respaldar el log de inmediato
ALTER DATABASE Reporteria SET RECOVERY FULL;
BACKUP LOG Reporteria TO DISK = N'D:\Backups\Reporteria_log_despues_carga.trn';
```
Con esto, el intervalo sin restauración puntual queda acotado a la duración de la carga. Un escenario así encaja con Wee.Reporteria si sus cargas viven en un SQL Server local.

### Cuándo usarlo y cuándo no
- **Sí**, de forma **temporal**: ventanas de carga masiva, reconstrucción de índices grandes o migraciones de datos, cuando el log en FULL sería enorme.
- **No** como modelo permanente en bases transaccionales: pierdes restauración puntual en cualquier intervalo con operaciones masivas, y es fácil olvidar que alguna se registró mínimamente.
- **No** si la base participa en Always On availability groups o log shipping con requisitos estrictos; los grupos de disponibilidad exigen FULL.
- No existe en Azure SQL Database, que siempre usa FULL.
- El panorama completo está en [[Modelos de recuperación]]; compara con [[Modelo de recuperación FULL]] y [[Modelo de recuperación SIMPLE]].

### Errores comunes
1. **Dejar la base en BULK_LOGGED** después de la carga. Agrega el regreso a FULL en el mismo script o job, con manejo de errores.
2. **No respaldar el log justo después.** Mientras tanto estás expuesto a perder la carga y todo lo que siga.
3. **Esperar registro mínimo sin cumplir las condiciones.** Sin `TABLOCK`, o con una tabla con índice clustered y datos, el registro puede no ser mínimo y el log crece igual. Revisa la documentación de requisitos.
4. **Suponer que el backup de log será pequeño.** Incluye las extensiones modificadas; planea espacio.

### Para profundizar
- [Modelos de recuperación (Microsoft Learn)](https://learn.microsoft.com/sql/relational-databases/backup-restore/recovery-models-sql-server)
- [Operaciones que pueden registrarse mínimamente](https://learn.microsoft.com/sql/relational-databases/logs/the-transaction-log-sql-server#MinimallyLogged)
- [Requisitos para el registro mínimo en importaciones masivas](https://learn.microsoft.com/sql/relational-databases/import-export/prerequisites-for-minimal-logging-in-bulk-import)

## Autoevaluación

Contesta primero en voz alta o en papel; luego despliega la respuesta.

> [!question]- ¿Qué hace BULK_LOGGED?
> Igual que FULL, pero las operaciones masivas (`BULK INSERT`, `SELECT INTO`, recreación de índices) se registran de forma mínima: el log crece mucho menos durante cargas grandes.

> [!question]- ¿Qué limitación tiene?
> Si un backup de log contiene operaciones con registro mínimo, no puedes restaurar a un punto **dentro** de ese backup (solo al final). Y ese backup de log incluye las páginas cambiadas, así que pesa más.

> [!question]- ¿Cómo se usa típicamente?
> Cambiar de FULL a BULK_LOGGED justo antes de una carga masiva, volver a FULL al terminar y hacer un backup de log inmediatamente.

