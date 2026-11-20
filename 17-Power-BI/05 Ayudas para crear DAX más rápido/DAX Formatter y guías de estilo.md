---
proyecto: Conocimiento
tipo: concepto
nivel: intermedio
progreso: pendiente
estado: pendiente
area: Power BI
tags: [power-bi]
---
# DAX Formatter y guías de estilo

Parte de: [[17-Power-BI/05 Ayudas para crear DAX más rápido/_Índice Ayudas para crear DAX más rápido|Ayudas para crear DAX más rápido]]

> [!info] Cómo avanzar
> `progreso`: **pendiente** → **viendo** (estudiando) → **visto** (contestaste la autoevaluación sin abrir las respuestas) → **dominado** (hiciste el laboratorio o lo aplicaste de verdad).

## Contenido

### Qué es
**DAX Formatter** es un servicio web **gratuito** de SQLBI que toma una fórmula DAX y la devuelve con formato consistente: sangrías, saltos de línea por argumento, mayúsculas en funciones y espacios uniformes. Detrás está una **guía de estilo** publicada por SQLBI que muchas herramientas aplican. El formato no cambia el resultado, pero hace el código legible, revisable en *pull requests* y fácil de depurar.

### Cómo funciona
- **En la web**: pegas la fórmula en daxformatter.com y eliges opciones (línea larga o corta, separadores regionales).
- **Integrado**: DAX Studio y Tabular Editor formatean usando DAX Formatter; la [[Vista de consultas DAX (DAX query view)]] de Desktop tiene su propio comando de formato (Mayús+Alt+F).
- Si tu modelo es confidencial, considera que el servicio web recibe el texto de la fórmula (no los datos); revisa las políticas de tu organización.

**Reglas de estilo comunes** (basadas en la guía de SQLBI):
1. **Funciones en mayúsculas** y un espacio antes del paréntesis de apertura en funciones con argumentos: `CALCULATE (`.
2. **Un argumento por línea** cuando la función no cabe en una línea, con sangría de 4 espacios; el paréntesis de cierre alineado con el inicio de la función.
3. **Columnas siempre con su tabla** (`Ventas[Cantidad]`) y **medidas siempre sin tabla** (`[Ventas $]`).
4. **Variables con nombres descriptivos** y `RETURN` en su propia línea.
5. Espacios alrededor de operadores, coma seguida de espacio.
6. Comentarios para explicar el *por qué*, no el *qué*.

Convenciones de equipo que conviene agregar: nombres de medidas en español y legibles para negocio (`Ventas $`, `Var vs AA %`), carpetas de visualización, prefijos para medidas auxiliares ocultas (`_Ventas base`) y descripción obligatoria.

### Ejemplo
> [!example] Modelo de ejemplo de esta sección
> - `Productos`: P1 Café (Bebidas, Negro), P2 Molino (Equipo, Rojo), P3 Galletas (Alimentos, Café).
> - `Ventas` (Cantidad × PrecioUnitario, CostoUnitario): 1) 2024-01-15 P1 2×100 (60); 2) 2024-02-10 P2 1×250 (150); 3) 2024-02-20 P1 3×100 (60); 4) 2025-01-05 P3 5×40 (25); 5) 2025-02-14 P2 2×250 (150).
> - `Calendario` (Fecha, Año, NumMes, Mes, AñoMes, Trimestre) relacionado 1:* con `Ventas[Fecha]`; `Productos` 1:* con `Ventas[IdProducto]`.
> - Medida base: `Ventas $ = SUMX(Ventas, Ventas[Cantidad] * Ventas[PrecioUnitario])` → total **1 450**; Bebidas 500, Equipo 750, Alimentos 200; 2024 = 750, 2025 = 700.

Antes:

```dax
Var vs AA %=var aa=calculate([Ventas $],sameperiodlastyear(Calendario[Fecha])) return if(not isblank(aa),divide([Ventas $]-aa,aa))
```

Después (estilo DAX Formatter):

```dax
Var vs AA % =
VAR VentasAA =
    CALCULATE (
        [Ventas $],
        SAMEPERIODLASTYEAR ( Calendario[Fecha] )
    )
RETURN
    IF (
        NOT ISBLANK ( VentasAA ),
        DIVIDE ( [Ventas $] - VentasAA, VentasAA )
    )
-- Febrero 2025: (500 − 550) / 550 = −9.09 %
```

Con el formato salta a la vista la estructura: una variable, una condición y una división segura. En la versión compacta es fácil no ver que faltaba un paréntesis o que el IF no tenía rama falsa.

### Cuándo usarlo y cuándo no
- **Siempre** antes de guardar una medida de más de una línea, y antes de compartir código en documentación, Teams o un *pull request*.
- En proyectos **PBIP/TMDL** versionados en Azure DevOps, el formato consistente reduce diferencias falsas entre versiones.
- Medidas triviales de una línea (`Num ventas = COUNTROWS ( Ventas )`) no necesitan saltos de línea.
- Si la política de la empresa impide enviar fórmulas a servicios externos, usa el formato integrado de Desktop o el de Tabular Editor 3.

### Errores comunes
- **Formatear a mano** cada quien a su estilo: sin una regla común, los diffs se vuelven ruido.
- **Columnas sin tabla o medidas con tabla**: el formato no lo corrige y confunde al leer.
- **Comentarios obsoletos** que ya no describen la fórmula.
- **Nombres crípticos** de variables (`x1`, `t`): el formato no compensa un mal nombre.

### Para profundizar
- [DAX Formatter](https://www.daxformatter.com/)
- [Rules for DAX code formatting (SQLBI)](https://www.sqlbi.com/articles/rules-for-dax-code-formatting/)
- [Usar variables para mejorar fórmulas DAX](https://learn.microsoft.com/en-us/dax/best-practices/dax-variables)

## Autoevaluación

Contesta primero en voz alta o en papel; luego despliega la respuesta.

> [!question]- ¿Qué hace DAX Formatter?
> Da formato legible a código DAX (sangrías, saltos de línea) según convenciones de SQLBI; Desktop también tiene opción de formato en el editor de consultas DAX.

> [!question]- Menciona tres reglas de estilo comunes.
> Columnas siempre con tabla (`Ventas[Cantidad]`), medidas sin tabla (`[Ventas $]`); un argumento por línea en funciones largas; variables con nombres descriptivos; medidas en carpetas de visualización.

