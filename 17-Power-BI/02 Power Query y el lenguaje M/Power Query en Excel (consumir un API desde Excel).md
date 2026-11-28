---
proyecto: Conocimiento
tipo: concepto
nivel: intermedio
progreso: pendiente
estado: pendiente
area: Power BI
tags: [power-bi]
contenido: completo
---
# Power Query en Excel (consumir un API desde Excel)

Parte de: [[17-Power-BI/02 Power Query y el lenguaje M/_Índice Power Query y el lenguaje M|Power Query y el lenguaje M]]

> [!info] Cómo avanzar
> `progreso`: **pendiente** → **viendo** (estudiando) → **visto** (contestaste la autoevaluación sin abrir las respuestas) → **dominado** (hiciste el laboratorio o lo aplicaste de verdad).

## Contenido

### Qué es
Power Query también vive dentro de **Excel**: es el mismo motor y el mismo lenguaje M que en Power BI. En Excel para Windows (Microsoft 365) está en la pestaña **Datos**, grupo **Obtener y transformar datos** (*Obtener datos*, *Desde la web*, *Desde tabla o rango*, *Consultas y conexiones*). Excel para Mac y Excel para la web también tienen Power Query, con un conjunto de conectores más limitado que va creciendo (Pendiente de confirmar qué conectores exactos hay hoy en Mac y web).

### Cómo funciona
1. *Datos > Obtener datos > De otras fuentes > De la web* (o *Consulta en blanco* para escribir M).
2. Se abre el mismo **Editor de Power Query**: pasos aplicados, barra de fórmulas, editor avanzado.
3. Al terminar, *Cerrar y cargar en…* te deja elegir el destino:
   - **Tabla** en una hoja (lo más común).
   - **Solo conexión** (la consulta existe para otras, sin volcar datos).
   - **Modelo de datos** (Power Pivot), el mismo motor tabular que Power BI, donde puedes crear relaciones y medidas DAX.
   - **Tabla dinámica**.
4. *Datos > Actualizar todo* vuelve a ejecutar las consultas. Puedes configurar actualización al abrir el archivo o cada N minutos en *Propiedades de la conexión*.

**Diferencias prácticas frente a Power BI**:

| Aspecto | Excel | Power BI |
|---|---|---|
| Destino | Hoja (límite de 1 048 576 filas), modelo de datos o conexión | Modelo semántico |
| Actualización programada | No hay de serie; manual, al abrir o con Power Automate/Office Scripts | Service con puerta de enlace |
| Credenciales | Por usuario en su Excel | Centralizadas en el Service |
| Compartir | El archivo (todos necesitan acceso al origen) | Informe publicado con permisos |
| Conectores | Menos que Power BI, sobre todo en Mac/web | El catálogo completo |
| Parámetros | Se suelen leer desde una celda con nombre | Parámetros de Power Query |

### Ejemplo
Un analista de cobranza quiere el tipo de cambio de un API dentro de su Excel de cartera, para calcular saldos en pesos.

```m
// Consulta "TipoCambio" (Datos > Obtener datos > Consulta en blanco > Editor avanzado)
let
    // fecha leída desde una celda con nombre "FechaCorte" en la hoja Parametros
    Fecha = Excel.CurrentWorkbook(){[Name = "FechaCorte"]}[Content]{0}[Column1],
    Json = Json.Document(Web.Contents("https://api.ejemplo.mx",
        [RelativePath = "tipocambio", Query = [fecha = Date.ToText(Date.From(Fecha), "yyyy-MM-dd")]])),
    Tabla = Table.FromRecords(Json[datos]),
    Tipos = Table.TransformColumnTypes(Tabla, {{"fecha", type date}, {"moneda", type text}, {"tasa", type number}})
in
    Tipos
```

*Cerrar y cargar en > Tabla* en la hoja `TC`. La hoja de cartera hace `BUSCARX` contra esa tabla. Cambiar `FechaCorte` y *Actualizar todo* trae la tasa nueva. (El API es ilustrativo.)

**¿Se puede copiar una consulta de Excel a Power BI?** Sí:
- En el panel *Consultas y conexiones* de Excel, clic derecho > **Copiar**; en el Editor de Power Query de Power BI, **Pegar** en el panel de consultas. Copia también las consultas de las que depende.
- O copia el M del editor avanzado.
- En Power BI Desktop, *Archivo > Importar > Power Query, Power Pivot, Power View* importa consultas y modelo de un libro completo.
Ajusta lo que dependa de Excel: `Excel.CurrentWorkbook()` no existe en Power BI; ahí se reemplaza por un parámetro o por leer el libro con `Excel.Workbook(File.Contents(...))`.

### Cuándo usarlo y cuándo no
- **Excel** cuando el resultado es un archivo de trabajo personal o de un equipo pequeño, cuando hay que seguir calculando con fórmulas, o como paso previo a un reporte (Wee.Reporteria).
- **Power BI** cuando el resultado lo consultan muchas personas, necesita actualización programada, seguridad por fila o volúmenes por arriba del millón de filas.
- Carga al **modelo de datos** en vez de a la hoja si pasas de unos cientos de miles de filas.

### Errores comunes
- **Cargar a hoja** millones de filas y chocar con el límite.
- **Compartir el Excel** y que los demás no tengan credenciales o acceso al API.
- **Rutas o celdas con nombre** que se renombran y rompen la consulta.
- **Copiar a Power BI** sin reemplazar `Excel.CurrentWorkbook()`.

### Para profundizar
- [Acerca de Power Query en Excel (soporte de Microsoft)](https://support.microsoft.com/office/7104fbee-9e62-4cb9-a02e-5bfb1a6c536a)
- [¿Qué es Power Query?](https://learn.microsoft.com/es-mx/power-query/power-query-what-is-power-query)
- [Importar libros de Excel en Power BI Desktop](https://learn.microsoft.com/es-mx/power-bi/connect-data/desktop-import-excel-workbooks)

Relacionado: [[Power Query - consumir un API]], [[Power Query - parámetros]].

## Autoevaluación

Contesta primero en voz alta o en papel; luego despliega la respuesta.

> [!question]- ¿Dónde está Power Query en Excel?
> *Datos → Obtener datos* (Obtener y transformar). Es el mismo motor y lenguaje M que en Power BI.

> [!question]- ¿Qué diferencias prácticas hay frente a Power BI?
> En Excel el resultado se carga a una tabla de hoja o al modelo de datos (Power Pivot); la actualización es manual, al abrir o por intervalo, no programada en un servicio. Algunos conectores difieren.

> [!question]- ¿Se puede copiar una consulta de Excel a Power BI?
> Sí: copiar el código del Editor avanzado (o copiar la consulta) y pegarla en Power BI; revisar rutas y credenciales.

## Práctica

- [[Lab 3 - Consumir un API en Power Query]]

