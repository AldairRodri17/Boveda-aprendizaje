---
proyecto: Conocimiento
tipo: concepto
nivel: intermedio
progreso: pendiente
estado: pendiente
area: Angular
tags: [angular]
---
# RxJS - operadores más usados

Parte de: [[20-Angular/02 Intermedio/_Índice Intermedio|Intermedio]]

> [!info] Cómo avanzar
> `progreso`: **pendiente** → **viendo** (estudiando) → **visto** (contestaste la autoevaluación sin abrir las respuestas) → **dominado** (hiciste el laboratorio o lo aplicaste de verdad).

## Contenido

### Qué es
Los **operadores** son funciones que transforman, filtran o combinan Observables. Se encadenan dentro de `.pipe()` y cada uno devuelve un Observable nuevo, sin modificar el original. Son como los métodos de arreglos (`map`, `filter`) pero aplicados a valores que llegan en el tiempo. Con una docena de operadores cubres casi todo lo que hace una app Angular.

### Cómo funciona
```ts
import { of, filter, map } from 'rxjs';

of(1, 2, 3, 4).pipe(
  filter(n => n % 2 === 0), // deja pasar 2 y 4
  map(n => n * 10),         // los convierte en 20 y 40
).subscribe(console.log);   // 20, 40
```

#### Transformación y filtrado
- **`map`**: transforma cada valor.
- **`filter`**: deja pasar solo los que cumplen la condición.
- **`tap`**: ejecuta un efecto secundario (log, guardar en caché, apagar un *spinner*) **sin cambiar el valor**.
- **`distinctUntilChanged`**: ignora un valor si es igual al **inmediato anterior**. `of(5, 5, 6, 6, 5)` emite `5, 6, 5`: el último 5 pasa porque el anterior era 6.
- **`debounceTime(ms)`**: espera a que pasen `ms` milisegundos sin nuevos valores y emite el último. Ideal para no buscar en cada tecla.
- **`take(n)`**, **`first()`**: toman los primeros valores y completan.

#### Operadores de "aplanamiento" (Observable dentro de Observable)
Cuando por cada valor necesitas lanzar otra operación asíncrona (por ejemplo, una petición HTTP), usas un operador `*Map`. La diferencia es qué hacen si llega un valor nuevo mientras la operación anterior sigue en curso:

| Operador | Comportamiento | Caso típico |
|---|---|---|
| `switchMap` | Cancela la anterior y se queda con la nueva | Búsqueda en vivo, cargar detalle al cambiar de ruta |
| `mergeMap` | Ejecuta todas en paralelo | Subir varios archivos a la vez |
| `concatMap` | Encola: una tras otra, en orden | Guardados que deben respetar el orden |
| `exhaustMap` | Ignora las nuevas mientras haya una en curso | Botón "Pagar" o "Iniciar sesión" (evita doble clic) |

#### Combinación
- **`forkJoin([a$, b$])`**: espera a que **todos completen** y emite **una vez** un arreglo con el **último valor** de cada uno. `forkJoin([of(1), of('a', 'b')])` emite `[1, 'b']` y completa. Es como `Promise.all`; perfecto para cargar catálogos en paralelo.
- **`combineLatest([a$, b$])`**: emite cada vez que **cualquiera** emite (después de que todos emitieron al menos una vez), con los últimos valores de todos. Útil para filtros que dependen de varias fuentes vivas. Si alguna fuente nunca completa, `forkJoin` nunca emite.

#### Errores
- **`catchError(err => ...)`**: intercepta un error y devuelve un Observable de reemplazo (un valor por defecto, `EMPTY`) o relanza con `throwError`. Debe ir **después** de lo que puede fallar.
- **`retry(n)`**: reintenta antes de rendirse.

### Ejemplo
Búsqueda de clientes con un `FormControl` llamado `busqueda`:

