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
# TypeScript para Angular

Parte de: [[20-Angular/01 Básico/_Índice Básico|Básico]]

> [!info] Cómo avanzar
> `progreso`: **pendiente** → **viendo** (estudiando) → **visto** (contestaste la autoevaluación sin abrir las respuestas) → **dominado** (hiciste el laboratorio o lo aplicaste de verdad).

## Contenido

### Qué es
TypeScript es JavaScript con **tipos estáticos**: el compilador revisa que uses bien variables, funciones y objetos antes de ejecutar nada. Angular está escrito en TypeScript y lo exige; además, sus plantillas también se revisan contra los tipos de tu componente (`strictTemplates` está activado por defecto desde Angular 22). Angular 22 requiere **TypeScript 6**.

Si ya usas React con TypeScript, la base es la misma. Lo que cambia es que en Angular usarás más **clases**, **decoradores** y **genéricos**.

### Cómo funciona
El compilador (`tsc`, integrado en el build de Angular) lee `tsconfig.json`, infiere tipos y marca errores. Los tipos desaparecen en el JavaScript final: son una ayuda en tiempo de desarrollo. Las características que más vas a usar:

1. **Interfaces y tipos** para modelar datos que vienen del API.
2. **Clases** para componentes, servicios, directivas y pipes.
3. **Decoradores** (`@Component`, `@Pipe`, `@Directive`, `@Service`) que agregan metadatos a una clase.
4. **Genéricos**: `signal<Producto[]>([])`, `http.get<Producto>(url)`, `input<string>()`.
5. **Uniones y literales**: `type Estado = 'activo' | 'inactivo'`.
6. **Encadenamiento opcional y coalescencia**: `usuario?.nombre ?? 'Anónimo'`.

#### `interface` contra `class` para modelos
Una `interface` solo describe la forma de un objeto y no genera código; es lo ideal para los JSON que devuelve tu API .NET. Una `class` existe en tiempo de ejecución, puede tener constructor y métodos, y se instancia con `new`. Para DTOs usa interfaces; usa clases cuando de verdad necesites comportamiento.

#### Literales y `as const`
Con `let estado = 'activo'` TypeScript infiere `string`. Con `const estado = 'activo' as const` infiere el tipo literal `'activo'`. En arreglos y objetos, `as const` además los vuelve de solo lectura, lo que sirve para catálogos fijos.

#### Aserción de asignación definitiva
En modo estricto, una propiedad de clase debe inicializarse. `nombre!: string` le dice al compilador "confía en mí, alguien la asignará después". Úsala con cuidado: si nadie la asigna, tendrás `undefined` en tiempo de ejecución. En Angular moderno casi no la necesitas porque `input.required()` y los inicializadores con `inject()` resuelven esos casos.

### Ejemplo
Modelo y utilidades para el catálogo de productos de Itzamna.Dionisio:

```ts
// productos/producto.ts
export interface Producto {
  id: number;
  nombre: string;
  precio: number;
  categoria?: string; // opcional
}

export type EstadoCarga = 'inactivo' | 'cargando' | 'listo' | 'error';

// Genérico: funciona con cualquier tipo de arreglo
export function primero<T>(lista: readonly T[]): T | undefined {
  return lista[0];
}

export const MONEDAS = ['MXN', 'USD'] as const;
export type Moneda = (typeof MONEDAS)[number]; // 'MXN' | 'USD'
```

Uso dentro de un servicio:

```ts
import { inject } from '@angular/core';
import { HttpClient } from '@angular/common/http';
import { Service } from '@angular/core';
import { Producto } from './producto';

@Service()
export class ProductosApi {
  private http = inject(HttpClient);

  obtener(id: number) {
    // El genérico tipa la respuesta: Observable<Producto>
    return this.http.get<Producto>(`/api/productos/${id}`);
  }
}
```

### Cuándo usarlo y cuándo no
- Tipa siempre lo que cruza fronteras: respuestas HTTP, inputs, outputs y valores de formularios.
- Deja que TypeScript **infiera** lo obvio (`const total = 0` ya es `number`).
- Evita `any`: apaga la revisión. Si no conoces la forma, usa `unknown` y valida.
- Prefiere uniones de literales a `enum` para estados simples; son más ligeras y se serializan igual que el JSON.

### Errores comunes
- **Confiar en que el tipo valida el JSON.** `http.get<Producto>()` no comprueba nada en tiempo de ejecución; si el API cambia, el tipo miente. Mantén los DTOs sincronizados con el backend.
- **Abusar de `!`.** Oculta errores de inicialización que luego aparecen como `Cannot read properties of undefined`.
- **Desactivar `strict`.** Pierdes la mitad del valor de TypeScript y de la revisión de plantillas.
- **Usar `class` para cada DTO.** Agrega código innecesario y confunde: el objeto que llega del servidor no es una instancia de esa clase.

### Para profundizar
- [TypeScript Handbook: tipos cotidianos](https://www.typescriptlang.org/docs/handbook/2/everyday-types.html)
- [TypeScript Handbook: genéricos](https://www.typescriptlang.org/docs/handbook/2/generics.html)
- [Angular: guía de estilo](https://angular.dev/style-guide)

## Ejercicios

Resuélvelos en papel o en la herramienta; la solución está plegada.

> [!question]- Escribe una interfaz `Producto` con `id` numérico, `nombre`, `precio` y `categoria` opcional.
> ```ts
> export interface Producto {
>   id: number;
>   nombre: string;
>   precio: number;
>   categoria?: string;
> }
> ```

> [!question]- Escribe una función genérica `primero<T>` que devuelva el primer elemento de un arreglo o `undefined`.
> ```ts
> function primero<T>(items: T[]): T | undefined {
>   return items[0];
> }
> ```

> [!question]- ¿Qué tipo infiere TypeScript para `const estado = 'activo' as const`?
> El tipo literal `'activo'` (no `string`).

## Autoevaluación

Contesta primero en voz alta o en papel; luego despliega la respuesta.

> [!question]- ¿Qué características de TypeScript usas más en Angular?
> Tipos e interfaces para modelos, genéricos (`http.get<Producto[]>()`), decoradores (`@Component`), modificadores (`private`, `readonly`), tipos unión (`'admin' | 'user'`) y modo estricto.

> [!question]- ¿Qué diferencia hay entre `interface` y `class` para un modelo de datos?
> Una `interface` solo existe en compilación (no genera código) y basta para tipar respuestas JSON. Una `class` existe en ejecución y sirve si necesitas métodos o `instanceof`.

> [!question]- ¿Qué hace el operador `!` en `nombre!: string`?
> Le dice al compilador que la propiedad **sí** tendrá valor aunque no se asigne en el constructor (*definite assignment*). Úsalo con cuidado: si te equivocas, tendrás `undefined` en tiempo de ejecución.

