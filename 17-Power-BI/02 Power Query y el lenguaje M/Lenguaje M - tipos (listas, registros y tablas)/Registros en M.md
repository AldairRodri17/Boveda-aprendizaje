---
proyecto: Conocimiento
tipo: concepto
nivel: intermedio
progreso: pendiente
estado: pendiente
area: Power BI
tags: [power-bi]
---
# Registros en M

Parte de: [[Lenguaje M - tipos (listas, registros y tablas)]]

> [!info] Cómo avanzar
> `progreso`: **pendiente** → **viendo** (estudiando) → **visto** (contestaste la autoevaluación sin abrir las respuestas) → **dominado** (hiciste el laboratorio o lo aplicaste de verdad).

## Contenido

### Qué es
Un **registro** en M es un conjunto de **campos con nombre**, cada uno con su valor, escrito entre corchetes: `[Id = 7, Nombre = "Café", Precio = 45.5]`. Es como un objeto anónimo de C# o un objeto JSON. Cada **fila** de una tabla es un registro, y cuando lees un JSON con `Json.Document`, los objetos `{ }` del JSON se convierten en registros.

### Cómo funciona
**Construcción**: `[campo = valor, ...]`. Los nombres con espacios se escriben `#"Nombre con espacio"`. Un registro vacío es `[]`.

**Acceso a campos**:
- `r[Nombre]` → el valor. Si el campo **no existe**, da **error**.
- `r[Nombre]?` → **acceso opcional**: devuelve `null` si el campo no existe. Indispensable con JSON de APIs, donde un campo puede venir o no.
- `Record.Field(r, "Nombre")` → lo mismo pero con el nombre como **texto**, útil cuando el nombre del campo está en una variable o en otra columna (acceso dinámico).
- `Record.FieldOrDefault(r, "Nombre", valorPorOmision)` → versión segura de la anterior.

**Funciones útiles**:

| Función | Qué hace |
|---|---|
| `Record.FieldNames(r)` | Lista de nombres de campos |
| `Record.FieldValues(r)` | Lista de valores |
| `Record.HasFields(r, "Campo")` | ¿Existe el campo? |
| `Record.AddField`, `Record.RemoveFields`, `Record.RenameFields` | Modificar (devuelven un registro nuevo) |
| `Record.ToTable(r)` | Tabla de dos columnas `Name` y `Value` |
| `Record.Combine({r1, r2})` | Une registros (el último gana en campos repetidos) |

Operador `&` combina registros: `[a = 1] & [b = 2]` = `[a = 1, b = 2]`.

Dentro de un `each` en una tabla, el registro es `_` (la fila actual), y `[Cantidad]` es simplemente `_[Cantidad]`.

### Ejemplo
Respuesta de un API de pólizas donde `vigencia` a veces no viene:

```m
let
    Json = Json.Document(Web.Contents("https://api.ejemplo.mx", [RelativePath = "polizas/123"])),
    // Json es un registro: [id = 123, ramo = "Autos", vigencia = [inicio = "...", fin = "..."]]
    Ramo = Json[ramo],
    Inicio = Json[vigencia]?[inicio]?,      // null si no hay vigencia o inicio
    Campos = Record.FieldNames(Json)
in
    [Ramo = Ramo, Inicio = Inicio, Campos = Campos]
```

Acceso dinámico: una tabla de configuración dice qué campo usar como importe según el ramo:

```m
let
    Mapa = [Autos = "MontoDanios", Vida = "SumaAsegurada", GMM = "MontoReclamado"],
    ConImporte = Table.AddColumn(Siniestros, "Importe", each
        Record.Field(_, Record.Field(Mapa, [Ramo])), type number)
in
    ConImporte
```

Registros como **configuración** de funciones, que verás por todos lados:

```m
Csv.Document(binario, [Delimiter = ";", Encoding = 65001, QuoteStyle = QuoteStyle.Csv])
Web.Contents(url, [Headers = [Accept = "application/json"], Query = [page = "1"]])
```

### Cuándo usarlo y cuándo no
- **Úsalos** para leer JSON, para opciones de funciones, para devolver varios valores desde una función (`[Total = ..., Filas = ...]`) y para búsquedas tipo diccionario pequeño.
- **Usa `?`** siempre que leas JSON de un tercero.
- **Usa `Record.Field`** cuando el nombre del campo no se conoce al escribir el código.
- **No los uses** como tablas: si tienes muchas "filas", conviértelas a tabla para que se puedan filtrar y cargar.

### Errores comunes
- **`r[campo]` sobre campos opcionales** y errores intermitentes: algunos registros traen el campo y otros no.
- **Mayúsculas**: `r[Nombre]` no encuentra `nombre`.
- **Confundir `r[Campo]` con `Record.Field(r, Campo)`**: el segundo espera texto entre comillas o una variable de texto.
- **Expandir columnas de registros** en la interfaz con la lista de campos fija: si el API agrega campos nuevos, no aparecen.

### Para profundizar
- [Funciones de registro](https://learn.microsoft.com/es-mx/powerquery-m/record-functions)
- [Record.Field](https://learn.microsoft.com/es-mx/powerquery-m/record-field)
- [Acceso a campos en la especificación (operador ?)](https://learn.microsoft.com/es-mx/powerquery-m/m-spec-operators)

Relacionado: [[Listas en M]], [[Tablas en M]], [[Power Query - consumir un API]].

## Autoevaluación

Contesta primero en voz alta o en papel; luego despliega la respuesta.

> [!question]- ¿Qué es un registro?
> Un conjunto de pares campo = valor, como una fila. `[Nombre = "Ana"]`.

> [!question]- ¿Cómo lees un campo que puede no existir?
> `Registro[Campo]?` devuelve null si no existe (en vez de error).

> [!question]- ¿Para qué sirve `Record.Field(r, "Campo")`?
> Para leer un campo cuyo nombre está en una variable de texto.