```ts
import { Component, inject } from '@angular/core';
import { ReactiveFormsModule, FormControl } from '@angular/forms';
import { toSignal } from '@angular/core/rxjs-interop';
import { debounceTime, distinctUntilChanged, filter, map, switchMap, catchError, of, tap } from 'rxjs';
import { ClientesApi, Cliente } from './clientes-api';

@Component({
  selector: 'app-buscar-clientes',
  imports: [ReactiveFormsModule],
  template: `
    <input [formControl]="busqueda" placeholder="Buscar cliente" />
    @for (c of resultados(); track c.id) { <li>{{ c.nombre }}</li> }
  `,
})
export class BuscarClientes {
  private api = inject(ClientesApi);
  protected busqueda = new FormControl('', { nonNullable: true });

  protected resultados = toSignal(
    this.busqueda.valueChanges.pipe(
      debounceTime(300),               // espera a que deje de teclear
      map(q => q.trim()),
      distinctUntilChanged(),          // no repite la misma búsqueda
      filter(q => q.length >= 2),
      tap(q => console.debug('buscando', q)),
      switchMap(q =>                   // cancela la petición anterior
        this.api.buscar(q).pipe(catchError(() => of([] as Cliente[]))),
      ),
    ),
    { initialValue: [] as Cliente[] },
  );
}
```

El `catchError` va **dentro** del `switchMap`: si fuera afuera, el primer error completaría el flujo principal y la búsqueda dejaría de funcionar.

### Cuándo usarlo y cuándo no
- Usa operadores cuando el **tiempo** importa: debounce, cancelación, reintentos, concurrencia.
- Si solo transformas un valor actual, un `computed()` es más simple.
- Para cargar varias cosas al entrar a una pantalla, `forkJoin`; para combinar filtros vivos, `combineLatest` o señales con `computed`.

### Errores comunes
- **`mergeMap` en una búsqueda**: respuestas viejas pueden llegar después de las nuevas y mostrar resultados incorrectos. Usa `switchMap`.
- **`switchMap` en un guardado**: si el usuario hace dos clics, el primer guardado se cancela en el cliente pero quizá ya llegó al servidor. Usa `exhaustMap` o `concatMap`.
- **`catchError` en el lugar equivocado**: mata el flujo principal.
- **Olvidar `distinctUntilChanged` tras `debounceTime`**: repites peticiones idénticas.

### Para profundizar
- [RxJS: operadores](https://rxjs.dev/guide/operators)
- [RxJS: referencia de la API](https://rxjs.dev/api)
- [Angular: RxJS interop](https://angular.dev/guide/signals/rxjs-interop)

## Ejercicios

Resuélvelos en papel o en la herramienta; la solución está plegada.

> [!question]- ¿Qué emite `of(1, 2, 3, 4).pipe(filter(n => n % 2 === 0), map(n => n * 10))`?
> `20`, `40` y completa.

> [!question]- ¿Qué emite `of(5, 5, 6, 6, 5).pipe(distinctUntilChanged())`?
> `5`, `6`, `5`.

> [!question]- Escribe el flujo de una búsqueda con un `FormControl` llamado `busqueda` y `api.buscar(q)`.
> ```ts
> resultados$ = this.busqueda.valueChanges.pipe(
>   debounceTime(300),
>   map(q => q.trim()),
>   distinctUntilChanged(),
>   switchMap(q => this.api.buscar(q).pipe(catchError(() => of([]))))
> );
> ```

> [!question]- ¿Qué emite `forkJoin([of(1), of('a', 'b')])`?
> Un solo valor: `[1, 'b']` (el último de cada uno).

## Autoevaluación

Contesta primero en voz alta o en papel; luego despliega la respuesta.

> [!question]- ¿Qué diferencia hay entre `switchMap`, `mergeMap`, `concatMap` y `exhaustMap`?
> Todos mapean a otro Observable. **switchMap** cancela el anterior (búsquedas). **mergeMap** los corre en paralelo. **concatMap** los encola en orden. **exhaustMap** ignora los nuevos mientras uno está activo (botón de guardar).

> [!question]- ¿Qué hacen `debounceTime` y `distinctUntilChanged`?
> `debounceTime(300)` espera 300 ms sin nuevos valores antes de emitir. `distinctUntilChanged()` no emite si el valor es igual al anterior. Juntos evitan peticiones de más al escribir.

> [!question]- ¿Para qué sirven `catchError` y `tap`?
> `catchError` atrapa un error y devuelve un Observable alternativo (o relanza). `tap` ejecuta efectos secundarios (logs) sin cambiar los valores.

> [!question]- ¿Qué diferencia hay entre `forkJoin` y `combineLatest`?
> `forkJoin` espera a que **todos completen** y emite una vez los últimos valores (varias peticiones HTTP en paralelo). `combineLatest` emite cada vez que cualquiera cambia, una vez que todos emitieron al menos una vez.

## Práctica

- [[Lab 2 - Servicio, HttpClient e interceptor en Angular]]

