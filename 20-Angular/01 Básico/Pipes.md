---
proyecto: Conocimiento
tipo: concepto
nivel: basico
progreso: pendiente
estado: pendiente
area: Angular
tags: [angular]
contenido: completo
---
# Pipes

Parte de: [[20-Angular/01 Básico/_Índice Básico|Básico]]

> [!info] Cómo avanzar
> `progreso`: **pendiente** → **viendo** (estudiando) → **visto** (contestaste la autoevaluación sin abrir las respuestas) → **dominado** (hiciste el laboratorio o lo aplicaste de verdad).

## Contenido

### Qué es
Un **pipe** transforma un valor para mostrarlo en la plantilla, sin cambiar el dato original. Se aplica con el operador `|`: `{{ fecha | date: 'short' }}`. Angular incluye pipes para fechas, monedas, números, porcentajes, texto, JSON, rebanadas de arreglos y valores asíncronos, y puedes crear los tuyos. En React lo harías llamando funciones de formato dentro del JSX; los pipes son esas funciones, pero declarativas y con caché automática.

### Cómo funciona
- Sintaxis: `valor | nombrePipe: arg1 : arg2`. Se pueden encadenar: `nombre | lowercase | titlecase`.
- Los pipes integrados viven en `@angular/common` y se importan individualmente en el componente: `DatePipe`, `CurrencyPipe`, `DecimalPipe` (`number`), `PercentPipe`, `UpperCasePipe`, `TitleCasePipe`, `SlicePipe`, `JsonPipe`, `KeyValuePipe`, `AsyncPipe`.
- Los de formato usan el **locale** de la app (por defecto `en-US`; para México ver [[Internacionalización en Angular]]).

Ejemplos con el locale predeterminado:

| Expresión | Resultado |
|---|---|
| `{{ 'hola mundo' \| titlecase }}` | `Hola Mundo` |
| `{{ 3.14159 \| number:'1.2-2' }}` | `3.14` (mín. 1 entero, entre 2 y 2 decimales) |
| `{{ 0.256 \| percent }}` | `26%` (sin decimales por defecto, redondea) |
| `{{ [1,2,3,4,5] \| slice:1:3 }}` | `2,3` (índices 1 y 2; el final no se incluye) |
| `{{ 1500 \| currency:'MXN' }}` | `MX$1,500.00` en en-US |

El formato `digitsInfo` es `minEnteros.minDecimales-maxDecimales`.

#### Puros contra impuros
- **Puro** (por defecto): Angular solo vuelve a ejecutar el pipe cuando cambia la **referencia** o el valor primitivo de la entrada. Es eficiente. Consecuencia: si haces `push` a un arreglo, un pipe puro que lo filtra no se entera.
- **Impuro** (`pure: false`): se ejecuta en cada ciclo de detección de cambios. Úsalo poco; puede volver lenta la vista.

#### El pipe `async`
`{{ datos$ | async }}` se suscribe a un Observable (o Promise), muestra el último valor, marca el componente para revisión cuando llega uno nuevo y **se desuscribe solo** al destruirse el componente. Con signals es menos necesario, porque puedes convertir el Observable con `toSignal()`.

### Ejemplo
Pipe propio `iniciales` que convierte "Ana López" en "AL":

```ts
// shared/iniciales.pipe.ts
import { Pipe, PipeTransform } from '@angular/core';

@Pipe({ name: 'iniciales' }) // standalone y puro por defecto
export class InicialesPipe implements PipeTransform {
  transform(nombre: string | null | undefined, max = 2): string {
    if (!nombre) return '';
    return nombre
      .trim()
      .split(/\s+/)
      .slice(0, max)
      .map(p => p[0].toUpperCase())
      .join('');
  }
}
```

```ts
@Component({
  selector: 'app-avatar',
  imports: [InicialesPipe, DatePipe],
  template: `
    <span class="avatar">{{ usuario().nombre | iniciales }}</span>
    <small>Alta: {{ usuario().alta | date: 'dd/MM/yyyy' }}</small>
  `,
})
export class Avatar {
  usuario = input.required<{ nombre: string; alta: string }>();
}
```

### Cuándo usarlo y cuándo no
- **Pipe**: formato de presentación reutilizable (fechas, moneda, iniciales, "hace 5 minutos").
- **`computed()`**: cuando la transformación depende de varias señales o es lógica del componente (filtrar y ordenar una lista).
- **Servicio o función**: cuando necesitas el resultado en TypeScript y no solo en la plantilla.
- Evita pipes impuros salvo que sepas su costo.

### Errores comunes
- **Olvidar importar el pipe.** Error "No pipe found with name". Agrégalo a `imports`.
- **Esperar que un pipe puro reaccione a una mutación.** Crea un arreglo nuevo (`[...lista, x]`) para que cambie la referencia.
- **Formatos de México sin registrar el locale.** `currency` y `date` salen en inglés; configura `LOCALE_ID`.
- **Encadenar varios `async` del mismo Observable.** Cada uno crea una suscripción (y una petición HTTP); usa `@if (datos$ | async; as d)` una sola vez o `toSignal`.

### Para profundizar
- [Angular: pipes](https://angular.dev/guide/templates/pipes)
- [API de DecimalPipe y digitsInfo](https://angular.dev/api/common/DecimalPipe)
- [API de AsyncPipe](https://angular.dev/api/common/AsyncPipe)

## Ejercicios

Resuélvelos en papel o en la herramienta; la solución está plegada.

> [!question]- ¿Qué muestra `{{ 'hola mundo' | titlecase }}`?
> `Hola Mundo`

> [!question]- ¿Qué muestra `{{ 3.14159 | number:'1.2-2' }}`?
> `3.14` (mínimo 1 entero, entre 2 y 2 decimales).

> [!question]- ¿Qué muestra `{{ 0.256 | percent }}` con el locale predeterminado (en-US)?
> `26%` (por defecto sin decimales, redondea).

> [!question]- ¿Qué muestra `{{ [1, 2, 3, 4, 5] | slice:1:3 }}`?
> `2,3` (elementos de índice 1 y 2; la interpolación une el arreglo con comas).

## Autoevaluación

Contesta primero en voz alta o en papel; luego despliega la respuesta.

> [!question]- ¿Qué es un pipe?
> Una función de transformación para la plantilla: `{{ fecha | date:'short' }}`. Ejemplos integrados: `date`, `currency`, `number`, `percent`, `uppercase`, `titlecase`, `json`, `slice`, `keyvalue`, `async`.

> [!question]- ¿Qué diferencia hay entre un pipe puro e impuro?
> El **puro** (predeterminado) solo se recalcula si cambia la referencia de la entrada, por eso es eficiente. El **impuro** (`pure: false`) se ejecuta en cada detección de cambios; úsalo poco.

> [!question]- ¿Qué hace el pipe `async`?
> Se suscribe a un Observable o Promise, muestra el último valor y se desuscribe al destruir el componente.

> [!question]- Escribe un pipe `iniciales` que convierta "Ana López" en "AL".
> ```ts
> @Pipe({ name: 'iniciales' })
> export class InicialesPipe implements PipeTransform {
>   transform(nombre: string): string {
>     return nombre.split(' ').filter(Boolean).map(p => p[0].toUpperCase()).join('');
>   }
> }
> ```

## Práctica

- [[Lab 1 - Componentes standalone y data binding en Angular]]

