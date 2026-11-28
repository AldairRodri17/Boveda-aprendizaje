---
proyecto: Conocimiento
tipo: concepto
nivel: basico
progreso: pendiente
estado: pendiente
area: Modelado 3D artístico
tags: [modelado-3d-artistico]
contenido: completo
---
# Modificadores en Blender

Parte de: [[38-Modelado-3D-Artistico/01 Blender/_Índice Blender|Blender]]

> [!info] Cómo avanzar
> `progreso`: **pendiente** → **viendo** (estudiando) → **visto** (contestaste la autoevaluación sin abrir las respuestas) → **dominado** (hiciste el laboratorio o lo aplicaste de verdad).

## Contenido

### Qué es
Un **modificador** es una operación que Blender aplica a la malla de un objeto **en tiempo real**, sin cambiar la malla original hasta que tú lo decides con **Apply**. Es **no destructivo**: puedes cambiar sus valores, desactivarlo, reordenarlo o borrarlo cuando quieras. Para impresión 3D son la forma más rápida de dar grosor, reflejar, suavizar, cortar o unir piezas, y de preparar un modelo para el laminador.

### Cómo funciona
Los modificadores viven en *Properties → Modifiers* (ícono de llave) y se agregan con **Add Modifier**. Se agrupan en *Edit*, *Generate*, *Deform* y *Physics*. Se apilan en una **pila** que se evalúa de arriba hacia abajo: la salida de uno es la entrada del siguiente, como un *pipeline* de middlewares en ASP.NET Core. **El orden importa**.

```mermaid
flowchart LR
  B[Malla base: media maceta] --> MI[Mirror]
  MI --> SO[Solidify]
  SO --> SU[Subdivision Surface]
  SU --> BO[Boolean: agujero de drenaje]
  BO --> R[Resultado que ves y exportas]
```

Ejemplo del orden: **Mirror antes de Subdivision** suaviza la unión entre mitades; al revés, cada mitad se suaviza por separado y queda una costura. **Solidify antes de Subdivision** da paredes suaves; al revés, Solidify engrosa una malla ya densa (más lento, mismo resultado visual).

**Los más útiles para imprimir**:

| Modificador | Para qué | Nota |
|---|---|---|
| Solidify | dar grosor a superficies | [[Modificador Solidify en Blender]] |
| Boolean | restar, unir, intersectar | [[Modificador Boolean en Blender]] |
| Mirror | simetría | [[Modificador Mirror en Blender]] |
| Subdivision Surface | suavizar | [[Modificador Subdivision Surface en Blender]] |
| Remesh | rehacer la malla con vóxeles | cierra mallas rotas |
| Decimate | reducir polígonos | aligerar esculturas y escaneos |
| Displace | relieve desde una imagen | litofanías y texturas (ver [[Litofanías]]) |

Cada modificador tiene botones para verlo en el viewport, en el render y en Edit Mode. Desactivar temporalmente uno pesado (Subdivision nivel 3) acelera el trabajo.

**Exportar con modificadores sin aplicar**: el exportador STL de Blender tiene la opción **Apply Modifiers** (activada normalmente), así que el archivo incluye su efecto. Aun así, antes de revisar con 3D Print Toolbox conviene **aplicarlos** (`Ctrl+A` sobre el modificador o el menú del modificador → *Apply*), porque las comprobaciones de manifold y grosor miran la malla real, no la evaluada. Haz una copia del objeto (`Shift+D`) o guarda una versión del `.blend` antes de aplicar, para conservar la versión editable.

### Ejemplo
Jarrón decorativo con pila completa, armada por script para reutilizarla:

```python
import bpy

obj = bpy.context.active_object
pila = [
    ("Simetria", 'MIRROR', {"use_axis": (True, False, False), "use_clip": True, "use_mirror_merge": True}),
    ("Pared", 'SOLIDIFY', {"thickness": 1.68, "offset": -1}),
    ("Suave", 'SUBSURF', {"levels": 2, "render_levels": 2}),
]
for nombre, tipo, props in pila:
    m = obj.modifiers.new(nombre, tipo)
    for k, v in props.items():
        setattr(m, k, v)
print([m.name for m in obj.modifiers])
```

### Práctica en tu Centauri 2
Modela media pantalla de lámpara con Mirror + Solidify (1.68 mm) + Subdivision. Cambia el orden de Mirror y Subdivision y observa la costura. Exporta la buena e imprímela en PLA blanco en **modo jarrón** solo si dejaste una sola pared; si tiene grosor de 1.68 mm, imprímela normal con 2 perímetros (ver [[Macetas, lámparas y objetos decorativos]]).

### Cuándo usarlo y cuándo no
Usa modificadores siempre que la operación pueda repetirse o ajustarse: grosores, simetrías, suavizados, agujeros. Aplica solo al final o cuando el siguiente paso lo exige (esculpir sobre el resultado, revisar con 3D Print Toolbox). Para cambios únicos y pequeños, editar la malla en Edit Mode es más directo.

### Errores comunes
- **Orden incorrecto** en la pila: costuras, grosores raros.
- **Aplicar demasiado pronto**: pierdes la flexibilidad; trabaja con copias.
- **Revisar manifold sin aplicar**: 3D Print Toolbox revisa la malla original.
- **Escala sin aplicar**: los valores de Solidify y Bevel salen distorsionados (ver [[Modelado poligonal en Blender]]).

### Para profundizar
- [Manual oficial: introducción a modificadores](https://docs.blender.org/manual/en/latest/modeling/modifiers/introduction.html)
- [Manual oficial: modificador Displace](https://docs.blender.org/manual/en/latest/modeling/modifiers/deform/displace.html)

## Subtemas

- [[Modificador Subdivision Surface en Blender]]
- [[Modificador Mirror en Blender]]
- [[Modificador Solidify en Blender]]
- [[Modificador Boolean en Blender]]

## Autoevaluación

Contesta primero en voz alta o en papel; luego despliega la respuesta.

> [!question]- ¿Qué es un modificador y por qué es no destructivo?
> Una operación que se aplica a la malla en tiempo real sin cambiarla hasta que das **Apply**. Puedes ajustar o quitarla cuando quieras. Se apilan en orden y el orden importa (p. ej. Mirror antes de Subdivision).

> [!question]- ¿Qué modificadores son los más útiles para imprimir?
> **Solidify** (dar grosor), **Boolean** (restar o unir), **Mirror** (simetría), **Subdivision Surface** (suavizar), **Remesh** y **Decimate** (rehacer o reducir la malla) y **Displace** (relieve desde una imagen, útil para litofanías).

> [!question]- ¿Qué pasa al exportar con modificadores sin aplicar?
> El exportador STL suele tener la opción *Apply Modifiers* activada y los incluye. Aun así, antes de revisar con 3D Print Toolbox conviene **aplicarlos**, porque las comprobaciones miran la malla real.

## Práctica

- [[Lab 1 - Maceta con modelado poligonal en Blender]]
- [[Lab 4 - Litofanía con una foto propia]]

## Progreso de los subtemas

```base
filters:
  and:
    - file.inFolder("38-Modelado-3D-Artistico/01 Blender/Modificadores en Blender")
    - file.name != "Modificadores en Blender"
views:
  - type: table
    name: Progreso
    order:
      - file.name
      - progreso
      - nivel
```
