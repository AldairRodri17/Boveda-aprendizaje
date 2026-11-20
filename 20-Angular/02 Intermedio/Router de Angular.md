---
proyecto: Conocimiento
tipo: concepto
nivel: intermedio
progreso: pendiente
estado: pendiente
area: Angular
tags: [angular]
---
# Router de Angular

Parte de: [[20-Angular/02 Intermedio/_Índice Intermedio|Intermedio]]

> [!info] Cómo avanzar
> `progreso`: **pendiente** → **viendo** (estudiando) → **visto** (contestaste la autoevaluación sin abrir las respuestas) → **dominado** (hiciste el laboratorio o lo aplicaste de verdad).

## Contenido

### Qué es
El **router** de Angular (`@angular/router`) asocia URLs con componentes y permite navegar sin recargar la página. Incluye parámetros de ruta, rutas hijas, redirecciones, carga perezosa, guards, resolvers y animaciones de transición. Es el equivalente a React Router, pero oficial y configurado como un arreglo de objetos `Route`.

### Cómo funciona
Para tener navegación necesitas tres piezas:

1. **Rutas**: un arreglo `Routes` (en `app.routes.ts`).
2. **Registro**: `provideRouter(routes)` en `app.config.ts`.
3. **Punto de render**: `<router-outlet />` en la plantilla donde se pinta el componente activo, y enlaces con `routerLink`.

```ts
// app.routes.ts
import { Routes } from '@angular/router';

export const routes: Routes = [
  { path: '', redirectTo: 'productos', pathMatch: 'full' },
  { path: 'productos', component: ListaProductos, title: 'Productos' },
  { path: 'productos/:id', component: DetalleProducto },
  { path: '**', component: NoEncontrado }, // siempre al final
];
```

#### Orden y comodín
El router recorre las rutas **en orden** y usa la **primera que coincide**. La ruta `**` coincide con cualquier URL; si la pones antes, atraparía todo y nunca llegarías a las demás. Por eso va al final. `pathMatch: 'full'` en la redirección vacía evita que `''` coincida como prefijo de todas las rutas.

#### Leer parámetros
Dos formas:
- **Como input** (recomendada): activa `withComponentInputBinding()` y declara un input con el mismo nombre del parámetro. Funciona también con *query params* y datos de resolvers.
- **Con `ActivatedRoute`**: `inject(ActivatedRoute).paramMap` (Observable) o `snapshot.paramMap.get('id')` (valor puntual).

```ts
export class DetalleProducto {
  id = input.required<string>(); // de /productos/:id; los parámetros llegan como texto
  protected producto = httpResource<Producto>(() => `/api/productos/${this.id()}`);
}
```

Ojo: cuando navegas de `/productos/1` a `/productos/2`, Angular **reutiliza** el mismo componente; por eso conviene que el parámetro sea reactivo (input-señal o `paramMap`) y no leer solo el `snapshot` una vez.

#### Navegar
- En plantilla: `<a routerLink="/productos">` o con parámetros `[routerLink]="['/productos', p.id]"` y `[queryParams]="{ pagina: 2 }"`.
- En código: `inject(Router).navigate(['/productos', id])` o `navigateByUrl('/productos/5')`.

#### `routerLinkActive`
Agrega una clase CSS al enlace cuando su ruta está activa, para resaltar el menú actual. Con `[routerLinkActiveOptions]="{ exact: true }"` exiges coincidencia exacta (útil para el enlace de inicio, que si no estaría activo siempre).

### Ejemplo
Menú con resaltado y rutas hijas para la administración de Itzamna.Dionisio:

```ts
@Component({
  selector: 'app-root',
  imports: [RouterOutlet, RouterLink, RouterLinkActive],
  template: `
    <nav>
      <a routerLink="/" routerLinkActive="activo" [routerLinkActiveOptions]="{ exact: true }">Inicio</a>
      <a routerLink="/productos" routerLinkActive="activo">Productos</a>
      <a routerLink="/pedidos" routerLinkActive="activo">Pedidos</a>
    </nav>
    <main><router-outlet /></main>
  `,
  styles: `.activo { font-weight: bold; border-bottom: 2px solid; }`,
})
export class App {}
```

```ts
export const routes: Routes = [
  { path: '', component: Inicio },
  {
    path: 'pedidos',
    component: PedidosLayout,          // tiene su propio <router-outlet />
    children: [
      { path: '', component: ListaPedidos },
      { path: ':id', component: DetallePedido },
    ],
  },
  { path: '**', component: NoEncontrado },
];
```

Para proteger rutas ve [[Guards y resolvers]]; para dividir el bundle, [[Lazy loading de rutas]].

### Cuándo usarlo y cuándo no
- Cualquier app con más de una pantalla.
- Usa **rutas hijas** para layouts compartidos (menú lateral de una sección).
- Usa **query params** para estado que debe poder compartirse por URL (filtros, página actual).
- No guardes en la URL datos sensibles ni objetos grandes.

### Errores comunes
- **Comodín antes que otras rutas**: todo cae en "no encontrado".
- **Olvidar importar `RouterLink`**: el enlace se comporta como `<a>` normal y recarga la página (o no hace nada).
- **Leer solo el `snapshot`** y que el detalle no cambie al navegar entre ids.
- **Rutas con `/` inicial** en `path` (`'/productos'`): el router no lo acepta; los `path` van sin barra.
- **Servidor sin *fallback***: al recargar `/productos/5` en producción da 404; configura el hosting (Azure Static Web Apps, IIS, Nginx) para servir `index.html`.

### Para profundizar
- [Angular: routing](https://angular.dev/guide/routing)
- [Angular: definir rutas](https://angular.dev/guide/routing/define-routes)
- [Angular: referencia del router](https://angular.dev/guide/routing/router-reference)

## Autoevaluación

Contesta primero en voz alta o en papel; luego despliega la respuesta.

> [!question]- ¿Qué necesitas para tener navegación?
> Un arreglo `routes`, `provideRouter(routes)` en la configuración, `<router-outlet />` en la plantilla y enlaces con `routerLink` (importando `RouterLink` y `RouterOutlet`).

> [!question]- ¿Cómo lees un parámetro como `/productos/:id`?
> Con `provideRouter(routes, withComponentInputBinding())` el parámetro llega como input: `id = input.required<string>()`. Alternativa: `inject(ActivatedRoute).paramMap`.

> [!question]- ¿Por qué la ruta comodín `**` debe ir al final?
> El router usa la **primera coincidencia** en orden; si `**` está antes, atrapa todas las URLs.

> [!question]- ¿Para qué sirve `routerLinkActive`?
> Agrega una clase CSS al enlace cuando su ruta está activa: `<a routerLink="/inicio" routerLinkActive="activo">`.

## Práctica

- [[Lab 3 - Router con guards y lazy loading en Angular]]

