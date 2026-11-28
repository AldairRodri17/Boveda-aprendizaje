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
# Power Query - tipos de datos

Parte de: [[17-Power-BI/02 Power Query y el lenguaje M/_Índice Power Query y el lenguaje M|Power Query y el lenguaje M]]

> [!info] Cómo avanzar
> `progreso`: **pendiente** → **viendo** (estudiando) → **visto** (contestaste la autoevaluación sin abrir las respuestas) → **dominado** (hiciste el laboratorio o lo aplicaste de verdad).

## Contenido

### Qué es
Cada columna en Power Query tiene un **tipo de datos**: texto, número entero, número decimal, decimal fijo (moneda), fecha, fecha/hora, fecha/hora/zona, duración, lógico, binario o *cualquiera* (`any`, sin tipo, el icono "ABC123"). El tipo determina cómo se guarda en el modelo, qué operaciones admite, cómo se comprime y si DAX podrá sumarlo o usarlo como fecha.

### Cómo funciona
El paso clave es `Table.TransformColumnTypes`, que recibe una lista de pares `{columna, tipo}` y, opcionalmente, una **cultura** que define cómo interpretar textos (separador decimal, orden día/mes):

| Tipo en la interfaz | Tipo M | Uso |
|---|---|---|
| Número entero | `Int64.Type` | Claves, cantidades |
| Número decimal | `type number` | Medidas físicas, porcentajes |
| Decimal fijo (moneda) | `Currency.Type` | Importes: 4 decimales exactos |
| Fecha | `type date` | Análisis por día |
| Fecha y hora | `type datetime` | Solo si de verdad necesitas la hora |
| Verdadero/Falso | `type logical` | Banderas |
| Texto | `type text` | Descripciones, códigos con ceros a la izquierda |

**¿Por qué tipos explícitos?** Porque:
- Una columna `any` se carga como texto y DAX no la podrá sumar.
- Los errores de conversión aparecen **aquí**, al actualizar, donde los ves y controlas, en lugar de resultados raros en el informe.
- El tipo correcto comprime mejor (entero vs texto) y permite plegar filtros al origen.
- Las relaciones solo funcionan entre columnas del **mismo tipo**.

**Decimal vs decimal fijo.** *Número decimal* es punto flotante de doble precisión: hasta ~15 dígitos significativos, pero con errores de redondeo binario (0.1 + 0.2 ≠ 0.3 exacto). *Decimal fijo* guarda exactamente 4 decimales como entero escalado: sin errores de redondeo en sumas de dinero y mejor compresión. Para importes en pesos, usa decimal fijo.

### Ejemplo
Tipos básicos sobre una tabla `Origen`:

```m
let
    Tipos = Table.TransformColumnTypes(Origen, {
        {"Cantidad", Int64.Type},
        {"Precio", type number},
        {"Fecha", type date}
    })
in
    Tipos
```

Importe que llega como texto `"$1,250.50"`:

```m
ImporteNum = Table.AddColumn(Origen, "ImporteNum", each
    Number.From(Text.Remove([Importe], {"$", ","}), "en-US"), Currency.Type)
```

Fechas `03/04/2025` en un CSV mexicano (día/mes) que se leían como 4 de marzo: indica la cultura en el cambio de tipo (*Cambiar tipo > Usar configuración regional* en la interfaz):

```m
FechaMX = Table.TransformColumnTypes(Origen, {{"Fecha", type date}}, "es-MX")
// "03/04/2025" -> 3 de abril de 2025
```

Columna `Activo` con "Sí"/"No" a lógico:

```m
ActivoBool = Table.TransformColumns(Origen, {{"Activo", each
    if _ = null then null else Text.Lower(Text.Trim(_)) = "sí", type logical}})
```

