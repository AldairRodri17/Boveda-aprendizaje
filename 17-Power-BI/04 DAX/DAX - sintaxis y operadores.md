---
proyecto: Conocimiento
tipo: concepto
nivel: basico
progreso: pendiente
estado: pendiente
area: Power BI
tags: [power-bi]
---
# DAX - sintaxis y operadores

Parte de: [[17-Power-BI/04 DAX/_Índice DAX|DAX]]

> [!info] Cómo avanzar
> `progreso`: **pendiente** → **viendo** (estudiando) → **visto** (contestaste la autoevaluación sin abrir las respuestas) → **dominado** (hiciste el laboratorio o lo aplicaste de verdad).

## Contenido

### Qué es
La sintaxis de DAX es pequeña: nombres de objetos (tablas, columnas, medidas), funciones, operadores y constantes. Dominarla evita la mitad de los errores de principiante, sobre todo los relacionados con referencias ambiguas y con el valor especial **BLANK**.

### Cómo funciona
**Referencias**
- Columna: `'Tabla'[Columna]`, siempre con el nombre de la tabla: `Ventas[Cantidad]`. Las comillas simples son obligatorias si el nombre de la tabla tiene espacios o caracteres especiales: `'Detalle Ventas'[Cantidad]`.
- Medida: solo entre corchetes, **sin** tabla: `[Ventas $]`. Es la convención recomendada porque, al leer, distingues de inmediato una medida (que hace transición de contexto) de una columna.
- Tabla: `Ventas` o `'Detalle Ventas'`.

**Operadores**

| Tipo | Operadores | Nota |
|---|---|---|
| Aritméticos | `+ - * / ^` | `/` falla con cero; usa `DIVIDE` |
| Comparación | `= == <> < > <= >=` | `==` es igualdad estricta (BLANK no es igual a 0) |
| Texto | `&` | concatena |
| Lógicos | `&&` (y), `\|\|` (o), `NOT`, `IN` | `IN` compara contra una lista `{ }` |

`&&` y `||` son operadores binarios; equivalen a las funciones `AND(a, b)` y `OR(a, b)`, que solo aceptan dos argumentos. `IN` evita cadenas largas de `||`.

**BLANK** es el "vacío" de DAX (parecido a NULL, pero no igual). Reglas que conviene memorizar:
- En aritmética se comporta como 0: `BLANK() + 5` = 5; `BLANK() * 5` = BLANK.
- En comparaciones con `=`, BLANK se considera igual a 0 y a "" : `BLANK() = 0` es TRUE. Para distinguirlo usa `ISBLANK()` o `==`.
- Los visuales ocultan filas cuya medida es BLANK; por eso devolver 0 "rellena" filas (ver [[DAX - IF, SWITCH y DIVIDE]]).

**Otros elementos**: comentarios con `--`, `//` o `/* ... */`; constantes de tabla con llaves `{ "Rojo", "Azul" }`; fechas con `DATE(2025, 2, 14)` o el literal `dt"2025-02-14"`; separador de argumentos coma (en configuraciones regionales con coma decimal se usa punto y coma).

### Ejemplo
> [!example] Modelo de ejemplo de esta sección
> - `Productos`: P1 Café (Bebidas, Negro), P2 Molino (Equipo, Rojo), P3 Galletas (Alimentos, Café).
> - `Ventas` (Cantidad × PrecioUnitario, CostoUnitario): 1) 2024-01-15 P1 2×100 (60); 2) 2024-02-10 P2 1×250 (150); 3) 2024-02-20 P1 3×100 (60); 4) 2025-01-05 P3 5×40 (25); 5) 2025-02-14 P2 2×250 (150).
> - `Calendario` (Fecha, Año, NumMes, Mes, AñoMes, Trimestre) relacionado 1:* con `Ventas[Fecha]`; `Productos` 1:* con `Ventas[IdProducto]`.
> - Medida base: `Ventas $ = SUMX(Ventas, Ventas[Cantidad] * Ventas[PrecioUnitario])` → total **1 450**; Bebidas 500, Equipo 750, Alimentos 200; 2024 = 750, 2025 = 700.

```dax
-- Columna calculada en Productos: texto concatenado
Etiqueta = Productos[Producto] & " - " & Productos[Color]

-- Medida con IN y &&
Ventas Bebidas o Equipo 2025 =
CALCULATE(
    [Ventas $],
    Productos[Categoria] IN { "Bebidas", "Equipo" },
    Calendario[Año] = 2025
)
-- Resultado: solo la línea 5 (Molino 2×250) → 500

-- Prueba de BLANK
Prueba blank = IF(ISBLANK([Ventas $]), "Sin ventas", "Con ventas")
```

En 2025, la categoría Bebidas no tiene ventas: `[Ventas $]` da BLANK, por eso `Prueba blank` muestra "Sin ventas" en esa celda. Si comparas `[Ventas $] = 0` también obtendrías TRUE, lo que puede confundir: en ese caso no sabrías si hubo ventas por cero pesos o ninguna venta.

### Cuándo usarlo y cuándo no
- Usa `IN { }` cuando compares una columna contra 3 o más valores.
- Usa `&&` dentro de un mismo filtro cuando ambas condiciones son sobre la misma tabla; si son columnas de tablas distintas, pásalas como **argumentos separados** de CALCULATE (más claro y eficiente).
- Usa `==` o `ISBLANK` solo cuando la diferencia entre 0 y vacío importe.
- Para concatenar muchos valores de una tabla, `&` no basta: usa `CONCATENATEX` (ver [[DAX - VALUES y DISTINCT]]).

### Errores comunes
- **Escribir `Ventas[Ventas $]`** para una medida: funciona, pero confunde medidas con columnas y rompe si mueves la medida a otra tabla.
- **Olvidar las comillas** en tablas con espacios: `Detalle Ventas[Cantidad]` da error de sintaxis.
- **Suponer que BLANK = 0 siempre**: `BLANK() * 5` es BLANK y `BLANK() = 0` es TRUE; mezclar ambas reglas sin pensarlo genera resultados raros en semáforos y conteos.
- **Usar `AND()` con tres argumentos**: solo acepta dos; anida o usa `&&`.
- **Dividir con `/`**: provoca infinito o error si el denominador es 0; usa `DIVIDE`.

### Para profundizar
- [Referencia de sintaxis DAX](https://learn.microsoft.com/en-us/dax/dax-syntax-reference)
- [Operadores DAX](https://learn.microsoft.com/en-us/dax/dax-operator-reference)
- [Reglas de formato de código DAX (SQLBI)](https://www.sqlbi.com/articles/rules-for-dax-code-formatting/)

## Autoevaluación

Contesta primero en voz alta o en papel; luego despliega la respuesta.

> [!question]- ¿Cómo se referencia una columna y una medida?
> Columna: `Tabla[Columna]` (siempre con tabla). Medida: `[Medida]` (sin tabla). Esta convención hace legible el código.

> [!question]- ¿Qué hace el operador `&&`, `||` e `IN`?
> Y lógico, O lógico, y pertenencia a una lista: `Productos[Color] IN {"Rojo", "Azul"}`.

> [!question]- ¿Qué devuelve `BLANK() + 5` y `BLANK() = 0`?
> `5` (el blank se trata como 0 en la suma) y `TRUE` (en comparación con `=`, blank equivale a 0). Para comparación estricta usa `==`.

> [!question]- ¿Cómo se concatenan textos?
> Con `&`: `Productos[Producto] & " - " & Productos[Color]`.

