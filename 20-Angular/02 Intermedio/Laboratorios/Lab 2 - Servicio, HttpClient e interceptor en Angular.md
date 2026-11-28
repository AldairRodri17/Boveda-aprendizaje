---
proyecto: Conocimiento
tipo: laboratorio
nivel: intermedio
progreso: pendiente
estado: borrador
area: Angular
herramienta: Angular CLI y el API público JSONPlaceholder
tags: [angular, laboratorio]
contenido: completo
---
# Lab 2 - Servicio, HttpClient e interceptor en Angular

Parte de: [[20-Angular/02 Intermedio/_Índice Intermedio|Intermedio]]

**Cubre:** [[HttpClient en Angular]] · [[Interceptores HTTP]] · [[Servicios e inyección de dependencias en Angular]] · [[RxJS - Observables]] · [[RxJS - operadores más usados]] · [[Manejo de errores en Angular]]

> [!info] Al terminar
> Si todos los criterios quedan marcados, cambia a `dominado` el `progreso` de este laboratorio y de las notas que cubre. Guarda tus archivos de práctica en esta misma carpeta.

## Objetivo

Consumir un API REST desde un servicio con HttpClient, mostrar estados de carga y error, y agregar dos interceptores: uno que pone un header de autorización y otro que registra y maneja errores.

## API

`https://jsonplaceholder.typicode.com` — API falso y gratuito: `GET /posts`, `GET /posts/1`, `POST /posts` (responde **201** con un `id` simulado; no guarda nada de verdad). Si tu red lo bloquea, crea un `db.json` y usa `npx json-server db.json` como API local.

## Pasos

1. En `app.config.ts` agrega `provideHttpClient(withInterceptors([...]))`.
2. Crea una interfaz `Post` (`userId`, `id`, `title`, `body`) y un servicio `PostsService` con `listar()`, `obtener(id)` y `crear(post)`.
3. Crea un componente `posts` que muestre los primeros 10 posts con estados *Cargando…*, *error* y *lista*, usando señales.
4. Agrega un formulario mínimo (título y cuerpo) que haga `POST` y agregue el post creado al inicio de la lista.
5. Crea `authInterceptor`: agrega `Authorization: Bearer demo-123` a cada petición.
6. Crea `logInterceptor`: registra en consola método, URL, status y milisegundos; si hay error, lo registra y lo relanza.
7. Provoca un error pidiendo `/posts-que-no-existen` (botón **Probar error**) y muestra un mensaje amable.
8. **Reto:** agrega `retry({ count: 2, delay: 500 })` solo a las peticiones GET dentro del servicio y comprueba en Network que reintenta.

## Criterios de terminado

- [ ] Se ven 10 posts y, mientras cargan, el texto *Cargando…*.
- [ ] En DevTools → Network, la petición lleva el header `Authorization: Bearer demo-123`.
- [ ] El `POST` responde **201** y el post nuevo aparece al inicio de la lista.
- [ ] La consola muestra líneas como `GET https://jsonplaceholder.typicode.com/posts → 200 en 85 ms`.
- [ ] El botón *Probar error* muestra un mensaje en pantalla (no se rompe la app) y la consola registra el 404.
- [ ] El componente no usa `HttpClient` directamente: todo pasa por el servicio.
- [ ] Reto: la petición fallida aparece 3 veces en Network.

## Solución

> [!tip] Los `import` de Angular y RxJS se omiten en algunos fragmentos para abreviar; VS Code los agrega con **Ctrl + .** sobre el nombre marcado en rojo.

> [!success]- app.config.ts
> ```ts
> export const appConfig: ApplicationConfig = {
>   providers: [
>     provideRouter(routes),
>     provideHttpClient(withInterceptors([authInterceptor, logInterceptor])),
>   ],
> };
> ```
> (Conserva los demás providers que haya generado el CLI).