### Cuándo usarlo y cuándo no
- **Asigna tipos al final** de la consulta, en un solo paso consolidado, después de limpiar; y otro temprano solo si una transformación lo necesita (filtrar por fecha, por ejemplo).
- **Texto, no número**, para códigos postales, RFC, folios o claves con ceros a la izquierda (`"01234"`).
- **Fecha, no fecha/hora**, cuando el análisis es diario: reduce cardinalidad. Si necesitas la hora, sepárala en otra columna.
- **Cultura explícita** siempre que el origen sea un texto (CSV, Excel capturado a mano, API) y no confíes en la configuración regional de la PC.

### Errores comunes
- **El paso automático *Tipo cambiado***: al promover encabezados, Power Query detecta tipos con las primeras 200 filas y **escribe los nombres de columna en el código**. Si mañana el origen renombra o quita una columna, la actualización falla con *no se encontró la columna*; si las primeras filas eran enteros y después vienen decimales, se generan errores. Revisa ese paso, ajústalo o desactiva la detección automática en *Opciones > Carga de datos* para tipar tú.
- **Depender de la configuración regional** de la máquina: en la PC del autor funciona y en el Service (cultura distinta) no.
- **Decimal flotante para dinero** y totales con centavos de diferencia.
- **Cambiar el tipo antes de limpiar** (por ejemplo, convertir a número antes de quitar `"N/A"`) y llenar la columna de errores.

### Para profundizar
- [Tipos de datos en Power Query](https://learn.microsoft.com/es-mx/power-query/data-types)
- [Tipos de datos en Power BI Desktop](https://learn.microsoft.com/es-mx/power-bi/connect-data/desktop-data-types)
- [Table.TransformColumnTypes](https://learn.microsoft.com/es-mx/powerquery-m/table-transformcolumntypes)

Relacionado: [[Power Query - manejo de errores]], [[Power Query - limpieza de datos]].

## Ejercicios

Resuélvelos en papel o en la herramienta; la solución está plegada.

> [!question]- Escribe en M el paso que convierta `Cantidad` a entero, `Precio` a número y `Fecha` a fecha, en una tabla `Origen`.
> ```m
> Tipos = Table.TransformColumnTypes(Origen, {
>     {"Cantidad", Int64.Type},
>     {"Precio", type number},
>     {"Fecha", type date}
> })
> ```

> [!question]- La columna `Importe` trae textos como `"$1,250.50"`. Escribe una columna nueva numérica.
> ```m
> ConImporte = Table.AddColumn(Tipos, "ImporteNum",
>     each Number.From(Text.Remove([Importe], {"$", ","}), "en-US"), type number)
> ```
> `Text.Remove` quita los caracteres y `Number.From` con cultura `en-US` interpreta el punto decimal.

> [!question]- Una columna `Activo` trae "Sí"/"No". Conviértela a lógico.
> ```m
> T = Table.TransformColumns(Origen, {{"Activo", each _ = "Sí", type logical}})
> ```

## Autoevaluación

Contesta primero en voz alta o en papel; luego despliega la respuesta.

> [!question]- ¿Por qué es importante asignar tipos explícitos?
> Determina cómo se ordena, agrega y almacena la columna; un número como texto no se puede sumar y una fecha como texto no sirve para inteligencia de tiempo.

> [!question]- Un CSV con fechas `03/04/2025` se interpreta mal (abril vs marzo). ¿Cómo se corrige?
> Cambiar tipo *Usando configuración regional* (Using locale) y elegir la cultura correcta, p. ej. `es-MX` (día/mes) o `en-US` (mes/día). En M: `Table.TransformColumnTypes(t, {{"Fecha", type date}}, "es-MX")`.

> [!question]- ¿Diferencia entre *Número decimal* y *Número decimal fijo* (moneda)?
> Decimal es punto flotante (puede tener errores de redondeo). Decimal fijo guarda 4 decimales exactos; útil para importes.

> [!question]- Error común: el paso automático *Tipo cambiado* después de promover encabezados. ¿Qué riesgo tiene?
> Escribe los nombres de columnas fijos; si el origen cambia un nombre, la consulta falla. Además puede adivinar mal un tipo. Conviene revisarlo y dejar un solo paso de tipos al final o donde haga falta.

## Práctica

- [[Lab 2 - Limpieza y combinación de datos en Power Query]]

