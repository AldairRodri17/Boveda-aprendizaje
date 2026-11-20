---
proyecto: Conocimiento
tipo: laboratorio
nivel: intermedio
progreso: pendiente
estado: borrador
area: JavaScript y Web
herramienta: TypeScript Playground (typescriptlang.org/play) o Node con `npx tsc`
tags: [javascript-web, laboratorio]
---
# Lab 7 - TypeScript básico con un módulo de carrito

Parte de: [[04-JavaScript-Web/06 Avanzado/_Índice Avanzado|Avanzado]]

**Cubre:** [[TypeScript]] · [[TypeScript - tipos básicos]] · [[TypeScript - interfaces y types]] · [[TypeScript - genéricos]] · [[TypeScript - uniones y narrowing]]

> [!info] Al terminar
> Si todos los criterios quedan marcados, cambia a `dominado` el `progreso` de este laboratorio y de las notas que cubre. Guarda tus archivos de práctica en esta misma carpeta.

## Objetivo

Convertir un módulo de carrito de compras de JS a TypeScript estricto, modelando productos, líneas, descuentos y resultados de operaciones, sin usar `any`.

## Pasos

1. Crea `carrito.ts` en esta carpeta (o usa el Playground con `strict` activado).
2. Define `interface Producto { id: string; nombre: string; precio: number }` y `type Linea = { producto: Producto; cantidad: number }`.
3. Define un tipo unión `Descuento` con variantes `{ tipo: 'porcentaje'; valor: number }`, `{ tipo: 'fijo'; monto: number }` y `{ tipo: 'ninguno' }`.
4. Escribe `aplicarDescuento(subtotal: number, d: Descuento): number` con `switch` exhaustivo (`never`).
5. Define `type Resultado<T> = { ok: true; valor: T } | { ok: false; error: string }` y haz que `agregar(carrito, producto, cantidad)` devuelva `Resultado<Linea[]>` (error si la cantidad no es entera positiva).
6. Escribe un genérico `agruparPor<T, K extends PropertyKey>(xs: T[], clave: (x: T) => K): Record<K, T[]>`.
7. Escribe `parsearProducto(x: unknown): Resultado<Producto>` que valide un objeto que llega de `JSON.parse`.
8. Corre `npx tsc --noEmit --strict carrito.ts` (o mira el panel de errores del Playground). Agrega una cuarta variante de `Descuento` y observa el error en el `switch`.

## Criterios de terminado

- [ ] `tsc --strict` termina sin errores y no hay ningún `any` en el archivo.
- [ ] Al agregar una variante nueva a `Descuento`, TS marca error en el `default` del `switch`.
- [ ] `agregar` con cantidad `1.5` devuelve `{ ok: false, ... }` y el llamador no puede leer `valor` sin revisar `ok`.
- [ ] `parsearProducto(JSON.parse('{"id":1}'))` devuelve error en vez de un producto inválido.

## Solución

> [!success]- Descuento y Resultado
> ```ts
> type Descuento =
>   | { tipo: 'porcentaje'; valor: number }
>   | { tipo: 'fijo'; monto: number }
>   | { tipo: 'ninguno' };
>
> function aplicarDescuento(subtotal: number, d: Descuento): number {
>   switch (d.tipo) {
>     case 'porcentaje': return subtotal * (1 - d.valor / 100);
>     case 'fijo': return Math.max(0, subtotal - d.monto);
>     case 'ninguno': return subtotal;
>     default: { const _x: never = d; return _x; }
>   }
> }
>
> type Resultado<T> = { ok: true; valor: T } | { ok: false; error: string };
> ```

> [!success]- Genérico y validación de unknown
> ```ts
> function agruparPor<T, K extends PropertyKey>(xs: T[], clave: (x: T) => K): Record<K, T[]> {
>   const r = {} as Record<K, T[]>;
>   for (const x of xs) (r[clave(x)] ??= []).push(x);
>   return r;
> }
>
> function parsearProducto(x: unknown): Resultado<Producto> {
>   if (typeof x !== 'object' || x === null) return { ok: false, error: 'no es objeto' };
>   const o = x as Record<string, unknown>;
>   if (typeof o.id !== 'string' || typeof o.nombre !== 'string' || typeof o.precio !== 'number')
>     return { ok: false, error: 'campos inválidos' };
>   return { ok: true, valor: { id: o.id, nombre: o.nombre, precio: o.precio } };
> }
> ```

## Notas personales

_Qué me costó, errores que cometí, dudas._