> [!success]- posts.service.ts
> ```ts
> export interface Post { userId: number; id: number; title: string; body: string; }
>
> @Injectable({ providedIn: 'root' })
> export class PostsService {
>   private http = inject(HttpClient);
>   private base = 'https://jsonplaceholder.typicode.com';
>
>   listar() { return this.http.get<Post[]>(`${this.base}/posts`); }
>   obtener(id: number) { return this.http.get<Post>(`${this.base}/posts/${id}`); }
>   crear(p: Omit<Post, 'id'>) { return this.http.post<Post>(`${this.base}/posts`, p); }
>   fallar() { return this.http.get(`${this.base}/posts-que-no-existen`); }
> }
> ```

> [!success]- Interceptores
> ```ts
> export const authInterceptor: HttpInterceptorFn = (req, next) =>
>   next(req.clone({ setHeaders: { Authorization: 'Bearer demo-123' } }));
>
> export const logInterceptor: HttpInterceptorFn = (req, next) => {
>   const inicio = performance.now();
>   const ms = () => Math.round(performance.now() - inicio);
>   return next(req).pipe(
>     tap(ev => {
>       if (ev instanceof HttpResponse) console.log(`${req.method} ${req.urlWithParams} → ${ev.status} en ${ms()} ms`);
>     }),
>     catchError((err: HttpErrorResponse) => {
>       console.error(`${req.method} ${req.urlWithParams} → ${err.status} en ${ms()} ms`, err.message);
>       return throwError(() => err);
>     }),
>   );
> };
> ```

> [!success]- Componente posts
> ```ts
> @Component({
>   selector: 'app-posts',
>   imports: [FormsModule],
>   template: `
>     <form (ngSubmit)="crear()">
>       <input name="titulo" [(ngModel)]="titulo" placeholder="Título" required />
>       <input name="cuerpo" [(ngModel)]="cuerpo" placeholder="Cuerpo" />
>       <button [disabled]="!titulo">Crear</button>
>       <button type="button" (click)="probarError()">Probar error</button>
>     </form>
>     @if (error()) { <p class="error">{{ error() }}</p> }
>     @if (cargando()) { <p>Cargando…</p> } @else {
>       @for (p of posts(); track p.id) { <article><h3>{{ p.title }}</h3><p>{{ p.body }}</p></article> }
>     }
>   `,
> })
> export class Posts implements OnInit {
>   private api = inject(PostsService);
>   private destroyRef = inject(DestroyRef);
>   posts = signal<Post[]>([]);
>   cargando = signal(true);
>   error = signal<string | null>(null);
>   titulo = '';
>   cuerpo = '';
>
>   ngOnInit() {
>     this.api.listar().pipe(takeUntilDestroyed(this.destroyRef)).subscribe({
>       next: ps => { this.posts.set(ps.slice(0, 10)); this.cargando.set(false); },
>       error: () => { this.error.set('No se pudieron cargar los posts.'); this.cargando.set(false); },
>     });
>   }
>   crear() {
>     this.api.crear({ userId: 1, title: this.titulo, body: this.cuerpo }).subscribe(p => {
>       this.posts.update(ps => [p, ...ps]);
>       this.titulo = ''; this.cuerpo = '';
>     });
>   }
>   probarError() {
>     this.api.fallar().subscribe({ error: (e: HttpErrorResponse) => this.error.set(`Error ${e.status}: recurso no encontrado.`) });
>   }
> }
> ```
> Como JSONPlaceholder no guarda nada, todos los posts creados reciben el mismo `id` simulado; si creas varios, usa `track $index` o genera un id local.

> [!success]- Reto: reintentos solo en GET
> ```ts
> listar() { return this.http.get<Post[]>(`${this.base}/posts`).pipe(retry({ count: 2, delay: 500 })); }
> fallar() { return this.http.get(`${this.base}/posts-que-no-existen`).pipe(retry({ count: 2, delay: 500 })); }
> ```
> No reintentes `POST` a ciegas: podrías crear el recurso dos veces.

## Notas personales

_Qué me costó, errores que cometí, dudas._
