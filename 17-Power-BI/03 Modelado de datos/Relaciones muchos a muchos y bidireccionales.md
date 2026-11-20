---
proyecto: Conocimiento
tipo: concepto
nivel: intermedio
progreso: pendiente
estado: pendiente
area: Power BI
tags: [power-bi]
---
# Relaciones muchos a muchos y bidireccionales

Parte de: [[17-Power-BI/03 Modelado de datos/_Índice Modelado de datos|Modelado de datos]]

> [!info] Cómo avanzar
> `progreso`: **pendiente** → **viendo** (estudiando) → **visto** (contestaste la autoevaluación sin abrir las respuestas) → **dominado** (hiciste el laboratorio o lo aplicaste de verdad).

## Contenido

### Qué es
Dos temas relacionados que conviene dominar porque son la fuente más común de **totales incorrectos** y **modelos lentos**:

- **Relación muchos a muchos (\*:\*)**: los valores de la columna clave se **repiten en ambos lados**. Power BI la permite como cardinalidad de relación (es una relación *limitada*).
- **Filtro bidireccional** (*Ambas*): el filtro viaja también del lado muchos al lado uno, en lugar de solo de dimensión a hecho.

### Cómo funciona
**¿Cuándo aparece un muchos a muchos?** En dos situaciones distintas:

1. **Granos distintos**: dos tablas se relacionan por un atributo que no es clave única en ninguna (Presupuesto por `Categoria` con Productos, donde `Categoria` se repite). Power BI propone `*:*`.
2. **Muchos a muchos clásico**: una entidad se asocia con muchas de otra y viceversa. Ejemplos: una cuenta bancaria con varios titulares y un cliente con varias cuentas; una póliza con varios asegurados; un siniestro con varios ajustadores; un producto en varias promociones.

**Patrón recomendado para el clásico**: una **tabla puente** (*bridge*) con una fila por pareja.

```mermaid
flowchart LR
  Cl[Clientes] -- "1:*" --> B[ClienteCuenta<br/>puente]
  B -- "*:1  ambas" --> Cu[Cuentas]
  Cu -- "1:*" --> M[Movimientos]
```

- `Clientes` 1:\* `ClienteCuenta` (simple).
- `ClienteCuenta` \*:1 `Cuentas`, con dirección **Ambas** en esta relación (para que el filtro de cliente llegue a cuentas a través del puente). Alternativa más controlada: dejarla simple y activar la dirección solo en las medidas con `CROSSFILTER(..., BOTH)`.
- `Cuentas` 1:\* `Movimientos`.

Filtrar al cliente Ana muestra los movimientos de todas sus cuentas. El total general **no** es la suma de los clientes: una cuenta compartida aparece en ambos clientes, pero el total la cuenta una vez (comportamiento "no aditivo", correcto, pero que hay que explicar al usuario).

**¿Qué riesgo tienen las bidireccionales?**
- **Ambigüedad**: con varias relaciones bidireccionales pueden existir dos caminos entre las mismas tablas; Power BI desactiva relaciones o el resultado depende del camino.
- **Resultados inesperados**: filtrar un hecho termina filtrando dimensiones, y estas a otros hechos (por ejemplo, seleccionar un producto en un visual de ventas reduce los clientes en un visual de cobranza).
- **Rendimiento**: más propagación de filtros en cada consulta.
- **RLS**: los filtros de seguridad no cruzan por defecto en la dirección inversa; hay que habilitarlo explícitamente.

### Ejemplo
Segmentación de productos que solo muestre productos **con ventas** en el periodo: en lugar de poner `Productos`–`Ventas` como bidireccional, filtra el visual de segmentación con una medida:

```dax
Tiene Ventas = INT ( NOT ISEMPTY ( Ventas ) )
```

y en el panel de filtros de la segmentación: `Tiene Ventas = 1`.

Medida con puente sin dejar la relación bidireccional en el modelo:

```dax
Saldo Clientes =
CALCULATE (
    SUM ( Movimientos[Importe] ),
    CROSSFILTER ( ClienteCuenta[IdCuenta], Cuentas[IdCuenta], BOTH )
)
```

### Cuándo usarlo y cuándo no
- **Puente + una relación bidireccional controlada** para muchos a muchos clásico.
- **Cardinalidad \*:\*** para relacionar hechos de grano superior (presupuesto) con dimensiones, aunque una dimensión intermedia (`Categorias`) suele ser más clara.
- **Bidireccional** solo en relaciones de puente o dimensiones muy concretas; nunca como configuración general.
- Si solo necesitas el efecto en una medida, usa `CROSSFILTER` o `TREATAS` en lugar de cambiar el modelo.

### Errores comunes
- **Relacionar dos hechos** con \*:\* (Ventas con Presupuesto por producto).
- **Bidireccional en todas las relaciones** "para que funcionen las segmentaciones".
- **No explicar el total no aditivo** en muchos a muchos y que el usuario sume a mano.
- **Puentes con duplicados** (la misma pareja dos veces) que duplican importes.

### Para profundizar
- [Guía de relaciones de muchos a muchos](https://learn.microsoft.com/es-mx/power-bi/guidance/relationships-many-to-many)
- [Guía de relaciones bidireccionales](https://learn.microsoft.com/es-mx/power-bi/guidance/relationships-bidirectional-filtering)
- [CROSSFILTER (DAX)](https://learn.microsoft.com/es-mx/dax/crossfilter-function-dax)

Relacionado: [[Relaciones (cardinalidad y dirección del filtro)]], [[DAX - USERELATIONSHIP, TREATAS y CROSSFILTER]].

## Autoevaluación

Contesta primero en voz alta o en papel; luego despliega la respuesta.

> [!question]- ¿Cuándo aparece una relación muchos a muchos?
> Cuando ninguna de las dos columnas es única (p. ej. Presupuesto por categoría relacionado con Productos por categoría) o en relaciones puente (clientes ↔ cuentas compartidas).

> [!question]- ¿Qué riesgo tienen las relaciones bidireccionales?
> Ambigüedad (varias rutas de filtro), resultados inesperados, peor rendimiento y problemas con RLS. Usarlas solo cuando hacen falta y preferir `CROSSFILTER` en medidas puntuales.

> [!question]- Patrón recomendado para muchos a muchos clásico.
> Tabla puente: Dimensión A 1:* Puente *:1 Dimensión B, con el filtro de la puente hacia la dimensión que filtra los hechos (bidireccional solo en ese tramo o con CROSSFILTER).

