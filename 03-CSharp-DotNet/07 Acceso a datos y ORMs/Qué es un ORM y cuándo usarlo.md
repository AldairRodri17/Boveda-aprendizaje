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
# Qué es un ORM y cuándo usarlo

Parte de: [[03-CSharp-DotNet/07 Acceso a datos y ORMs/_Índice Acceso a datos y ORMs|Acceso a datos y ORMs]]

> [!info] Cómo avanzar
> `progreso`: **pendiente** → **viendo** (estudiando) → **visto** (contestaste la autoevaluación sin abrir las respuestas) → **dominado** (hiciste el laboratorio o lo aplicaste de verdad).

## Contenido

### Qué es
Un **ORM** (*Object-Relational Mapper*) es una biblioteca que traduce entre el mundo de objetos de tu código (clases, propiedades, listas) y el mundo relacional de la base (tablas, columnas, llaves foráneas). Su objetivo es que trabajes con objetos y deje de ser necesario escribir a mano el SQL repetitivo y el mapeo de columnas. En .NET los dos ejemplos principales son [[Entity Framework Core]] (ORM completo) y [[Dapper]] (micro-ORM).

### Cómo funciona
El problema que resuelve se llama *desajuste de impedancia objeto-relacional*: los objetos tienen referencias, herencia y colecciones; las tablas tienen filas, llaves y JOINs. Un ORM completo ofrece:

1. **Mapeo**: qué clase corresponde a qué tabla y qué propiedad a qué columna (por convención o configuración).
2. **Generación de SQL**: traduce consultas en LINQ a SQL del motor concreto.
3. **Materialización**: convierte filas en objetos.
4. **Change tracking**: recuerda qué objetos cargaste y qué cambió, para generar `INSERT`, `UPDATE` y `DELETE` al guardar.
5. **Unidad de trabajo**: agrupa todos los cambios en una transacción (`SaveChanges`).
6. **Relaciones y carga**: navega entre entidades relacionadas y decide cuándo traerlas.
7. **Migraciones**: evoluciona el esquema desde el modelo de código.

Un **micro-ORM** solo hace los puntos 1 y 3 (y la creación de parámetros): tú escribes el SQL.

```mermaid
flowchart LR
  C[Código: objetos y LINQ] -->|ORM completo genera SQL| DB[(Base de datos)]
  C2[Código: SQL escrito por ti] -->|micro-ORM mapea resultados| DB
  DB -->|filas| M[Materialización a objetos]
```

### Ejemplo
Con EF Core, modificar un cliente se ve así; el ORM detecta qué cambió y genera el `UPDATE`:

```csharp
var cliente = await db.Clientes.SingleAsync(c => c.Id == id, ct);
cliente.CambiarCorreo(nuevoCorreo);      // regla de dominio en la entidad
await db.SaveChangesAsync(ct);           // UPDATE dbo.Cliente SET Correo = @p0 WHERE Id = @p1
```

Con Dapper, el mismo caso es explícito y el SQL vive en un archivo:

```sql
-- Sql/Clientes/CambiarCorreo.sql
UPDATE dbo.Cliente SET Correo = @Correo WHERE Id = @Id;
```

```csharp
var filas = await cn.ExecuteAsync(_sql["Clientes.CambiarCorreo"], new { Id = id, Correo = nuevoCorreo });
```

### Cuándo conviene un ORM completo como EF Core
- Dominio rico con muchas entidades relacionadas y reglas de negocio en el código (agregados que se cargan, modifican y guardan como unidad).
- Mucho CRUD, formularios administrativos, prototipos rápidos.
- El equipo prefiere LINQ y quiere migraciones de esquema versionadas junto al código.
- Necesitas soportar varios motores con el mismo código (SQL Server en producción, SQLite en pruebas rápidas).

### Cuándo un micro-ORM como Dapper
- El SQL es el centro: consultas complejas, reportes, CTEs, funciones de ventana, *hints*.
- La lógica ya está en **procedimientos almacenados** (tu caso en Itzamna.Dionisio).
- Quieres que el SQL sea revisable por un DBA en archivos `.sql` y control total del plan.
- Rendimiento predecible y sin "magia": lo que escribes es lo que se ejecuta.

### Cuándo ninguno
Para cargas masivas (`SqlBulkCopy`) o streaming de millones de filas, [[ADO.NET]] directo. Para datos que no son relacionales (documentos, caché), otra tecnología. La comparación detallada está en [[Dapper vs EF Core vs ADO.NET]].

### Errores comunes
- **Creer que el ORM te libera de saber SQL**: sin entender índices, JOINs y planes, escribirás LINQ que genera consultas terribles. Revisa siempre el SQL generado.
- **Cargar entidades completas para leer dos campos**: usa proyecciones (`Select`) o consultas sin tracking.
- **El [[Problema N+1]]**: recorrer una lista y tocar relaciones que disparan una consulta por elemento.
- **Repositorios genéricos encima de EF Core** que esconden lo que EF ya ofrece y duplican abstracciones (ver [[Repository]]).
- **Elegir por moda**: decide por el tipo de sistema y las habilidades del equipo.

### Para profundizar
- [Información general de EF Core (Microsoft Learn)](https://learn.microsoft.com/ef/core/)
- [Dapper en GitHub](https://github.com/DapperLib/Dapper)
- [Martin Fowler: ORM Hate](https://martinfowler.com/bliki/OrmHate.html)

## Autoevaluación

Contesta primero en voz alta o en papel; luego despliega la respuesta.

> [!question]- ¿Qué hace un ORM?
> Mapea tablas a clases y traduce operaciones sobre objetos a SQL: consultas con LINQ, seguimiento de cambios, relaciones y migraciones.

> [!question]- ¿Cuándo conviene un ORM completo como EF Core?
> En CRUD con modelos ricos y relaciones donde la productividad pesa más que el control del SQL, y el equipo no quiere mantener SQL a mano.

> [!question]- ¿Cuándo un micro-ORM como Dapper?
> Cuando quieres escribir y controlar el SQL (stored procedures, consultas afinadas, reportes) y solo necesitas mapear filas a objetos. Es tu elección actual.

