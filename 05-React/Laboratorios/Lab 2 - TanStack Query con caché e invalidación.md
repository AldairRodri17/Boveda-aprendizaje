---
proyecto: Conocimiento
tipo: laboratorio
nivel: intermedio
progreso: pendiente
estado: borrador
area: React
herramienta: Vite + React, TanStack Query, json-server y React Query Devtools
tags: [react, laboratorio]
contenido: completo
---
# Lab 2 - TanStack Query con caché e invalidación

Parte de: [[05-React/_Índice React|React]]

**Cubre:** [[TanStack Query]] · [[Custom hooks]] · [[Suspense y lazy loading]] · [[Error boundaries]]

> [!info] Al terminar
> Si todos los criterios quedan marcados, cambia a `dominado` el `progreso` de este laboratorio y de las notas que cubre. Guarda tus archivos de práctica en esta misma carpeta.

## Objetivo

Conectar una lista de tareas a una API local con TanStack Query: consultas con clave por parámetro, mutaciones que invalidan la caché, una actualización optimista y observar todo en las Devtools.

## Pasos

1. Crea el proyecto con `npm create vite@latest <nombre> -- --template react` (o `react-ts`), luego `npm install` y `npm run dev`. Instala `@tanstack/react-query` y `@tanstack/react-query-devtools`.
2. Crea `db.json` con `{ "tareas": [ { "id": "1", "titulo": "Leer", "hecha": false } ] }` y levanta la API con `npx json-server db.json --port 3001`.
3. Envuelve la app en `QueryClientProvider` y agrega `<ReactQueryDevtools />`.
4. Crea hooks en `api/tareas.js`: `useTareas()` con `queryKey: ['tareas']`, `useTarea(id)` con `['tareas', id]`, `useCrearTarea()` y `useAlternarTarea()`.
5. `useCrearTarea` invalida `['tareas']` en `onSuccess`. Verifica en Network que se vuelve a pedir la lista.
6. Pon `staleTime: 30_000` en `useTareas`. Cambia de pestaña del navegador y regresa: observa que no se vuelve a pedir mientras el dato esté fresco.
7. Haz `useAlternarTarea` optimista: en `onMutate` cancela consultas, guarda el estado previo, actualiza la caché con `setQueryData`; en `onError` restaura; en `onSettled` invalida.
8. Detén json-server y prueba alternar una tarea: debe regresar a su estado previo y mostrarse un error.

## Criterios de terminado

- [ ] Al crear una tarea, la lista se actualiza sola sin recargar la página.
- [ ] Con `staleTime: 30_000`, volver a la pestaña antes de 30 s no genera petición nueva en Network.
- [ ] En las Devtools ves las claves `['tareas']` y `['tareas', id]` por separado.
- [ ] Con la API apagada, el cambio optimista se revierte y se muestra un mensaje de error.
- [ ] Ningún componente llama `fetch` directamente: todo pasa por los hooks de `api/tareas.js`.

## Solución

> [!success]- Hooks de consulta y creación
> ```js
> const API = 'http://localhost:3001/tareas';
> const pedir = async (url, init) => {
>   const r = await fetch(url, init);
>   if (!r.ok) throw new Error(`HTTP ${r.status}`);
>   return r.json();
> };
>
> export const useTareas = () =>
>   useQuery({ queryKey: ['tareas'], queryFn: () => pedir(API), staleTime: 30_000 });
>
> export const useTarea = (id) =>
>   useQuery({ queryKey: ['tareas', id], queryFn: () => pedir(`${API}/${id}`), enabled: !!id });
>
> export function useCrearTarea() {
>   const qc = useQueryClient();
>   return useMutation({
>     mutationFn: (titulo) => pedir(API, {
>       method: 'POST', headers: { 'Content-Type': 'application/json' },
>       body: JSON.stringify({ titulo, hecha: false }),
>     }),
>     onSuccess: () => qc.invalidateQueries({ queryKey: ['tareas'] }),
>   });
> }
> ```

> [!success]- Actualización optimista
> ```js
> export function useAlternarTarea() {
>   const qc = useQueryClient();
>   return useMutation({
>     mutationFn: (t) => pedir(`${API}/${t.id}`, {
>       method: 'PATCH', headers: { 'Content-Type': 'application/json' },
>       body: JSON.stringify({ hecha: !t.hecha }),
>     }),
>     onMutate: async (t) => {
>       await qc.cancelQueries({ queryKey: ['tareas'] });
>       const previas = qc.getQueryData(['tareas']);
>       qc.setQueryData(['tareas'], (xs = []) => xs.map(x => x.id === t.id ? { ...x, hecha: !x.hecha } : x));
>       return { previas };
>     },
>     onError: (_e, _t, ctx) => qc.setQueryData(['tareas'], ctx.previas),
>     onSettled: () => qc.invalidateQueries({ queryKey: ['tareas'] }),
>   });
> }
> ```

## Notas personales

_Qué me costó, errores que cometí, dudas._
