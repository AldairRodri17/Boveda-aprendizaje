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
# Control flow (@if, @for y @switch)

Parte de: [[20-Angular/01 Básico/_Índice Básico|Básico]]

> [!info] Cómo avanzar
> `progreso`: **pendiente** → **viendo** (estudiando) → **visto** (contestaste la autoevaluación sin abrir las respuestas) → **dominado** (hiciste el laboratorio o lo aplicaste de verdad).

## Contenido

### Qué es
El **control flow integrado** son bloques de plantilla que deciden qué se muestra: `@if` / `@else if` / `@else`, `@for` con `@empty`, y `@switch` con `@case` y `@default`. Llegó en Angular 17 y reemplaza a las directivas estructurales `*ngIf`, `*ngFor` y `*ngSwitch`, que quedaron obsoletas en Angular 20. Además existen `@defer` (carga diferida, ver [[Deferrable views (@defer)]]) y `@let` (variables locales de plantilla).

En React harías esto con `&&`, ternarios y `.map()` dentro del JSX; aquí son bloques explícitos que el compilador entiende y optimiza, sin importar nada.

### Cómo funciona
#### `@if`
```html
@if (usuario(); as u) {
  <p>Bienvenido, {{ u.nombre }}</p>
} @else if (cargando()) {
  <p>Cargando...</p>
} @else {
  <a routerLink="/login">Inicia sesión</a>
}
```
La forma `; as u` guarda el resultado de la expresión en una variable local; muy útil con valores que pueden ser `null` (el tipo dentro del bloque ya no es nulo).

#### `@for`
```html
@for (p of productos(); track p.id; let i = $index, ultimo = $last) {
  <li [class.ultimo]="ultimo">{{ i + 1 }}. {{ p.nombre }}</li>
} @empty {
  <li>Sin productos</li>
}
```
- `track` es **obligatorio**: le dice a Angular cómo identificar cada elemento para reutilizar los nodos del DOM cuando la lista cambia, en lugar de destruirlos y recrearlos. Es la misma idea que la `key` de React, pero Angular la exige para evitar listas lentas. Usa un identificador estable (`p.id`); `$index` solo si la lista nunca se reordena.
- Variables implícitas: `$index`, `$count`, `$first`, `$last`, `$even`, `$odd`. Puedes renombrarlas con `let`.
- `@empty` se muestra cuando la colección está vacía.

#### `@switch`
```html
@switch (estado()) {
  @case ('ok') { <span>✔</span> }
  @case ('warn') { <span>⚠</span> }
  @default { <span>?</span> }
}
```
Compara con igualdad estricta (`===`). Angular 22 agregó la comprobación exhaustiva con `@default never(...)` para que el compilador avise si una unión de tipos queda sin `@case`.

### Ejemplo
Pantalla de pedidos que combina los tres bloques con dos señales de estado:

```ts
import { Component, signal } from '@angular/core';
import { CurrencyPipe } from '@angular/common';

interface Pedido { id: number; cliente: string; total: number; estado: 'ok' | 'warn' | 'cancelado'; }

@Component({
  selector: 'app-pedidos',
  imports: [CurrencyPipe],
  template: `
    @if (cargando()) {
      <p>Cargando…</p>
    } @else if (error(); as e) {
      <p class="error">{{ e }}</p>
    } @else {
      <ul>
        @for (p of pedidos(); track p.id) {
          <li>
            @switch (p.estado) {
              @case ('ok') { <span>🟢</span> }
              @case ('warn') { <span>🟡</span> }
              @default { <span>⚪</span> }
            }
            {{ p.cliente }} – {{ p.total | currency: 'MXN' }}
          </li>
        } @empty {
          <li>Sin pedidos</li>
        }
      </ul>
    }
  `,
})
export class Pedidos {
  protected cargando = signal(false);
  protected error = signal<string | null>(null);
  protected pedidos = signal<Pedido[]>([]);
}
```

#### Variables con `@let`
Para no repetir una expresión dentro de la plantilla, declara una variable local de solo lectura:

```html
@let total = subtotal() * 1.16;
<p>Subtotal: {{ subtotal() }} · Con IVA: {{ total }}</p>
```

