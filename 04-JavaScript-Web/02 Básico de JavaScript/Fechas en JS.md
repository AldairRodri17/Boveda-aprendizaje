---
proyecto: Conocimiento
tipo: concepto
nivel: basico
progreso: pendiente
estado: pendiente
area: JavaScript y Web
tags: [javascript-web]
contenido: completo
---
# Fechas en JS

Parte de: [[04-JavaScript-Web/02 Básico de JavaScript/_Índice Básico de JavaScript|Básico de JavaScript]]

> [!info] Cómo avanzar
> `progreso`: **pendiente** → **viendo** (estudiando) → **visto** (contestaste la autoevaluación sin abrir las respuestas) → **dominado** (hiciste el laboratorio o lo aplicaste de verdad).

## Contenido

### Qué es
`Date` es el objeto nativo de JavaScript para fechas y horas. Por dentro es solo **un número**: los **milisegundos transcurridos desde el 1 de enero de 1970 a las 00:00:00 UTC** (la *época Unix*). No guarda zona horaria: los métodos "locales" (`getHours`, `getDate`) traducen ese instante a la zona del equipo que ejecuta el código, y los métodos `getUTC*` lo muestran en UTC.

### Cómo funciona
**Crear fechas**
```js
new Date();                          // ahora
new Date(0);                         // 1970-01-01T00:00:00.000Z
new Date(2024, 0, 15);               // 15 de enero de 2024, hora LOCAL
new Date(Date.UTC(2024, 0, 15));     // 15 de enero de 2024 00:00 UTC
new Date('2024-03-10T12:00:00Z');    // cadena ISO con zona
Date.now();                          // número de milisegundos actual
```

**Meses de 0 a 11.** En el constructor numérico y en `getMonth()`, enero es `0` y diciembre es `11`. Los días sí empiezan en 1. Si te pasas, `Date` **se desborda** al siguiente: `new Date(2024, 0, 32)` es 1 de febrero. Ese desbordamiento es útil: el **día 0** de un mes es el último día del mes anterior.

```js
new Date(2024, 2, 0).getDate();      // 29 → febrero de 2024 tiene 29 días (bisiesto)
```

**Cadenas: la trampa de la zona horaria.** Según la especificación:
- Una cadena ISO de **solo fecha** (`'2024-03-10'`) se interpreta en **UTC**.
- Una cadena ISO con **fecha y hora sin zona** (`'2024-03-10T00:00'`) se interpreta en **hora local**.

En la Ciudad de México (UTC−6), `new Date('2024-03-10')` es el 10 de marzo a las 00:00 UTC, que localmente es **9 de marzo a las 18:00**. Si muestras `getDate()`, verás 9. Es el error de "la fecha sale un día antes". Otros formatos de cadena (`'03/10/2024'`) dependen del navegador: no los uses.

**Diferencias entre fechas.** Restar dos `Date` da milisegundos:

```js
const a = new Date('2024-01-01T00:00:00Z');
const b = new Date('2024-03-01T00:00:00Z');
(b - a) / (1000 * 60 * 60 * 24);     // 60 días (31 de enero + 29 de febrero)
```

Haz estas cuentas en **UTC**: en zonas con horario de verano, un día local puede tener 23 o 25 horas.

**Salida**
- `toISOString()`: siempre UTC, `'2024-03-10T00:00:00.000Z'`. Ideal para enviar a la API y guardar.
- Para mostrar al usuario, usa `Intl.DateTimeFormat` o `toLocaleDateString('es-MX', {...})` (ver [[Intl (formato de números y fechas)]]).
- `JSON.stringify` convierte un `Date` a ISO, pero `JSON.parse` **no** lo regresa a `Date` (ver [[JSON.stringify y JSON.parse]]).

### Ejemplo
Recibir fechas de Itzamna.Dionisio (que serializa `DateTimeOffset` en ISO) y mostrarlas:

```js
const fmt = new Intl.DateTimeFormat('es-MX', {
  dateStyle: 'medium', timeStyle: 'short', timeZone: 'America/Mexico_City',
});

const pedido = await (await fetch('/api/pedidos/1024')).json();
const creado = new Date(pedido.creadoEn);       // "2024-11-15T18:30:00Z"
etiqueta.textContent = fmt.format(creado);       // algo como "15 nov 2024, 12:30 p.m."

// Para un <input type="date"> (valor 'AAAA-MM-DD' sin hora) no conviertas a Date
// si solo necesitas la fecha: guárdala como cadena para evitar el corrimiento de zona.
const fechaEntrega = inputFecha.value;           // '2024-11-20'
```

