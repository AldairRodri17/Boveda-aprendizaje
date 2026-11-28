---
proyecto: Conocimiento
tipo: laboratorio
nivel: intermedio
progreso: pendiente
estado: borrador
area: Angular
herramienta: Angular CLI y navegador (DevTools)
tags: [angular, laboratorio]
contenido: completo
---
# Lab 1 - Signals en Angular (carrito de compras)

Parte de: [[20-Angular/02 Intermedio/_Índice Intermedio|Intermedio]]

**Cubre:** [[Signals]] · [[computed y effect]] · [[Servicios e inyección de dependencias en Angular]] · [[Signals vs RxJS]]

> [!info] Al terminar
> Si todos los criterios quedan marcados, cambia a `dominado` el `progreso` de este laboratorio y de las notas que cubre. Guarda tus archivos de práctica en esta misma carpeta.

## Objetivo

Construir un carrito de compras cuyo estado viva en un servicio con señales, con totales calculados con `computed` y persistencia con `effect`.

## Pasos

1. Crea un proyecto nuevo (o usa el del Lab 1 de Básico) sin SSR.
2. Genera un servicio: `ng g s carrito`. Debe tener una señal privada `items` (producto + cantidad) y exponer:
   - `items` de solo lectura,
   - `cantidad` y `total` con `computed`,
   - métodos `agregar(producto)`, `quitar(id)` y `vaciar()`.
3. Al agregar un producto que ya está, debe subir su cantidad (no duplicarse). Nada de `push`: siempre arreglos nuevos.
4. Con un `effect`, guarda los items en `localStorage` cada vez que cambien, y léelos al crear el servicio.
5. Crea un componente `catalogo` con 4 productos fijos y botón **Agregar**, y un componente `carrito` que muestre las líneas, la cantidad total, el total con el pipe `currency` y un botón **Vaciar**.
6. Muestra en el encabezado de la app un contador con la cantidad del carrito (otro componente que inyecta el mismo servicio).
7. **Reto:** muestra un reloj en el encabezado con `toSignal(interval(1000))`.

## Criterios de terminado

- [ ] Agregar dos veces el mismo producto deja **una** línea con cantidad 2.
- [ ] El total y el contador del encabezado se actualizan solos al agregar, quitar o vaciar.
- [ ] En DevTools → Application → Local Storage aparece la clave del carrito y cambia con cada acción.
- [ ] Al recargar la página el carrito conserva su contenido.
- [ ] Desde un componente no es posible hacer `carrito.items.set(...)` (el compilador lo impide).
- [ ] Puedo explicar por qué `items().push(x)` no actualizaría la vista.
- [ ] Reto: el reloj avanza cada segundo.

## Solución

> [!tip] Los `import` de Angular y RxJS se omiten en algunos fragmentos para abreviar; VS Code los agrega con **Ctrl + .** sobre el nombre marcado en rojo.

> [!success]- carrito.service.ts
> ```ts
> import { Injectable, computed, effect, signal } from '@angular/core';
>
> export interface Producto { id: number; nombre: string; precio: number; }
> export interface Linea extends Producto { cantidad: number; }
>
> const CLAVE = 'carrito';
>
> @Injectable({ providedIn: 'root' })
> export class CarritoService {
>   private readonly _items = signal<Linea[]>(JSON.parse(localStorage.getItem(CLAVE) ?? '[]'));
>   readonly items = this._items.asReadonly();
>   readonly cantidad = computed(() => this._items().reduce((s, l) => s + l.cantidad, 0));
>   readonly total = computed(() => this._items().reduce((s, l) => s + l.precio * l.cantidad, 0));
>
>   constructor() {
>     effect(() => localStorage.setItem(CLAVE, JSON.stringify(this._items())));
>   }
>
>   agregar(p: Producto) {
>     this._items.update(items =>
>       items.some(l => l.id === p.id)
>         ? items.map(l => (l.id === p.id ? { ...l, cantidad: l.cantidad + 1 } : l))
>         : [...items, { ...p, cantidad: 1 }]);
>   }
>   quitar(id: number) {
>     this._items.update(items => items.filter(l => l.id !== id));
>   }
>   vaciar() {
>     this._items.set([]);
>   }
> }
> ```
> Si más adelante activas SSR, mueve la lectura de `localStorage` a código que solo corra en el navegador.

> [!success]- Componente carrito
> ```ts
> @Component({
>   selector: 'app-carrito',
>   imports: [CurrencyPipe],
>   template: `
>     @for (l of carrito.items(); track l.id) {
>       <div>{{ l.nombre }} × {{ l.cantidad }} = {{ l.precio * l.cantidad | currency: 'MXN' }}
>         <button (click)="carrito.quitar(l.id)">Quitar</button></div>
>     } @empty { <p>Carrito vacío</p> }
>     <p><strong>{{ carrito.cantidad() }} artículos · {{ carrito.total() | currency: 'MXN' }}</strong></p>
>     <button (click)="carrito.vaciar()">Vaciar</button>
>   `,
> })
> export class Carrito {
>   protected carrito = inject(CarritoService);
> }
> ```

> [!success]- ¿Por qué no `push`?
> Una señal avisa cuando recibe un valor **distinto** (`Object.is`). `push` modifica el mismo arreglo, la referencia no cambia y nadie se entera. `update(items => [...items, x])` entrega un arreglo nuevo.

> [!success]- Reto: reloj con toSignal
> ```ts
> import { toSignal } from '@angular/core/rxjs-interop';
> import { interval, map } from 'rxjs';
>
> ahora = toSignal(interval(1000).pipe(map(() => new Date())), { initialValue: new Date() });
> ```
> En la plantilla: `{{ ahora() | date: 'mediumTime' }}`. La suscripción se cancela sola al destruir el componente.

## Notas personales

_Qué me costó, errores que cometí, dudas._
