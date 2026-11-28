---
proyecto: Conocimiento
tipo: laboratorio
nivel: basico
progreso: pendiente
estado: borrador
area: Angular
herramienta: Angular CLI, Node.js y VS Code
tags: [angular, laboratorio]
contenido: completo
---
# Lab 1 - Componentes standalone y data binding en Angular

Parte de: [[20-Angular/01 Básico/_Índice Básico|Básico]]

**Cubre:** [[Angular CLI y estructura de un proyecto]] · [[Componentes en Angular]] · [[Componentes standalone]] · [[Templates y data binding (interpolación, propiedades y eventos)]] · [[Two-way binding]] · [[Control flow (@if, @for y @switch)]] · [[Input y Output]] · [[Pipes]]

> [!info] Al terminar
> Si todos los criterios quedan marcados, cambia a `dominado` el `progreso` de este laboratorio y de las notas que cubre. Guarda tus archivos de práctica en esta misma carpeta.

## Objetivo

Crear una app de tareas con dos componentes standalone que se comuniquen con inputs y outputs, usando todos los tipos de binding y el control flow.

## Requisitos

- Node.js LTS y el CLI: `npm install -g @angular/cli` (comprueba con `ng version`).
- VS Code con la extensión *Angular Language Service*.

## Pasos

1. Crea el proyecto: `ng new tareas-app` (estilos CSS, sin SSR) y arráncalo con `ng serve -o`.
2. Revisa `src/main.ts`, `src/app/app.config.ts` y el componente raíz. Anota qué hace cada uno.
3. Crea una interfaz `Tarea` (`id`, `titulo`, `hecha`) en `src/app/tarea.ts`.
4. Genera un componente hijo: `ng g c tarea-item`. Debe recibir una `tarea` (input requerido) y emitir `alternar` y `eliminar` con el `id`.
5. En su plantilla: casilla enlazada a `hecha`, título con el pipe `titlecase`, clase `hecha` cuando esté terminada (tachado) y botón **Eliminar**.
6. En el componente raíz: un arreglo inicial con 2 tareas, un campo de texto con `[(ngModel)]`, botón **Agregar** deshabilitado si el texto está vacío y que también funcione con Enter.
7. Muestra la lista con `@for` y `track`, con un mensaje en `@empty`, y un contador de pendientes.
8. Muestra la fecha de hoy con el pipe `date`.
9. **Reto:** agrega un filtro *Todas / Pendientes / Hechas* con `@switch` o con un método que filtre.

## Criterios de terminado

- [ ] `ng serve` compila sin errores ni advertencias y la app abre en `http://localhost:4200`.
- [ ] Agregar una tarea con el botón o con Enter la muestra en la lista y limpia el campo.
- [ ] El botón Agregar está deshabilitado con el campo vacío o con solo espacios.
- [ ] Marcar la casilla tacha la tarea y el contador de pendientes cambia.
- [ ] Eliminar todas las tareas muestra el mensaje del bloque `@empty`.
- [ ] El hijo no modifica la lista directamente: solo emite eventos y el padre actualiza el arreglo.
- [ ] Reto: el filtro cambia lo que se muestra sin perder tareas.

## Solución

> [!success]- Nota sobre nombres de archivo
> Según la versión del CLI, el componente raíz puede llamarse `App` (archivo `app.ts`) o `AppComponent` (`app.component.ts`), y el hijo `TareaItem` o `TareaItemComponent`. Ajusta los `import` a lo que generó tu CLI.

> [!success]- tarea.ts y componente hijo
> ```ts
> // tarea.ts
> export interface Tarea { id: number; titulo: string; hecha: boolean; }
>
> // tarea-item.ts
> import { Component, input, output } from '@angular/core';
> import { TitleCasePipe } from '@angular/common';
> import { Tarea } from '../tarea';
>
> @Component({
>   selector: 'app-tarea-item',
>   imports: [TitleCasePipe],
>   template: `
>     <li [class.hecha]="tarea().hecha">
>       <input type="checkbox" [checked]="tarea().hecha" (change)="alternar.emit(tarea().id)" />
>       {{ tarea().titulo | titlecase }}
>       <button (click)="eliminar.emit(tarea().id)">Eliminar</button>
>     </li>
>   `,
>   styles: `.hecha { text-decoration: line-through; color: gray; }`,
> })
> export class TareaItem {
>   tarea = input.required<Tarea>();
>   alternar = output<number>();
>   eliminar = output<number>();
> }
> ```

> [!success]- Componente raíz
> ```ts
> import { Component } from '@angular/core';
> import { DatePipe } from '@angular/common';
> import { FormsModule } from '@angular/forms';
> import { TareaItem } from './tarea-item/tarea-item';
> import { Tarea } from './tarea';
>
> @Component({
>   selector: 'app-root',
>   imports: [FormsModule, DatePipe, TareaItem],
>   template: `
>     <h1>Mis tareas</h1>
>     <p>Hoy es {{ hoy | date: 'fullDate' }}</p>
>     <input [(ngModel)]="nueva" placeholder="Nueva tarea" (keyup.enter)="agregar()" />
>     <button (click)="agregar()" [disabled]="!nueva.trim()">Agregar</button>
>     <p>Pendientes: {{ pendientes() }}</p>
>     <ul>
>       @for (t of tareas; track t.id) {
>         <app-tarea-item [tarea]="t" (alternar)="alternar($event)" (eliminar)="eliminar($event)" />
>       } @empty {
>         <li>No hay tareas. ¡Agrega una!</li>
>       }
>     </ul>
>   `,
> })
> export class App {
>   hoy = new Date();
>   nueva = '';
>   tareas: Tarea[] = [
>     { id: 1, titulo: 'instalar angular cli', hecha: true },
>     { id: 2, titulo: 'crear mi primer componente', hecha: false },
>   ];
>   private siguienteId = 3;
>
>   agregar() {
>     const titulo = this.nueva.trim();
>     if (!titulo) return;
>     this.tareas = [...this.tareas, { id: this.siguienteId++, titulo, hecha: false }];
>     this.nueva = '';
>   }
>   alternar(id: number) {
>     this.tareas = this.tareas.map(t => (t.id === id ? { ...t, hecha: !t.hecha } : t));
>   }
>   eliminar(id: number) {
>     this.tareas = this.tareas.filter(t => t.id !== id);
>   }
>   pendientes() {
>     return this.tareas.filter(t => !t.hecha).length;
>   }
> }
> ```
> Se crean arreglos nuevos en lugar de mutar: así el patrón sigue funcionando cuando pases a señales u OnPush.

> [!success]- Reto: filtro
> ```ts
> filtro: 'todas' | 'pendientes' | 'hechas' = 'todas';
> visibles() {
>   return this.filtro === 'todas' ? this.tareas
>     : this.tareas.filter(t => t.hecha === (this.filtro === 'hechas'));
> }
> ```
> ```html
> <select [(ngModel)]="filtro">
>   <option value="todas">Todas</option>
>   <option value="pendientes">Pendientes</option>
>   <option value="hechas">Hechas</option>
> </select>
> ```
> Y en el `@for` usa `visibles()` en lugar de `tareas`.

## Notas personales

_Qué me costó, errores que cometí, dudas._
