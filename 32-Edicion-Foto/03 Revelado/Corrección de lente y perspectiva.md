---
proyecto: Conocimiento
tipo: concepto
nivel: basico
progreso: pendiente
estado: pendiente
area: Edición de fotografía
tags: [edicion-foto]
---
# Corrección de lente y perspectiva

Parte de: [[32-Edicion-Foto/03 Revelado/_Índice Revelado|Revelado]]

> [!info] Cómo avanzar
> `progreso`: **pendiente** → **viendo** (estudiando) → **visto** (contestaste la autoevaluación sin abrir las respuestas) → **dominado** (hiciste el laboratorio o lo aplicaste de verdad).

## Contenido

### Qué es
Toda lente tiene defectos ópticos medibles. La **corrección de lente** los compensa con software usando un perfil de esa lente: **distorsión** (líneas rectas que se curvan en barril o cojín), **viñeteo** (esquinas más oscuras) y **aberración cromática transversal** (bordes de color magenta o verde en zonas de alto contraste). La **corrección de perspectiva** es distinta: arregla la convergencia de líneas que aparece cuando inclinas la cámara hacia arriba o abajo, como edificios que parecen caerse hacia atrás.

### Cómo funciona
**Corrección de lente en darktable**: el módulo **lens correction** detecta la cámara y la lente por los metadatos EXIF y aplica el perfil. Tiene dos fuentes de datos:
- **Lensfun**: base de datos libre de perfiles de lentes medidos por la comunidad.
- **Metadatos incrustados**: muchos fabricantes, entre ellos Sony, guardan en el RAW los parámetros de corrección que la cámara aplicaría al JPEG; darktable puede usarlos directamente.

Activa esta corrección **antes de ajustar la exposición**, porque al quitar el viñeteo cambia el brillo de las esquinas.

**Perspectiva**: el módulo **rotate and perspective** (rotar y perspectiva) puede detectar líneas automáticamente (*estructura*) o dejarte dibujar líneas verticales y horizontales, o un rectángulo, que deben quedar rectas. Luego deforma la imagen para que esas líneas queden paralelas; al final recortas los bordes vacíos.

### Ejemplo
Con la a6700 y la lente de kit 16–50 mm a 16 mm, fotografías una iglesia desde abajo:
1. *lens correction*: método *metadatos incrustados* o Lensfun; desaparece el barril típico del 16 mm y las esquinas oscuras.
2. *rotate and perspective*: dibuja dos líneas sobre las aristas verticales de la fachada y aplica corrección vertical. El edificio deja de "caerse hacia atrás".
3. Ajusta el **aspecto** (*lens shift* o *aspect adjust*) si la fachada quedó demasiado alargada; las correcciones fuertes estiran la parte de arriba.
4. Recorta a 4:5 dejando aire arriba, porque la corrección se come parte del encuadre.

Consejo de toma: si sabes que vas a corregir, deja **margen alrededor** al disparar.

En el iPhone 15 Pro, las fotos ya salen con distorsión corregida; en la app Fotos, *Recortar* tiene controles de enderezar y de inclinación vertical y horizontal para la perspectiva.

### Cuándo usarlo y cuándo no
- **Lente**: casi siempre en RAW de cámara con lentes intercambiables, sobre todo grandes angulares y zooms de kit.
- **Perspectiva**: arquitectura, interiores, producto y cualquier foto con líneas verticales evidentes.
- **No corrijas** cuando la perspectiva es la intención (contrapicado dramático) o cuando la distorsión de un ojo de pez es parte del look.

### Errores comunes
1. **Corregir el 100 % de la convergencia vertical**: a veces se ve antinatural; dejar un 10–20 % suele verse más creíble.
2. **No dejar margen** al disparar: la corrección recorta cabezas o techos.
3. **Duplicar correcciones**: si tu revelador aplica el perfil y además tú usas otro módulo de viñeteo, las esquinas quedan claras.
4. **Olvidar la aberración cromática** en ramas contra el cielo: activa la corrección de AC de la lente o el módulo de aberraciones cromáticas.

### Para profundizar
- [darktable: lens correction](https://docs.darktable.org/usermanual/development/en/module-reference/processing-modules/lens-correction/)
- [darktable 4.6: rotate and perspective](https://docs.darktable.org/usermanual/4.6/en/module-reference/processing-modules/rotate-perspective/)
- [[Fotografía de arquitectura]] y [[Recorte y enderezado]].

## Autoevaluación

Contesta primero en voz alta o en papel; luego despliega la respuesta.

> [!question]- ¿Qué corrige la corrección de lente?
> Distorsión (líneas curvas), viñeteo (esquinas oscuras) y aberración cromática (bordes de color).

> [!question]- ¿Qué módulo usa darktable y de dónde saca los datos?
> **lens correction**, con perfiles de la base de datos libre Lensfun (o metadatos de algunos archivos). Para la a6700, revisa que tu lente esté en la lista (Pendiente de confirmar).

> [!question]- ¿Cómo enderezas edificios que se ven inclinados hacia atrás?
> Con **rotate and perspective** (darktable): marca dos líneas verticales y deja que corrija. Luego recorta los bordes vacíos.

## Práctica

- [[Práctica Edición 04 - Revelado completo paso a paso]]

