---
proyecto: Conocimiento
tipo: concepto
nivel: basico
progreso: pendiente
estado: pendiente
area: Fundamentos
tags: [fundamentos]
---
# Dinero y decimales

Parte de: [[02-Fundamentos/06 Datos y formatos/_Índice Datos y formatos|Datos y formatos]]

> [!info] Cómo avanzar
> `progreso`: **pendiente** → **viendo** (estudiando) → **visto** (contestaste la autoevaluación sin abrir las respuestas) → **dominado** (hiciste el laboratorio o lo aplicaste de verdad).

## Contenido

### Qué es
Representar dinero correctamente exige **aritmética decimal exacta**, reglas de **redondeo** explícitas y guardar siempre la **moneda** junto al monto. El error clásico es usar tipos de **punto flotante binario** (`float`, `double`, `number` de JavaScript), que no pueden representar exactamente valores como 0.10 o 0.20 y acumulan errores de centavos.

### Cómo funciona
**Por qué no `float`/`double`.** Son binarios (IEEE 754): representan sumas de potencias de 2. Un número como 0.1 no tiene representación binaria finita (como 1/3 en decimal), así que se guarda una aproximación:

```python
print(0.1 + 0.2)          # 0.30000000000000004
print(0.1 + 0.2 == 0.3)   # False
```

En una factura con cientos de partidas, esos errores se acumulan y el total no cuadra con la suma de los renglones ni con lo que calcula el SAT o el banco.

**Tipos correctos:**
- **C#**: `decimal` (128 bits, base 10, hasta 28–29 dígitos significativos). `0.1m + 0.2m == 0.3m` es `true`.
- **SQL Server**: `decimal(p, s)` / `numeric`. `DECIMAL(19,4)` es una elección común: 15 dígitos enteros y 4 decimales (las 4 posiciones permiten cálculos intermedios, como precios unitarios o tipos de cambio, antes de redondear a centavos). Evita el tipo `money` por sus problemas de precisión en divisiones.
- **PostgreSQL**: `numeric(19,4)`.
- **Python**: `decimal.Decimal`, creado **desde cadena**: `Decimal("0.1")`, no `Decimal(0.1)`, que copia el error del float.
- **JavaScript**: no tiene decimal nativo. Opciones: trabajar en **enteros de centavos**, recibir montos como **cadena** (ver [[JSON]]) y usar una librería decimal para cálculos, o dejar los cálculos al backend.

**Redondeo.** Cuando un valor queda exactamente a la mitad, hay dos convenciones:
- **Al par (redondeo bancario, `ToEven`)**: `2.345 → 2.34`, `2.355 → 2.36`, `2.5 → 2`, `3.5 → 4`. Reparte el sesgo hacia arriba y abajo; es el **modo por defecto** de `Math.Round` en .NET.
- **Alejándose de cero (comercial, `AwayFromZero`)**: `2.345 → 2.35`, `2.5 → 3`. Es el que la mayoría espera en tickets y facturas.

```csharp
Math.Round(2.345m, 2);                                // 2.34
Math.Round(2.345m, 2, MidpointRounding.AwayFromZero); // 2.35
```

Define en un solo lugar qué modo usa tu sistema y **dónde** se redondea (por partida o en el total), porque cambia el resultado.

**Repartir sin perder centavos.** Dividir $100.00 entre 3 da 33.333...; si redondeas cada parte a 33.33, sumas 99.99 y se pierde un centavo. El algoritmo correcto trabaja en **centavos enteros**:
1. 10000 centavos ÷ 3 = 3333 con residuo 1.
2. Todas las partes reciben 3333 y las primeras **residuo** partes reciben un centavo más.
3. Resultado: 33.34, 33.33, 33.33 (suma exacta 100.00).

Para $1,000.00 en 7 pagos: 100000 ÷ 7 = 14285 residuo 5 → cinco pagos de **142.86** y dos de **142.85**.

### Ejemplo
IVA (16 %) y total de $1,234.56, verificado:

```csharp
var subtotal = 1234.56m;
var iva = Math.Round(subtotal * 0.16m, 2, MidpointRounding.AwayFromZero); // 197.5296 → 197.53
var total = subtotal + iva;                                               // 1432.09
```

Reparto en Pochteca.Nomisma (pagos en parcialidades):

