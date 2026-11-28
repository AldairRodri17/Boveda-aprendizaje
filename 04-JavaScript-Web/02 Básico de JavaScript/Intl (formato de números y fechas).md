---
proyecto: Conocimiento
tipo: concepto
nivel: intermedio
progreso: pendiente
estado: pendiente
area: JavaScript y Web
tags: [javascript-web]
contenido: completo
---
# Intl (formato de números y fechas)

Parte de: [[04-JavaScript-Web/02 Básico de JavaScript/_Índice Básico de JavaScript|Básico de JavaScript]]

> [!info] Cómo avanzar
> `progreso`: **pendiente** → **viendo** (estudiando) → **visto** (contestaste la autoevaluación sin abrir las respuestas) → **dominado** (hiciste el laboratorio o lo aplicaste de verdad).

## Contenido

### Qué es
`Intl` es el espacio de nombres de la **API de internacionalización** de JavaScript. Formatea números, monedas, porcentajes, fechas, tiempos relativos, listas y plurales, y compara cadenas según las reglas de cada idioma y país (*locale*), sin bibliotecas externas. Para una aplicación en México, resuelve "$1,234.50", "15 de noviembre de 2024" o "hace 3 días" con los separadores y palabras correctos.

### Cómo funciona
Cada funcionalidad es un **constructor** que recibe un *locale* (etiqueta BCP 47 como `'es-MX'`) y un objeto de opciones, y devuelve un formateador reutilizable:

| Constructor | Para qué |
|---|---|
| `Intl.NumberFormat` | Números, moneda, porcentaje, unidades, notación compacta |
| `Intl.DateTimeFormat` | Fechas y horas, con zona horaria |
| `Intl.RelativeTimeFormat` | "ayer", "en 2 horas", "hace 3 días" |
| `Intl.ListFormat` | "a, b y c" |
| `Intl.PluralRules` | Elegir singular/plural |
| `Intl.Collator` | Ordenar y comparar cadenas por idioma (ver [[sort]]) |

Los datos de idioma vienen del navegador o de Node (ICU), así que el texto exacto puede variar levemente entre versiones (un espacio, "p.m." vs "p. m."). No compares cadenas formateadas en pruebas unitarias sin fijar el entorno.

### Ejemplos
```js
const mxn = new Intl.NumberFormat('es-MX', { style: 'currency', currency: 'MXN' });
mxn.format(1234567.891);           // "$1,234,567.89"

const pct = new Intl.NumberFormat('es-MX', {
  style: 'percent', minimumFractionDigits: 1, maximumFractionDigits: 1,
});
pct.format(0.1234);                // "12.3%" (el porcentaje se pasa como fracción)

new Intl.NumberFormat('es-MX', { notation: 'compact' }).format(1500000); // "1.5 M"

const rel = new Intl.RelativeTimeFormat('es-MX', { numeric: 'auto' });
rel.format(-1, 'day');             // "ayer"
rel.format(2, 'hour');             // "dentro de 2 horas"
// con numeric: 'always' (por defecto) diría "hace 1 día"

new Intl.ListFormat('es-MX', { type: 'conjunction' }).format(['Ana', 'Luis', 'Eva']); // "Ana, Luis y Eva"
```

`numeric: 'auto'` permite palabras como "ayer", "mañana" u "hoy" en lugar de números.

### Fechas en una zona fija
`Date` guarda un instante en UTC (ver [[Fechas en JS]]); la zona se aplica **al formatear**. Con la opción `timeZone` muestras la hora de México aunque el usuario tenga su equipo en otra zona:

```js
const fmtFecha = new Intl.DateTimeFormat('es-MX', {
  dateStyle: 'long',
  timeStyle: 'short',
  timeZone: 'America/Mexico_City',
});
fmtFecha.format(new Date('2024-11-15T18:30:00Z')); // "15 de noviembre de 2024 a las 12:30 p.m."

// Partes sueltas para armar un formato propio
fmtFecha.formatToParts(new Date()); // [{type:'day', value:'15'}, ...]
```

Usa identificadores IANA (`America/Mexico_City`, `America/Tijuana`, `America/Cancun`), no abreviaturas como "CST".

### Reutiliza el formateador
Crear un `Intl.NumberFormat` o `DateTimeFormat` es relativamente **costoso**: tiene que resolver el locale, cargar datos y preparar patrones. `format()` en cambio es barato. Si formateas cada celda de una tabla de 5,000 renglones creando un formateador nuevo por celda, lo notarás. Crea los formateadores una vez (a nivel de módulo) y reutilízalos:

```js
// formatos.js
export const moneda = new Intl.NumberFormat('es-MX', { style: 'currency', currency: 'MXN' });
export const fecha = new Intl.DateTimeFormat('es-MX', { dateStyle: 'medium', timeZone: 'America/Mexico_City' });

// pedidos-view.js
import { moneda, fecha } from './formatos.js';
celda.textContent = moneda.format(pedido.total);
```

`n.toLocaleString('es-MX', {...})` y `d.toLocaleDateString(...)` son atajos cómodos para casos aislados, pero internamente pueden crear el formateador en cada llamada.

### Cuándo usarlo y cuándo no
Úsalo para **todo lo que se muestra** al usuario: montos, fechas, porcentajes. No lo uses para datos que viajan a la API o se guardan: ahí van números crudos e ISO 8601. Para redondeo financiero exacto, haz el cálculo con enteros (centavos) o en el backend con `decimal` de C#; `Intl` solo presenta.

### Errores comunes
- **Pasar 12.34 a `style: 'percent'`** esperando "12.34%": da "1,234%". Pasa la fracción (0.1234).
- **Concatenar símbolos a mano** (`'$' + n.toFixed(2)`): sin separadores de miles.
- **Crear formateadores en bucles**.
- **No fijar `timeZone`** cuando el negocio opera en una zona concreta.
- **Comparar texto formateado** en pruebas sin fijar locale y versión.

### Para profundizar
- [`Intl` (MDN)](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/Intl)
- [`Intl.NumberFormat` (MDN)](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/Intl/NumberFormat)
- [`Intl.DateTimeFormat` (MDN)](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/Intl/DateTimeFormat)
- [ECMA-402, especificación de Intl](https://tc39.es/ecma402/)

## Ejercicios

Resuélvelos en papel o en la herramienta; la solución está plegada.

> [!question]- Formatea `1234567.891` como pesos mexicanos.
> `new Intl.NumberFormat('es-MX', { style: 'currency', currency: 'MXN' }).format(1234567.891)` → `$1,234,567.89`.

> [!question]- Formatea `0.1234` como porcentaje con un decimal en es-MX.
> `new Intl.NumberFormat('es-MX', { style: 'percent', maximumFractionDigits: 1 }).format(0.1234)` → `12.3%`.

> [!question]- Muestra "ayer" para `-1` día.
> `new Intl.RelativeTimeFormat('es', { numeric: 'auto' }).format(-1, 'day')` → `ayer`.

## Autoevaluación

Contesta primero en voz alta o en papel; luego despliega la respuesta.

> [!question]- ¿Para qué sirve `Intl`?
> Formatear números, monedas, fechas, listas, plurales y tiempos relativos según el idioma y la región, sin librerías.

> [!question]- ¿Por qué crear el formateador una vez y reutilizarlo?
> `new Intl.NumberFormat(...)` es costoso de construir; en listas largas créalo fuera del bucle y llama `.format()` muchas veces.

> [!question]- ¿Cómo muestras una fecha en la zona de México sin importar la del usuario?
> Con la opción `timeZone: 'America/Mexico_City'` en `Intl.DateTimeFormat`.