Regla de oro: **guarda y transmite instantes en UTC (ISO 8601)**, convierte a la zona del usuario **solo al mostrar**, y trata las "fechas de calendario" (cumpleaños, fecha de entrega) como cadenas `AAAA-MM-DD` o con un tipo específico.

### Temporal, la alternativa moderna
`Temporal` es la nueva API estándar de fechas de ECMAScript que resuelve los problemas de `Date`: objetos inmutables, meses de 1 a 12, tipos separados para cada caso (`Temporal.PlainDate` para una fecha de calendario, `Temporal.ZonedDateTime` para un instante con zona, `Temporal.Instant`, `Temporal.Duration`) y aritmética correcta con horarios de verano. Llegó a Firefox 139 y a Chrome/Edge 144 (enero de 2026); en Safari estable el soporte está (Pendiente de confirmar). Mientras no esté en todos tus navegadores objetivo, existe un *polyfill* oficial.

```js
// Donde esté disponible
const hoy = Temporal.Now.plainDateISO('America/Mexico_City');
const entrega = hoy.add({ days: 5 });
entrega.toString();                  // 'AAAA-MM-DD'
```

### Cuándo usarlo y cuándo no
`Date` está bien para marcas de tiempo simples, medir duraciones y convertir ISO de la API. Para cálculos de calendario (sumar meses, zonas horarias, recurrencias) usa Temporal donde haya soporte, o una biblioteca ligera (date-fns, Luxon, Day.js).

### Errores comunes
- **Meses base 0** en `new Date(a, m, d)`.
- **Fecha corrida un día** por interpretar `'AAAA-MM-DD'` en UTC y mostrarla en local.
- **Mutar un `Date` compartido** con `setDate`: los `Date` son mutables; copia con `new Date(d)`.
- **Parsear formatos no ISO** como `'15/11/2024'`.

### Para profundizar
- [`Date` (MDN)](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/Date)
- [Formato de cadena de fecha y hora (MDN)](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/Date#date_time_string_format)
- [Documentación de Temporal (TC39)](https://tc39.es/proposal-temporal/docs/)
- [`Temporal` (MDN)](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/Temporal)

## Ejercicios

Resuélvelos en papel o en la herramienta; la solución está plegada.

> [!question]- ¿Qué imprime este código?
> ```js
> const d = new Date(2024, 0, 31);
> d.setMonth(1);
> console.log(d.getMonth(), d.getDate());
> ```
>
> > [!success]- Respuesta
> > `2 2`: el 31 de febrero no existe y se desborda al 2 de marzo (2024 es bisiesto).

> [!question]- ¿Cuántos días tiene febrero de 2024? Calcúlalo con `Date`.
> `new Date(2024, 2, 0).getDate()` → `29` (el día 0 de marzo es el último de febrero).

> [!question]- ¿Cuántos días hay entre `2024-01-01T00:00:00Z` y `2024-03-01T00:00:00Z`?
> `(b - a) / 86400000` → `60`.

## Autoevaluación

Contesta primero en voz alta o en papel; luego despliega la respuesta.

> [!question]- ¿Qué rango tienen los meses en `new Date(año, mes, día)`?
> De 0 a 11: `new Date(2024, 11, 25)` es 25 de diciembre. Los días sí van de 1 a 31.

> [!question]- ¿En qué zona horaria se interpreta `new Date('2024-03-10')` y `new Date('2024-03-10T00:00')`?
> La sola fecha (`AAAA-MM-DD`) se toma como **UTC**; con hora y sin zona se toma como **hora local**. Por eso una fecha puede "salir un día antes" en México.

> [!question]- ¿Qué es un `Date` por dentro?
> Un número: milisegundos desde 1970-01-01 UTC. Restar dos fechas da milisegundos.

> [!question]- ¿Qué alternativa moderna existe a `Date`?
> La API `Temporal` (tipos separados para fecha, hora, zona). Su disponibilidad por navegador: (Pendiente de confirmar); revisa compatibilidad antes de usarla sin polyfill.

