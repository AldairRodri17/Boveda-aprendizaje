---
proyecto: Conocimiento
tipo: concepto
nivel: basico
progreso: pendiente
estado: pendiente
area: Power BI
tags: [power-bi]
contenido: completo
---
# Import, DirectQuery, Dual, Live Connection y Direct Lake

Parte de: [[17-Power-BI/01 Fundamentos/_Índice Fundamentos|Fundamentos]]

> [!info] Cómo avanzar
> `progreso`: **pendiente** → **viendo** (estudiando) → **visto** (contestaste la autoevaluación sin abrir las respuestas) → **dominado** (hiciste el laboratorio o lo aplicaste de verdad).

## Contenido

### Qué es
El **modo de almacenamiento** decide **dónde viven los datos** que consulta un informe y **quién hace el cálculo** cuando un usuario mueve un filtro. Es la primera decisión de arquitectura de un modelo y afecta rendimiento, frescura de datos, funciones disponibles y costo. Power BI ofrece cinco opciones que se agrupan en dos familias:

- **Por tabla** dentro de tu propio modelo: [[Modo de almacenamiento Import]], [[Modo de almacenamiento DirectQuery]], [[Modo de almacenamiento Dual]] y, en Fabric, **Direct Lake**.
- **Por informe**: [[Conexión dinámica (Live Connection) en Power BI]], donde no tienes modelo propio y te conectas a uno que ya existe.

### Cómo funciona
```mermaid
flowchart TB
  U[Usuario filtra un visual] --> Q[Consulta DAX]
  Q --> I{Modo de la tabla}
  I -->|Import| V[VertiPaq en memoria<br/>copia comprimida del origen]
  I -->|DirectQuery| S[(Origen: SQL Server, etc.)<br/>se traduce a SQL en cada consulta]
  I -->|Dual| D[Memoria o SQL<br/>según con qué tablas se combine]
  I -->|Direct Lake| L[(OneLake - Delta)<br/>columnas cargadas a memoria bajo demanda]
  Q -.Live Connection.-> R[Modelo semántico remoto<br/>Service o Analysis Services]
```

| Modo | Dónde están los datos | Frescura | Rendimiento | Notas |
|---|---|---|---|---|
| **Import** | Copia comprimida en el modelo | La de la última actualización | El mejor | Todo DAX y M disponible; límite de tamaño por licencia |
| **DirectQuery** | En el origen | Al momento | Depende del origen | Limitaciones de M/DAX; cada visual genera consultas |
| **Dual** | Ambos | Según el caso | Combina | Para dimensiones compartidas entre tablas Import y DirectQuery |
| **Direct Lake** | Tablas Delta en OneLake | Casi inmediata tras *reframing* | Similar a Import | Solo con capacidad Fabric; sin copia de importación |
| **Live Connection** | Modelo remoto | La del modelo remoto | La del modelo remoto | No tienes Power Query ni modelo propio |

**Direct Lake** merece atención: lee directamente archivos Delta Parquet de OneLake y los carga al mismo motor VertiPaq sólo cuando se consultan. Una "actualización" en Direct Lake (*framing*) solo actualiza metadatos en segundos, no copia datos. Hay dos variantes: **Direct Lake sobre OneLake** (no cae a DirectQuery y permite mezclar con tablas Import) y **Direct Lake sobre SQL** (usa el punto de conexión SQL y puede caer a DirectQuery con vistas o seguridad SQL). Detalle en [[Direct Lake]].

### Ejemplo
Modelo de ventas para Itzamna.Dionisio:

- `Ventas` (80 millones de filas, se actualiza cada minuto en SQL Server): el negocio exige ver el día en curso → **DirectQuery**.
- `Productos` (2 000 filas) y `Calendario` (pocas miles): se usan con `Ventas` y también solos en segmentaciones → **Dual**. Así las segmentaciones responden desde memoria y, cuando se cruzan con `Ventas`, el motor envía un solo SQL con el `JOIN` al origen, en lugar de mezclar resultados de dos mundos (lo que crearía **relaciones limitadas**).
- `Metas` (Excel de 50 filas) → **Import**.

