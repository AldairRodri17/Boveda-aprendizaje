---
proyecto: Conocimiento
tipo: laboratorio
nivel: intermedio
progreso: pendiente
estado: borrador
area: React
herramienta: Vite + React y React Router
tags: [react, laboratorio]
---
# Lab 4 - Rutas protegidas con React Router (data router)

Parte de: [[05-React/_Índice React|React]]

**Cubre:** [[React Router]] · [[Rutas protegidas]] · [[Context]] · [[zustand]] · [[Error boundaries]]

> [!info] Al terminar
> Si todos los criterios quedan marcados, cambia a `dominado` el `progreso` de este laboratorio y de las notas que cubre. Guarda tus archivos de práctica en esta misma carpeta.

## Objetivo

Montar una app con `createBrowserRouter` que tenga rutas públicas y privadas, proteja con un `loader` que redirige al login conservando la ruta original, y maneje errores con `errorElement`.

## Pasos

1. Crea el proyecto con `npm create vite@latest <nombre> -- --template react` (o `react-ts`), luego `npm install` y `npm run dev`. Instala `react-router` (si tu versión lo requiere, `react-router-dom`; revisa la documentación de la versión que instales).
2. Crea un store de sesión (zustand o un módulo simple) con `usuario`, `iniciar(nombre)` y `cerrar()`, persistido en `sessionStorage`.
3. Rutas: `/` (pública), `/login` (pública), y bajo un layout privado `/panel` y `/panel/ajustes`. Usa `<Outlet />` en el layout.
4. En el `loader` del layout privado: si no hay sesión, `throw redirect('/login?next=' + encodeURIComponent(ruta))`.
5. En `/login`, al iniciar sesión navega a `next` (o `/panel`) con `replace: true`.
6. Agrega `errorElement` a la raíz y una ruta `/panel/roto` cuyo loader lance un error; muestra el mensaje con `useRouteError()`.
7. Botón "Cerrar sesión" que limpie la sesión y navegue a `/`.
8. Prueba: abrir `/panel/ajustes` sin sesión, iniciar sesión, usar Atrás, cerrar sesión y volver a intentar.

## Criterios de terminado

- [ ] Sin sesión, abrir `/panel/ajustes` lleva a `/login?next=%2Fpanel%2Fajustes`.
- [ ] Tras iniciar sesión se llega a `/panel/ajustes`, y "Atrás" no regresa a la pantalla de login.
- [ ] El loader de la ruta privada no se ejecuta (no hay petición) cuando no hay sesión.
- [ ] `/panel/roto` muestra la página de error sin dejar la app en blanco.
- [ ] Recargar `/panel` con sesión activa no redirige.

## Solución

> [!success]- Router y guardia en el loader
> ```jsx
> import { createBrowserRouter, RouterProvider, redirect, Outlet } from 'react-router';
>
> const requiereSesion = ({ request }) => {
>   if (!useSesion.getState().usuario) {
>     const url = new URL(request.url);
>     throw redirect(`/login?next=${encodeURIComponent(url.pathname + url.search)}`);
>   }
>   return null;
> };
>
> const router = createBrowserRouter([
>   { path: '/', element: <Raiz />, errorElement: <PaginaError />, children: [
>     { index: true, element: <Inicio /> },
>     { path: 'login', element: <Login /> },
>     { path: 'panel', element: <Outlet />, loader: requiereSesion, children: [
>       { index: true, element: <Panel /> },
>       { path: 'ajustes', element: <Ajustes /> },
>       { path: 'roto', loader: () => { throw new Error('Falla de prueba'); }, element: <p /> },
>     ]},
>   ]},
> ]);
> ```

> [!success]- Login con regreso
> ```jsx
> function Login() {
>   const [params] = useSearchParams();
>   const navigate = useNavigate();
>   const iniciar = useSesion(s => s.iniciar);
>   return (
>     <form onSubmit={(e) => {
>       e.preventDefault();
>       iniciar(new FormData(e.currentTarget).get('nombre'));
>       navigate(params.get('next') ?? '/panel', { replace: true });
>     }}>
>       <label>Nombre <input name="nombre" required /></label>
>       <button>Entrar</button>
>     </form>
>   );
> }
> ```
> Recuerda: esto solo protege la navegación. La API debe validar la sesión en cada petición.

## Notas personales

_Qué me costó, errores que cometí, dudas._
