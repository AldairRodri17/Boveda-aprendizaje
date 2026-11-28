---
proyecto: Conocimiento
tipo: concepto
nivel: intermedio
progreso: pendiente
estado: pendiente
area: React
tags: [react]
contenido: completo
---
# Estilos (CSS Modules y Tailwind)

Parte de: [[05-React/_Índice React|React]]

> [!info] Cómo avanzar
> `progreso`: **pendiente** → **viendo** (estudiando) → **visto** (contestaste la autoevaluación sin abrir las respuestas) → **dominado** (hiciste el laboratorio o lo aplicaste de verdad).

## Contenido

### Qué es
React no impone una forma de dar estilos: puedes usar CSS global, CSS Modules, Tailwind, CSS-in-JS o estilos en línea. Dos enfoques dominan en proyectos con Vite:

- **CSS Modules**: escribes CSS normal en archivos `*.module.css`, pero cada clase se vuelve **local al componente** que la importa. El compilador renombra `.boton` a algo único como `_boton_x7f2a_1`, así que dos componentes pueden tener `.boton` sin chocar.
- **Tailwind CSS**: un framework de **clases utilitarias** (`flex`, `gap-2`, `p-4`, `text-sm`, `bg-blue-600`) que aplicas directamente en el JSX. Casi no escribes CSS propio; compones el diseño con clases que siguen una escala predefinida de espacios, colores y tamaños.

### Cómo funciona
#### CSS Modules
Vite soporta CSS Modules sin configuración: cualquier archivo que termine en `.module.css` se procesa así. Al importarlo obtienes un objeto cuyas llaves son tus nombres de clase y cuyos valores son los nombres únicos generados.

```css
/* Tarjeta.module.css */
.tarjeta { border: 1px solid #ddd; border-radius: 8px; padding: 1rem; }
.destacada { border-color: #2563eb; }
.titulo { font-size: 1.125rem; margin: 0 0 .5rem; }
```

```tsx
import styles from './Tarjeta.module.css';

export function Tarjeta({ titulo, destacada }: { titulo: string; destacada?: boolean }) {
  return (
    <div className={destacada ? `${styles.tarjeta} ${styles.destacada}` : styles.tarjeta}>
      <h3 className={styles.titulo}>{titulo}</h3>
    </div>
  );
}
```

Ventajas: es CSS real (media queries, variables CSS, `:hover`, animaciones), el estilo vive junto al componente y no hay fugas globales. Desventajas: tienes que inventar nombres de clase y mantener dos archivos por componente; los valores de diseño (colores, espacios) se mantienen consistentes solo si usas variables CSS compartidas.

#### Tailwind
Tailwind escanea tus archivos fuente buscando nombres de clase y genera **solo el CSS de las clases que usas**, por eso el CSS final suele ser pequeño. En la versión 4 se integra con Vite mediante el plugin `@tailwindcss/vite` y en tu CSS principal basta `@import "tailwindcss";` (revisa la guía oficial de instalación para tu versión).

```tsx
export function Tarjeta({ titulo, destacada }: { titulo: string; destacada?: boolean }) {
  return (
    <div className={`rounded-lg border p-4 ${destacada ? 'border-blue-600' : 'border-gray-300'}`}>
      <h3 className="mb-2 text-lg font-semibold">{titulo}</h3>
    </div>
  );
}
```

Ventajas: rapidez (no cambias de archivo ni inventas nombres), consistencia por la escala de diseño, responsive y estados con prefijos (`md:flex`, `hover:bg-blue-700`, `dark:bg-gray-900`), CSS final pequeño. Desventajas: JSX con listas largas de clases, curva para aprender los nombres, y el diseño queda acoplado al marcado. Las clases deben aparecer **completas** en el código: `bg-${color}-600` construido dinámicamente no se detecta al compilar.

### Clases condicionales legibles
Concatenar cadenas con ternarios se vuelve ilegible rápido. La utilidad `clsx` (muy pequeña) recibe cadenas, objetos y valores falsos, y devuelve solo las clases activas:

```tsx
import clsx from 'clsx';

// Con CSS Modules
<li className={clsx(styles.item, activo && styles.activo, deshabilitado && styles.deshabilitado)} />

// Con Tailwind
<button className={clsx('rounded px-3 py-1', primario ? 'bg-blue-600 text-white' : 'bg-gray-100', cargando && 'opacity-50')} />
```

Con Tailwind se suele combinar con `tailwind-merge` (función `twMerge`), que resuelve conflictos cuando un componente recibe clases desde fuera: `twMerge('px-2', 'px-4')` deja solo `px-4`. Para componentes con muchas variantes (tamaño × color × estado) existen utilidades como `cva` (class-variance-authority).

### Cuándo usar cada uno
| Situación | Recomendación |
|---|---|
| Equipo cómodo con CSS, diseño propio detallado, animaciones | CSS Modules |
| Prototipado rápido, sistema de diseño basado en escala, muchas pantallas internas | Tailwind |
| Estilos globales: reset, tipografía, variables de tema | Un CSS global (`index.css`) en cualquiera de los dos |
| Valores calculados en tiempo de ejecución (posición de un elemento arrastrado, ancho de una barra de progreso) | `style={{ width: pct + '%' }}` o variables CSS |

Ambos pueden convivir: Tailwind para el layout y CSS Modules para un componente con estilos complejos. Lo importante en un proyecto como Itzamna.Clio es elegir **un enfoque principal** y documentarlo, para no terminar con cinco formas de dar estilo. Coloca el `.module.css` junto al componente dentro de su feature (ver [[Estructura de carpetas por feature]]).

### Errores comunes
- **Usar guiones en CSS Modules** (`.boton-primario`) y luego `styles.boton-primario`: no es válido en JS; usa `styles['boton-primario']` o camelCase.
- **Construir clases de Tailwind dinámicamente** (`text-${tamano}`): no se generan. Usa un mapa con las clases completas.
- **Concatenar con ternarios anidados**: usa `clsx`.
- **Estilos en línea para todo**: no admiten `:hover`, media queries ni pseudo-elementos, y dificultan el mantenimiento.
- **Mezclar enfoques sin regla**: CSS global, módulos, Tailwind y estilos en línea en el mismo componente.

### Para profundizar
- [CSS Modules en Vite](https://vite.dev/guide/features#css-modules)
- [Documentación de Tailwind CSS](https://tailwindcss.com/docs)
- [Repositorio y especificación de CSS Modules](https://github.com/css-modules/css-modules)
- [clsx (GitHub)](https://github.com/lukeed/clsx)

## Autoevaluación

Contesta primero en voz alta o en papel; luego despliega la respuesta.

> [!question]- ¿Qué hace CSS Modules?
> Cada `archivo.module.css` genera nombres de clase únicos al compilar; importas `styles.boton` y no hay choques globales. Vite lo soporta sin configuración.

> [!question]- ¿Qué propone Tailwind y qué ventajas/desventajas tiene?
> Clases utilitarias (`p-4 flex gap-2`) directamente en el JSX. Ventajas: rapidez, consistencia por su escala de diseño, CSS final pequeño. Desventajas: JSX con muchas clases y curva para aprender los nombres.

> [!question]- ¿Cómo aplicas clases condicionales de forma legible?
> Con una pequeña utilidad como `clsx`: `className={clsx(styles.item, activo && styles.activo)}`.

