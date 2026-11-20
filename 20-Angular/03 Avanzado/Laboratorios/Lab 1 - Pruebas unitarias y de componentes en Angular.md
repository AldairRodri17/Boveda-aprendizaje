---
proyecto: Conocimiento
tipo: laboratorio
nivel: avanzado
progreso: pendiente
estado: borrador
area: Angular
herramienta: Angular CLI (ng test)
tags: [angular, laboratorio]
---
# Lab 1 - Pruebas unitarias y de componentes en Angular

Parte de: [[20-Angular/03 Avanzado/_Índice Avanzado|Avanzado]]

**Cubre:** [[Pruebas en Angular (unitarias y de componentes)]] · [[Input y Output]] · [[Signals]] · [[HttpClient en Angular]] · [[Interceptores HTTP]]

> [!info] Al terminar
> Si todos los criterios quedan marcados, cambia a `dominado` el `progreso` de este laboratorio y de las notas que cubre. Guarda tus archivos de práctica en esta misma carpeta.

## Objetivo

Escribir pruebas para un servicio con señales, un componente con inputs y outputs, un servicio HTTP y un interceptor, usando `TestBed` y `HttpTestingController`.

## Antes de empezar

Usa el código de los laboratorios anteriores: `CarritoService` (Lab 1 de Intermedio), `TareaItem` (Lab 1 de Básico), `PostsService` y `authInterceptor` (Lab 2 de Intermedio). Ejecuta `ng test` una vez para ver qué runner tiene tu proyecto; el código de abajo usa solo `describe`, `it`, `beforeEach` y `expect(...).toBe/toEqual`, que funcionan igual en Vitest y en Jasmine.

## Pasos

1. **CarritoService:** prueba que agregar dos veces el mismo producto deja una línea con cantidad 2 y que `total()` se calcula bien. Limpia `localStorage` antes de cada prueba.
2. **TareaItem:** pasa una tarea con `setInput`, verifica que el título aparece en *Title Case* y que al hacer clic en **Eliminar** se emite el `id`.
3. **PostsService:** con `HttpTestingController`, comprueba que `listar()` hace un `GET` a la URL correcta y devuelve lo que responde el servidor simulado.
4. **authInterceptor:** comprueba que una petición sale con el header `Authorization`.
5. Rompe a propósito una prueba (cambia un valor esperado), mira el mensaje de error y vuelve a dejarla en verde.
6. **Reto:** prueba que el componente `Posts` muestra el mensaje de error cuando el servicio responde 500.

## Criterios de terminado

- [ ] `ng test` termina con todas las pruebas en verde.
- [ ] Hay al menos 5 pruebas: 2 del carrito, 2 del componente, 1 del servicio HTTP y 1 del interceptor.
- [ ] Ninguna prueba llama al API real (las pruebas HTTP terminan con `httpMock.verify()` sin peticiones pendientes).
- [ ] Vi fallar una prueba a propósito y entendí el mensaje.
- [ ] Reto: la prueba del error 500 pasa.

## Solución

> [!tip] Los `import` de Angular y RxJS se omiten en algunos fragmentos para abreviar; VS Code los agrega con **Ctrl + .** sobre el nombre marcado en rojo.

> [!success]- carrito.service.spec.ts
> ```ts
> describe('CarritoService', () => {
>   let carrito: CarritoService;
>   const cafe = { id: 1, nombre: 'Café', precio: 50 };
>
>   beforeEach(() => {
>     localStorage.clear();
>     TestBed.configureTestingModule({});
>     carrito = TestBed.inject(CarritoService);
>   });
>
>   it('agrupa el mismo producto en una línea', () => {
>     carrito.agregar(cafe);
>     carrito.agregar(cafe);
>     expect(carrito.items().length).toBe(1);
>     expect(carrito.items()[0].cantidad).toBe(2);
>   });
>
>   it('calcula el total', () => {
>     carrito.agregar(cafe);
>     carrito.agregar({ id: 2, nombre: 'Pan', precio: 20 });
>     expect(carrito.total()).toBe(70);
>   });
> });
> ```

> [!success]- tarea-item.spec.ts
> ```ts
> describe('TareaItem', () => {
>   let fixture: ComponentFixture<TareaItem>;
>
>   beforeEach(() => {
>     TestBed.configureTestingModule({ imports: [TareaItem] });
>     fixture = TestBed.createComponent(TareaItem);
>     fixture.componentRef.setInput('tarea', { id: 7, titulo: 'comprar pan', hecha: false });
>     fixture.detectChanges();
>   });
>
>   it('muestra el título en Title Case', () => {
>     expect(fixture.nativeElement.textContent).toContain('Comprar Pan');
>   });
>
>   it('emite el id al eliminar', () => {
>     let emitido: number | undefined;
>     fixture.componentInstance.eliminar.subscribe(id => (emitido = id));
>     fixture.nativeElement.querySelector('button').click();
>     expect(emitido).toBe(7);
>   });
> });
> ```

> [!success]- posts.service.spec.ts e interceptor
> ```ts
> describe('PostsService', () => {
>   let service: PostsService;
>   let httpMock: HttpTestingController;
>
>   beforeEach(() => {
>     TestBed.configureTestingModule({
>       providers: [provideHttpClient(withInterceptors([authInterceptor])), provideHttpClientTesting()],
>     });
>     service = TestBed.inject(PostsService);
>     httpMock = TestBed.inject(HttpTestingController);
>   });
>
>   afterEach(() => httpMock.verify());
>
>   it('listar() hace GET y devuelve los posts', () => {
>     let res: Post[] = [];
>     service.listar().subscribe(p => (res = p));
>     const req = httpMock.expectOne('https://jsonplaceholder.typicode.com/posts');
>     expect(req.request.method).toBe('GET');
>     req.flush([{ userId: 1, id: 1, title: 'a', body: 'b' }]);
>     expect(res.length).toBe(1);
>   });
>
>   it('el interceptor agrega Authorization', () => {
>     service.obtener(1).subscribe();
>     const req = httpMock.expectOne('https://jsonplaceholder.typicode.com/posts/1');
>     expect(req.request.headers.get('Authorization')).toBe('Bearer demo-123');
>     req.flush({});
>   });
> });
> ```
> Si agregaste `retry` en el reto del Lab 2, las pruebas de error deben responder varias veces o quitar el reintento en la prueba.

> [!success]- Reto: error 500 en el componente
> ```ts
> it('muestra error si el API falla', async () => {
>   TestBed.configureTestingModule({
>     imports: [Posts],
>     providers: [provideHttpClient(), provideHttpClientTesting()],
>   });
>   const fixture = TestBed.createComponent(Posts);
>   fixture.detectChanges();                       // dispara ngOnInit
>   const httpMock = TestBed.inject(HttpTestingController);
>   httpMock.expectOne(r => r.url.endsWith('/posts'))
>     .flush('falla', { status: 500, statusText: 'Server Error' });
>   fixture.detectChanges();
>   await fixture.whenStable();
>   expect(fixture.nativeElement.textContent).toContain('No se pudieron cargar');
> });
> ```

## Notas personales

_Qué me costó, errores que cometí, dudas._
