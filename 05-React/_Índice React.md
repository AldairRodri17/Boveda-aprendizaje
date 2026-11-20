---
proyecto: Conocimiento
tipo: indice
estado: en-progreso
area: React
tags: [indice, react]
---
# React

Avance general en [[Temario]] · Mapa mental: [[Mapa mental - React.canvas|Mapa mental]].

> [!info] Cómo avanzar
> Cada nota tiene **Autoevaluación** (preguntas con la respuesta plegada). Márcala `visto` cuando la contestes sin abrir las respuestas. Los laboratorios llevan las notas a `dominado`.

## Temas

- [[React]]
- [[Qué es React y por qué existe]]
- [[JSX]]
- [[Componentes]]
- [[Props]]
- [[Estado con useState]]
- [[Eventos en React]]
- [[Listas y keys]]
- [[Renderizado condicional]]
- [[Componentes controlados]]
- [[useEffect]]
- [[useRef]]
- [[useMemo y useCallback]]
- [[useReducer]]
- [[Context]]
- [[Custom hooks]]
- [[React Router]]
- [[Rutas protegidas]]
- [[TanStack Query]]
- [[zustand]]
- [[dnd-kit]]
- [[react-easy-crop]]
- [[Suspense y lazy loading]]
- [[Error boundaries]]
- [[Rendimiento en React]]
- [[Pruebas con Testing Library]]
- [[Estilos (CSS Modules y Tailwind)]]
- [[Estructura de carpetas por feature]]
- [[Next.js (SSR, SSG y App Router)]]
  - [[SSR en Next.js]]
  - [[SSG e ISR en Next.js]]
  - [[App Router de Next.js]]
- [[Server Components]]
- [[Máquinas de estado en UI (XState)]]

## Laboratorios

- [[Lab 1 - App de notas con estado y efectos]]
- [[Lab 2 - TanStack Query con caché e invalidación]]
- [[Lab 3 - Formulario controlado con validación]]
- [[Lab 4 - Rutas protegidas con React Router (data router)]]

## Progreso

```base
filters:
  and:
    - file.inFolder("05-React")
    - file.name != "_Índice React"
views:
  - type: table
    name: Temas
    filters:
      and:
        - tipo != "laboratorio"
    order:
      - file.name
      - progreso
      - nivel
    sort: []
  - type: table
    name: Laboratorios
    filters:
      and:
        - tipo == "laboratorio"
    order:
      - file.name
      - progreso

```

## Progreso de toda el área

```base
filters:
  and:
    - file.inFolder("05-React")
    - tipo != "indice"
views:
  - type: table
    name: Pendientes
    filters:
      and:
        - progreso != "visto"
        - progreso != "dominado"
    order:
      - file.name
      - file.folder
      - progreso
      - nivel
  - type: table
    name: Vistos
    filters:
      and:
        - progreso == "visto"
    order:
      - file.name
      - file.folder
  - type: table
    name: Dominados
    filters:
      and:
        - progreso == "dominado"
    order:
      - file.name
      - file.folder
```
