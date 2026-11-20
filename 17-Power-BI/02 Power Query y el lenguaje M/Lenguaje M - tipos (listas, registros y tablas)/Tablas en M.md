---
proyecto: Conocimiento
tipo: concepto
nivel: intermedio
progreso: pendiente
estado: pendiente
area: Power BI
tags: [power-bi]
---
# Tablas en M

Parte de: [[Lenguaje M - tipos (listas, registros y tablas)]]

> [!info] Cómo avanzar
> `progreso`: **pendiente** → **viendo** (estudiando) → **visto** (contestaste la autoevaluación sin abrir las respuestas) → **dominado** (hiciste el laboratorio o lo aplicaste de verdad).

## Contenido

### Qué es
Una **tabla** en M es un conjunto de **filas** (cada una un registro) con **columnas con nombre** y, opcionalmente, un **tipo** por columna. Es la estructura que termina cargándose al modelo de Power BI: cada consulta que se carga debe devolver una tabla. Casi todas las funciones de la interfaz son funciones `Table.*`.

### Cómo funciona
**Definir el tipo de una tabla.** El tipo de una tabla describe sus columnas y sus tipos:

```m
type table [IdProducto = Int64.Type, Producto = text, Precio = Currency.Type, Activo = logical]
```

Lo usas al crear una tabla literal o para asegurar el esquema de una tabla vacía o de una función. También puedes asignarlo con `Value.ReplaceType(tabla, tipo)` (cambia la declaración sin convertir valores) o, lo habitual, con `Table.TransformColumnTypes` (sí convierte).

**Crear tablas**:

| Desde | Función |
|---|---|
| Literal | `#table(tipoOColumnas, filas)` |
| Lista de valores | `Table.FromList(lista, Splitter.SplitByNothing(), {"Col"})` |
| Lista de listas por fila | `Table.FromRows({{1, "a"}, {2, "b"}}, {"Id", "Txt"})` |
| Lista de columnas | `Table.FromColumns({{1, 2}, {"a", "b"}}, {"Id", "Txt"})` |
| Lista de registros | `Table.FromRecords({[Id = 1], [Id = 2]})` |
| Un registro | `Record.ToTable(r)` (columnas `Name`, `Value`) |
| Columnas de valores | `Table.FromValue` |

Y de vuelta: `Table.ToRecords`, `Table.ToRows`, `Table.ToColumns`, `Table.Column(t, "Col")` (= `t[Col]`).

**¿Una celda puede contener una tabla?** Sí. Una celda puede contener cualquier valor: lista, registro, tabla o función. Es lo que pasa tras un **merge** (`Table.NestedJoin` crea una columna de tablas), al leer una carpeta (`Content` es binario y luego tabla) o con *Agrupar por > Todas las filas*. En la vista previa aparece la palabra `Table` y se **expande** con el botón de la columna (`Table.ExpandTableColumn`). Lo que **no** puedes es cargar al modelo una columna de tablas: hay que expandirla o agregarla antes.

### Ejemplo
Tabla de tipo de cambio escrita a mano con su tipo, útil para pruebas:

```m
let
    TipoCambio = #table(
        type table [Fecha = date, Moneda = text, Tasa = number],
        {
            {#date(2025, 1, 2), "USD", 20.53},
            {#date(2025, 1, 3), "USD", 20.61}
        })
in
    TipoCambio
```

Las cifras son de ejemplo.

Columna de tablas con *Agrupar por > Todas las filas* para quedarte con la última venta de cada cliente:

```m
let
    Grupos = Table.Group(Ventas, {"IdCliente"}, {{"Filas", each _, type table}}),
    Ultima = Table.AddColumn(Grupos, "UltimaVenta", each
        Table.Max([Filas], "Fecha")[Fecha], type date),
    Final = Table.RemoveColumns(Ultima, {"Filas"})
in
    Final
```

Tabla vacía con esquema, para que una consulta no falle cuando no hay datos:

```m
Vacia = #table(type table [IdVenta = Int64.Type, Importe = Currency.Type], {})
```

### Cuándo usarlo y cuándo no
- **Tablas literales** para catálogos pequeños que no existen en ningún sistema (umbrales, mapeos), aunque si cambian seguido es mejor un Excel en SharePoint (*Especificar datos* también crea una tabla literal comprimida).
- **Columnas de tablas** como paso intermedio, nunca como resultado.
- **Tipo de tabla explícito** en funciones que devuelven tablas, para que el esquema sea estable aunque vengan cero filas.

### Errores comunes
- **Cargar con columnas `Table` o `Record` sin expandir** → error al aplicar.
- **`Value.ReplaceType` para "convertir"** tipos: solo cambia la etiqueta; los valores siguen siendo texto.
- **Filas de distinto largo** en `#table` → error.
- **Expandir con lista fija de columnas** cuando el origen agrega columnas.

### Para profundizar
- [Funciones de tabla](https://learn.microsoft.com/es-mx/powerquery-m/table-functions)
- [#table](https://learn.microsoft.com/es-mx/powerquery-m/sharptable)
- [Tipos de tabla en la especificación](https://learn.microsoft.com/es-mx/powerquery-m/m-spec-types)

Relacionado: [[Listas en M]], [[Registros en M]], [[Power Query - combinar consultas (merge y append)]].

## Autoevaluación

Contesta primero en voz alta o en papel; luego despliega la respuesta.

> [!question]- ¿Cómo se define el tipo de una tabla?
> `type table [Id = Int64.Type, Nombre = text]`; se puede pasar a `#table` para que las columnas queden tipadas.

> [!question]- ¿Qué funciones convierten listas o registros a tabla?
> `Table.FromList`, `Table.FromRecords`, `Table.FromRows`, `Record.ToTable`.

> [!question]- ¿Una celda puede contener una tabla?
> Sí: es típico tras `Table.NestedJoin` o `Table.Group`; se expande con `Table.ExpandTableColumn`.