`@let` se reevalúa cuando cambian sus dependencias y solo existe en el bloque donde se declara. Úsalo para legibilidad; si la lógica crece, muévela a un `computed()` en la clase.

#### Por qué es mejor que `*ngIf`
Los bloques son parte del lenguaje de plantillas: no hay que importar `NgIf` ni `CommonModule`, el compilador entiende mejor los tipos (por ejemplo, estrecha `null` dentro de `@if`) y el algoritmo de `@for` para reconciliar listas es más eficiente. Además el bloque `@else if` no tenía equivalente directo en la sintaxis vieja.

Si tienes código con `*ngIf`/`*ngFor`, la migración automática lo convierte: `ng g @angular/core:control-flow`.

### Cuándo usarlo y cuándo no
- **Siempre** en código nuevo: no requiere imports, es más legible y es más rápido que las directivas antiguas.
- Para mostrar u ocultar con CSS (sin destruir el elemento) usa `[hidden]` o `[class.oculto]`; `@if` sí quita el contenido del DOM y destruye los componentes internos.
- Para listas enormes (miles de filas) no basta `@for`: usa *virtual scroll* del CDK (ver [[Angular Material y CDK]]).

### Errores comunes
- **`track` con un valor inestable** (por ejemplo, un objeto nuevo en cada llamada o `Math.random()`): Angular recrea todo el DOM en cada cambio. Usa un id.
- **Olvidar llamar la señal**: `@if (cargando)` siempre es verdadero porque la función existe; debe ser `@if (cargando())`.
- **Poner `@empty` fuera del `@for`**: debe ir pegado al cierre del bloque `@for`.
- **Usar `@switch` esperando coincidencias "truthy"**: compara con `===`; `'1'` no es igual a `1`.

### Para profundizar
- [Angular: control flow en plantillas](https://angular.dev/guide/templates/control-flow)
- [Angular: variables locales con @let](https://angular.dev/guide/templates/variables)
- [Migración a control flow](https://angular.dev/reference/migrations/control-flow)

## Ejercicios

Resuélvelos en papel o en la herramienta; la solución está plegada.

> [!question]- Escribe una lista de `productos` que muestre "Sin productos" si está vacía.
> ```html
> <ul>
>   @for (p of productos; track p.id) {
>     <li>{{ p.nombre }}</li>
>   } @empty {
>     <li>Sin productos</li>
>   }
> </ul>
> ```

> [!question]- Muestra "Cargando…", el error o los datos según dos señales `cargando()` y `error()`.
> ```html
> @if (cargando()) {
>   <p>Cargando…</p>
> } @else if (error()) {
>   <p class="error">{{ error() }}</p>
> } @else {
>   <app-tabla [datos]="datos()" />
> }
> ```

> [!question]- Con `@switch`, muestra un ícono según `estado` (`'ok'`, `'warn'` o cualquier otro).
> ```html
> @switch (estado) {
>   @case ('ok') { <span>✔</span> }
>   @case ('warn') { <span>⚠</span> }
>   @default { <span>✖</span> }
> }
> ```

## Autoevaluación

Contesta primero en voz alta o en papel; luego despliega la respuesta.

> [!question]- ¿Qué bloques de control flow existen en las plantillas?
> `@if / @else if / @else`, `@for` (con `@empty`) y `@switch / @case / @default`. Reemplazan a `*ngIf`, `*ngFor` y `[ngSwitch]`.

> [!question]- ¿Por qué `@for` exige `track`?
> Para identificar cada elemento y que Angular reutilice el DOM al cambiar la lista en vez de recrearlo. Usa un identificador estable: `track item.id` (`track $index` solo si la lista no cambia de orden).

> [!question]- ¿Qué variables implícitas ofrece `@for`?
> `$index`, `$first`, `$last`, `$even`, `$odd` y `$count`.

> [!question]- ¿Cómo guardas en una variable el resultado de una expresión en `@if`?
> `@if (usuario(); as u) { Hola {{ u.nombre }} }`.

## Práctica

- [[Lab 1 - Componentes standalone y data binding en Angular]]

