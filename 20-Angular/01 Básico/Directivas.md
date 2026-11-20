---
proyecto: Conocimiento
tipo: concepto
nivel: basico
progreso: pendiente
estado: pendiente
area: Angular
tags: [angular]
---
# Directivas

Parte de: [[20-Angular/01 Básico/_Índice Básico|Básico]]

> [!info] Cómo avanzar
> `progreso`: **pendiente** → **viendo** (estudiando) → **visto** (contestaste la autoevaluación sin abrir las respuestas) → **dominado** (hiciste el laboratorio o lo aplicaste de verdad).

## Contenido

### Qué es
Una **directiva** es una clase que agrega comportamiento a un elemento del DOM. Hay tres tipos:

1. **Componentes**: directivas con plantilla propia (todo lo visto en [[Componentes en Angular]]).
2. **Directivas de atributo**: cambian la apariencia o el comportamiento de un elemento existente (`ngClass`, `ngStyle`, `ngModel`, `routerLinkActive` o las tuyas, como `appResaltar`).
3. **Directivas estructurales**: agregan o quitan elementos del DOM usando una plantilla (`*ngIf`, `*ngFor`, o propias con `*`). Con el control flow `@if`/`@for` hoy rara vez escribes una.

En React no hay un equivalente directo: lo más cercano es un *hook* que recibe un `ref` o un componente envoltorio. En Angular la directiva se "pega" al elemento con un atributo.

### Cómo funciona
Una directiva se declara con `@Directive` y un `selector` de atributo (entre corchetes). Se aplica cuando un elemento de una plantilla coincide con el selector y el componente importó la directiva.

Para reaccionar a eventos del elemento anfitrión y modificar sus propiedades se usa la propiedad **`host`** del decorador:

```ts
host: {
  '(mouseenter)': 'activar()',      // escucha un evento
  '[style.backgroundColor]': 'color()', // enlaza una propiedad
  'class': 'resaltable',             // atributo estático
}
```

Las alternativas históricas son los decoradores `@HostListener` y `@HostBinding`. Siguen funcionando, pero la guía de estilo recomienda `host` porque reúne en un solo lugar todo lo que la directiva hace al anfitrión, no necesita decoradores sobre cada miembro y el compilador revisa los tipos de esas expresiones (`typeCheckHostBindings` está activo por defecto desde v21).

Para acceder al elemento nativo usa `inject(ElementRef)`; para manipularlo de forma segura con SSR, `inject(Renderer2)` o bindings en `host`.

### Ejemplo
Directiva que resalta un elemento al pasar el mouse, con color configurable:

```ts
// shared/resaltar.ts
import { Directive, input, signal, computed } from '@angular/core';

@Directive({
  selector: '[appResaltar]',
  host: {
    '(mouseenter)': 'encima.set(true)',
    '(mouseleave)': 'encima.set(false)',
    '[style.backgroundColor]': 'fondo()',
    '[style.transition]': '"background-color 150ms"',
  },
})
export class Resaltar {
  // El input se llama igual que el selector: <li appResaltar="lightblue">
  color = input('khaki', { alias: 'appResaltar' });
  protected encima = signal(false);
  protected fondo = computed(() => (this.encima() ? this.color() || 'khaki' : ''));
}
```

```ts
@Component({
  selector: 'app-lista',
  imports: [Resaltar],
  template: `
    @for (p of productos(); track p.id) {
      <li appResaltar="lightblue">{{ p.nombre }}</li>
    }
  `,
})
export class Lista { /* ... */ }
```

#### Directivas del framework que más usarás
- `RouterLink`, `RouterLinkActive` (navegación).
- `NgModel`, `FormControlName`, `FormField` (formularios).
- `NgOptimizedImage` (`ngSrc`) para imágenes con carga optimizada.
- `NgTemplateOutlet` para renderizar una plantilla `ng-template` recibida por input.

#### Composición con `hostDirectives`
Un componente puede aplicar directivas a su propio anfitrión con `hostDirectives: [Resaltar]`. Sirve para reutilizar comportamientos (foco, tooltip, accesibilidad) sin herencia.

### Cuándo usarlo y cuándo no
- **Directiva de atributo**: cuando el comportamiento es transversal y no necesita su propia plantilla (autofocus, permisos `*appSiRol`, arrastrar, tooltips).
- **Componente**: cuando hay que dibujar HTML nuevo.
- **Bindings directos** (`[class.x]`, `[style.y]`): para estilos condicionales simples, mejor que `ngClass`/`ngStyle`.
- **Directiva estructural propia**: solo en casos especiales; el control flow cubre casi todo.

### Errores comunes
- **Olvidar importar la directiva.** No da error: el atributo simplemente se queda como atributo HTML y no pasa nada. Si "no funciona", revisa `imports`.
- **Selector sin corchetes.** `selector: 'appResaltar'` buscaría un *elemento* `<appResaltar>`, no un atributo.
- **Manipular el DOM con `nativeElement` directamente.** Rompe con SSR y es más difícil de probar; prefiere `host`.
- **Lógica de negocio en directivas.** Mantenlas pequeñas y enfocadas en la vista; la lógica va en servicios.

### Para profundizar
- [Angular: directivas](https://angular.dev/guide/directives)
- [Angular: directivas de atributo](https://angular.dev/guide/directives/attribute-directives)
- [Angular: elementos anfitriones (host)](https://angular.dev/guide/components/host-elements)

## Autoevaluación

Contesta primero en voz alta o en papel; luego despliega la respuesta.

> [!question]- ¿Qué tipos de directivas hay?
> **Componentes** (directivas con plantilla), **de atributo** (cambian apariencia o comportamiento de un elemento, p. ej. `ngClass`) y **estructurales** (agregan o quitan elementos, p. ej. `*ngIf`, hoy sustituida por `@if`).

> [!question]- Escribe una directiva que resalte un elemento al pasar el mouse.
> ```ts
> @Directive({
>   selector: '[appResaltar]',
>   host: {
>     '(mouseenter)': 'activo.set(true)',
>     '(mouseleave)': 'activo.set(false)',
>     '[style.backgroundColor]': "activo() ? color() : ''",
>   },
> })
> export class Resaltar {
>   color = input('yellow', { alias: 'appResaltar' });
>   activo = signal(false);
> }
> ```
> Uso: `<p appResaltar="lightblue">…</p>` (importando `Resaltar`).

> [!question]- ¿Qué ventaja tiene la propiedad `host` sobre `@HostListener` / `@HostBinding`?
> Concentra en el decorador los eventos y bindings del elemento anfitrión; es la forma recomendada actualmente. Los decoradores siguen funcionando.

