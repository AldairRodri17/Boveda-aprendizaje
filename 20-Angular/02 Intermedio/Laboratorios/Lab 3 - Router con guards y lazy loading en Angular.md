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
# Lab 3 - Router con guards y lazy loading en Angular

Parte de: [[20-Angular/02 Intermedio/_Índice Intermedio|Intermedio]]

**Cubre:** [[Router de Angular]] · [[Guards y resolvers]] · [[Lazy loading de rutas]] · [[Servicios e inyección de dependencias en Angular]]

> [!info] Al terminar
> Si todos los criterios quedan marcados, cambia a `dominado` el `progreso` de este laboratorio y de las notas que cubre. Guarda tus archivos de práctica en esta misma carpeta.

## Objetivo

Armar la navegación de una app con rutas normales, rutas con parámetro, una sección protegida por guard y dos secciones cargadas con lazy loading.

## Rutas a construir

| Ruta | Qué muestra | Carga |
|---|---|---|
| `/` | Inicio | normal |
| `/login` | Botón "Entrar como demo" | normal |
| `/productos` | Lista de productos | lazy (`loadChildren`) |
| `/productos/:id` | Detalle | lazy (dentro del mismo archivo de rutas) |
| `/admin` | Panel | lazy (`loadComponent`) + guard |
| `**` | Página no encontrada | normal |

## Pasos

1. Crea los componentes `inicio`, `login`, `no-encontrado`, `admin`, y en una carpeta `productos` los componentes `lista` y `detalle`.
2. Crea `AuthService` con una señal `logueado` y métodos `entrar()` y `salir()`.
3. Crea `productos/productos.routes.ts` con las rutas `''` y `':id'`.
4. Define `app.routes.ts` según la tabla. Activa `withComponentInputBinding()` para recibir `id` como input.
5. Crea `authGuard` (`ng g guard auth`, tipo *CanActivate*) que, sin sesión, redirija a `/login?returnUrl=/admin`.
6. En `login`, al entrar, navega a `returnUrl` (o a `/` si no hay).
7. Agrega un menú con `routerLink` y `routerLinkActive`, y un botón **Salir**.
8. Ejecuta `ng build` y revisa la salida.
9. **Reto:** agrega un `CanDeactivateFn` en `admin` que pregunte con `confirm()` si hay cambios sin guardar, y un `ResolveFn` que cargue el producto antes de abrir el detalle.

## Criterios de terminado

- [ ] Navegar entre Inicio y Productos no recarga la página (solo cambia el `router-outlet`).
- [ ] `/productos/3` muestra "Producto 3" leyendo el `id` como input.
- [ ] Sin sesión, ir a `/admin` redirige a `/login?returnUrl=%2Fadmin`; tras entrar, regresa a `/admin`.
- [ ] Una URL inexistente muestra la página de no encontrado.
- [ ] La salida de `ng build` lista *lazy chunks* para productos y admin.
- [ ] En Network, el chunk de admin se descarga solo la primera vez que entro a `/admin`.
- [ ] Puedo explicar por qué el guard no sustituye la autorización del backend.

## Solución

> [!tip] Los `import` de Angular y RxJS se omiten en algunos fragmentos para abreviar; VS Code los agrega con **Ctrl + .** sobre el nombre marcado en rojo.

> [!success]- app.routes.ts y app.config.ts
> ```ts
> export const routes: Routes = [
>   { path: '', component: Inicio, title: 'Inicio' },
>   { path: 'login', component: Login },
>   { path: 'productos', loadChildren: () => import('./productos/productos.routes').then(m => m.PRODUCTOS_ROUTES) },
>   { path: 'admin', canActivate: [authGuard], loadComponent: () => import('./admin/admin').then(m => m.Admin) },
>   { path: '**', component: NoEncontrado },
> ];
>
> // app.config.ts
> provideRouter(routes, withComponentInputBinding())
> ```
> Ajusta la ruta del `import()` al nombre de archivo que generó tu CLI (`admin.ts` o `admin.component.ts`).

> [!success]- productos.routes.ts y detalle
> ```ts
> export const PRODUCTOS_ROUTES: Routes = [
>   { path: '', component: Lista },
>   { path: ':id', component: Detalle },
> ];
>
> @Component({ selector: 'app-detalle', template: `<h2>Producto {{ id() }}</h2>` })
> export class Detalle {
>   id = input.required<string>();
> }
> ```

> [!success]- AuthService y authGuard
> ```ts
> @Injectable({ providedIn: 'root' })
> export class AuthService {
>   readonly logueado = signal(false);
>   entrar() { this.logueado.set(true); }
>   salir() { this.logueado.set(false); }
> }
>
> export const authGuard: CanActivateFn = (route, state) =>
>   inject(AuthService).logueado()
>     ? true
>     : inject(Router).createUrlTree(['/login'], { queryParams: { returnUrl: state.url } });
> ```

> [!success]- Login
> ```ts
> @Component({ selector: 'app-login', template: `<button (click)="entrar()">Entrar como demo</button>` })
> export class Login {
>   private auth = inject(AuthService);
>   private router = inject(Router);
>   returnUrl = input<string>();   // llega del query param gracias a withComponentInputBinding
>   entrar() {
>     this.auth.entrar();
>     this.router.navigateByUrl(this.returnUrl() ?? '/');
>   }
> }
> ```

> [!success]- Menú
> ```html
> <nav>
>   <a routerLink="/" routerLinkActive="activo" [routerLinkActiveOptions]="{ exact: true }">Inicio</a>
>   <a routerLink="/productos" routerLinkActive="activo">Productos</a>
>   <a routerLink="/admin" routerLinkActive="activo">Admin</a>
>   <button (click)="auth.salir()">Salir</button>
> </nav>
> <router-outlet />
> ```
> El componente raíz importa `RouterLink`, `RouterLinkActive` y `RouterOutlet`.

> [!success]- Reto: CanDeactivate y Resolve
> ```ts
> export const cambiosGuard: CanDeactivateFn<Admin> = comp =>
>   !comp.hayCambios() || confirm('Tienes cambios sin guardar. ¿Salir?');
>
> export const productoResolver: ResolveFn<Producto> = route =>
>   inject(ProductosService).obtener(Number(route.paramMap.get('id')));
>
> // rutas
> { path: 'admin', canActivate: [authGuard], canDeactivate: [cambiosGuard], loadComponent: … }
> { path: ':id', component: Detalle, resolve: { producto: productoResolver } }
> ```
> Con `withComponentInputBinding()`, el dato resuelto llega como input `producto`.

## Notas personales

_Qué me costó, errores que cometí, dudas._