El resultado es un [[Modelos compuestos|modelo compuesto]].

### Cuándo usarlo y cuándo no
- **Import por defecto.** Es el más rápido y completo; con actualización incremental cubre muchísimos casos "casi en tiempo real".
- **DirectQuery** solo con una razón concreta: datos que cambian cada minuto y se necesitan al instante, volumen que no cabe, o política que prohíbe copiar datos.
- **Dual** para dimensiones en modelos con DirectQuery.
- **Direct Lake** si tu equipo ya trabaja en Fabric con Lakehouse/Warehouse.
- **Live Connection** cuando existe un modelo semántico corporativo; reutilízalo en lugar de copiarlo.

### Errores comunes
- **Elegir DirectQuery "para que esté actualizado" sin medir el origen.** Si la base tarda 8 segundos por consulta, cada visual tarda 8 segundos.
- **Dejar dimensiones en Import junto a hechos DirectQuery.** Se crean relaciones limitadas y consultas más lentas; ponlas en Dual.
- **Creer que Direct Lake funciona sin Fabric.** Requiere capacidad F.
- **Cambiar de Import a DirectQuery a medio proyecto**: no se puede revertir una tabla DirectQuery a Import sin reconstruirla y algunas transformaciones dejarán de funcionar.

### Para profundizar
- [Modo de almacenamiento en Power BI Desktop](https://learn.microsoft.com/es-mx/power-bi/transform-model/desktop-storage-mode)
- [Uso de DirectQuery en Power BI Desktop](https://learn.microsoft.com/es-mx/power-bi/connect-data/desktop-use-directquery)
- [Información general de Direct Lake](https://learn.microsoft.com/es-mx/fabric/fundamentals/direct-lake-overview)

## Subtemas

- [[Modo de almacenamiento Import]]
- [[Modo de almacenamiento DirectQuery]]
- [[Modo de almacenamiento Dual]]
- [[Conexión dinámica (Live Connection) en Power BI]]

## Autoevaluación

Contesta primero en voz alta o en papel; luego despliega la respuesta.

> [!question]- ¿Qué diferencia fundamental hay entre Import y DirectQuery?
> Import copia los datos al modelo (VertiPaq, en memoria comprimida): muy rápido, pero los datos son tan recientes como la última actualización. DirectQuery deja los datos en el origen y envía consultas en cada interacción: datos actuales, pero depende del rendimiento del origen y tiene limitaciones de DAX/M.

> [!question]- ¿Qué es una conexión dinámica (Live Connection)?
> Conectar el informe a un modelo semántico ya existente (en el Service o Analysis Services) sin tener modelo propio: no hay Power Query ni tablas locales (salvo que se convierta en modelo compuesto).

> [!question]- ¿Qué ofrece Direct Lake frente a Import y DirectQuery?
> Lee directamente tablas Delta de OneLake con el motor en memoria, sin copia de importación ni consultas SQL por cada visual. Requiere Fabric. Ver [[Direct Lake]].

> [!question]- Caso: tienes un catálogo de 2 000 productos que se usa con una tabla de hechos en DirectQuery. ¿Qué modo darías a Productos?
> **Dual**: se comporta como Import cuando la consulta solo usa esa tabla o tablas Import, y como DirectQuery cuando se combina con la tabla DirectQuery, evitando relaciones "limitadas".

## Progreso de los subtemas

```base
filters:
  and:
    - file.inFolder("17-Power-BI/01 Fundamentos/Import, DirectQuery, Dual, Live Connection y Direct Lake")
    - file.name != "Import, DirectQuery, Dual, Live Connection y Direct Lake"
views:
  - type: table
    name: Progreso
    order:
      - file.name
      - progreso
      - nivel
```