```csharp
public static IReadOnlyList<decimal> Repartir(decimal monto, int partes)
{
    ArgumentOutOfRangeException.ThrowIfNegativeOrZero(partes);
    var centavos = (long)Math.Round(monto * 100m, MidpointRounding.AwayFromZero);
    var (cociente, residuo) = Math.DivRem(centavos, partes);
    return Enumerable.Range(0, partes)
        .Select(i => (cociente + (i < residuo ? 1 : 0)) / 100m)
        .ToList();
}

Repartir(1000.00m, 7); // 142.86 ×5, 142.85 ×2 → suma 1000.00
Repartir(100.00m, 3);  // 33.34, 33.33, 33.33
```

Un tipo de valor evita mezclar monedas:

```csharp
public readonly record struct Dinero(decimal Monto, string Moneda) // Moneda: código ISO 4217 (MXN, USD)
{
    public static Dinero operator +(Dinero a, Dinero b) =>
        a.Moneda == b.Moneda ? new(a.Monto + b.Monto, a.Moneda)
                             : throw new InvalidOperationException("Monedas distintas");
}
```

En la base de datos, junto al monto guarda la **moneda** (`char(3)` con código ISO 4217) y, si hubo conversión, el **tipo de cambio** usado y su **fecha** y fuente (por ejemplo, el tipo de cambio FIX del Banco de México de ese día), para poder reproducir el cálculo meses después.

### Cuándo usarlo y cuándo no
- **`decimal` / `DECIMAL`**: siempre para dinero, cantidades fiscales, tipos de cambio y porcentajes que se aplican a dinero.
- **Enteros en centavos**: en JavaScript o en sistemas donde quieres aritmética exacta simple; ojo con monedas de 0 o 3 decimales.
- **`double`**: medidas físicas, estadística, gráficas, donde un error relativo mínimo es aceptable.

### Errores comunes
1. **`double` para montos.** Totales que no cuadran por centavos.
2. **`Decimal(0.1)` desde float** en Python. Hereda el error; usa cadenas.
3. **No definir el modo de redondeo.** El default de .NET es al par y sorprende a negocio.
4. **Redondear cada parte de un reparto** sin asignar el residuo. Se pierden o sobran centavos.
5. **Guardar montos sin moneda.** Imposible saber si 100 son pesos o dólares cuando el sistema crece.

### Para profundizar
- [Microsoft Learn: Math.Round y MidpointRounding](https://learn.microsoft.com/dotnet/api/system.math.round)
- [Documentación de Python: decimal](https://docs.python.org/3/library/decimal.html)
- [Microsoft Learn: decimal y numeric (Transact-SQL)](https://learn.microsoft.com/sql/t-sql/data-types/decimal-and-numeric-transact-sql)
- [ISO 4217: códigos de moneda](https://www.iso.org/iso-4217-currency-codes.html)

## Ejercicios

Resuélvelos en papel o en la herramienta; la solución está plegada.

> [!question]- ¿Cuánto da `Math.Round(2.345m, 2)` en C#? ¿Y con `MidpointRounding.AwayFromZero`?
> `2.34` (al par) y `2.35`.

> [!question]- Calcula el IVA (16 %) de $1,234.56 redondeado a centavos y el total.
> IVA = 197.5296 → **197.53**; total **1,432.09**.

> [!question]- Divide $1,000.00 en 7 pagos sin perder centavos.
> 100000 / 7 = 14285 resto 5: cinco pagos de **142.86** y dos de **142.85** (suma 1,000.00).

> [!question]- ¿Qué imprime en Python `0.1 + 0.2 == 0.3`? ¿Y con `Decimal`?
> `False`. Con `Decimal('0.1') + Decimal('0.2') == Decimal('0.3')` → `True` (crea los Decimal desde cadena, no desde float).

## Autoevaluación

Contesta primero en voz alta o en papel; luego despliega la respuesta.

> [!question]- ¿Por qué no usar `float`/`double` para dinero?
> Son binarios y no representan exactamente decimales como 0.1: `0.1 + 0.2 == 0.30000000000000004`. Usa `decimal` en C#, `DECIMAL(19,4)` en SQL, o enteros en centavos.

> [!question]- ¿Qué es el redondeo bancario?
> Redondear al par más cercano cuando el valor está exactamente a la mitad (`2.5 → 2`, `3.5 → 4`). Es el modo por defecto de `Math.Round` en .NET (`MidpointRounding.ToEven`); para "redondeo comercial" usa `MidpointRounding.AwayFromZero`.

> [!question]- ¿Cómo repartes $100.00 entre 3 sin perder centavos?
> Trabaja en centavos: 10000 / 3 = 3333 con residuo 1; dos partes de 33.33 y una de 33.34. La suma debe dar exactamente el total.

> [!question]- ¿Qué hay que guardar junto al monto?
> La **moneda** (código ISO 4217: `MXN`, `USD`) y, si hay conversiones, el tipo de cambio y su fecha.

