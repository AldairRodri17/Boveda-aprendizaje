---
proyecto: Conocimiento
tipo: concepto
nivel: basico
progreso: pendiente
estado: pendiente
area: Edición de fotografía
tags: [edicion-foto]
contenido: completo
---
# Tamaños para web y redes sociales

Parte de: [[32-Edicion-Foto/08 Exportación/_Índice Exportación|Exportación]]

> [!info] Cómo avanzar
> `progreso`: **pendiente** → **viendo** (estudiando) → **visto** (contestaste la autoevaluación sin abrir las respuestas) → **dominado** (hiciste el laboratorio o lo aplicaste de verdad).

## Contenido

### Qué es
Cada plataforma muestra las fotos en ciertas **dimensiones y proporciones**, y recomprime lo que subes. Exportar al tamaño y proporción correctos te da control sobre el recorte, la nitidez y el color, en lugar de dejar que la plataforma decida. Los números de referencia más importantes son los de Instagram, porque además dictan el formato vertical que domina las redes.

### Cómo funciona
**Instagram** muestra las fotos del *feed* a **1080 px de ancho**. Proporciones de referencia (según guías de terceros actualizadas a 2026; Pendiente de confirmar en la ayuda oficial de Instagram):
- **Vertical 4:5 → 1080 × 1350 px**: la opción segura para publicaciones verticales y la máxima que aceptan las herramientas de programación y los anuncios.
- **Vertical 3:4 → 1080 × 1440 px**: desde 2025 la app conserva sin recortar las fotos 3:4 publicadas desde la propia app; la cuadrícula del perfil muestra miniaturas verticales.
- **Cuadrada 1:1 → 1080 × 1080 px**.
- **Horizontal → 1080 × 566 px** (1.91:1) como máximo de ancho relativo.
- **Historias y Reels 9:16 → 1080 × 1920 px**. Deja libres de texto importante aproximadamente el 14 % superior, el 35 % inferior y un margen lateral, donde la interfaz pone botones, nombre y descripción.

**Fórmula**: `alto = ancho × (proporción_alto / proporción_ancho)`.
```python
for nombre, (pw, ph) in {"4:5": (4, 5), "3:4": (3, 4), "3:2 horiz": (3, 2), "9:16": (9, 16)}.items():
    print(nombre, 1080, "x", round(1080 * ph / pw))
# 4:5 1080 x 1350 · 3:4 1080 x 1440 · 3:2 horiz 1080 x 720 · 9:16 1080 x 1920
```

**Mostrar una foto 3:2 horizontal como 4:5 sin recortar**: crea un lienzo de 1080 × 1350, coloca la foto a 1080 × 720 centrada y deja bordes (blancos, negros o de color) arriba y abajo. En darktable puedes hacerlo con el módulo **framing** (encuadre) con proporción 4:5; en GIMP, `Imagen > Tamaño del lienzo`.

**Web propia**: 2000–2560 px de lado largo es suficiente para pantallas grandes; usa AVIF o WebP con JPEG de respaldo (ver [[Formatos de exportación de imagen (JPEG, PNG, TIFF, HEIC, WebP y AVIF)]]).

### Ejemplo
Ajuste de exportación en darktable para Instagram vertical:
- Formato JPEG, calidad 90, perfil **sRGB**.
- *Establecer tamaño*: en píxeles, 1080 × 1350 (o ancho 1080 y alto 0 si ya recortaste a 4:5).
- *Permitir ampliación*: no.
- Nitidez de salida: la que aplica el módulo de exportación por defecto, o añade una instancia de nitidez suave.
- Guárdalo como **preset de exportación** "Instagram 4:5" para usarlo siempre.

Con la a6700 en vertical (4128 × 6192, 3:2), recorta primero a 4:5 (4128 × 5160) y luego exporta reduciendo a 1080 × 1350.

### Por qué no subir el archivo de 48 MP directo
La foto de 48 MP del iPhone (8064 × 6048) se mostrará a 1080 px de ancho: la plataforma la reduce con su propio algoritmo y la recomprime fuerte, con lo que puedes perder nitidez, aparecer artefactos y cambiar el color si no está en sRGB. Además, tarda en subir y gasta datos. Exportar tú al tamaño exacto, en sRGB y con nitidez de salida te da control. Desde el iPhone, la app de Instagram ya hace esa reducción al subir; desde la PC conviene exportar.

### Cuándo usarlo y cuándo no
- Exporta a la medida para cualquier red. Para **impresión** y **archivo**, no reduzcas: conserva la resolución completa.
- Para clientes, entrega una versión web y otra en alta resolución.

### Errores comunes
1. **Subir horizontales 3:2** sin pensar: se ven pequeñas en el feed vertical.
2. **Texto en las zonas que tapa la interfaz** en historias.
3. **Exportar en Adobe RGB o P3** para redes: colores apagados.
4. **Ampliar** fotos pequeñas a 1080: se ven blandas.

### Para profundizar
- [Instagram: resolución de las fotos que compartes](https://help.instagram.com/1631821640426723/)
- [Instagram: tamaño y proporción de reels](https://help.instagram.com/1038071743007909)
- [Sked Social: guía de tamaños de Instagram 2026](https://skedsocial.com/blog/best-instagram-image-and-video-size-recommendations.md)
- [[Recorte y enderezado]].

## Ejercicios

Resuélvelos en papel o en la herramienta; la solución está plegada.

> [!question]- Exporta una foto vertical 4:5 a 1080 de ancho. ¿Qué alto pones?
> 1080 × 5/4 = **1350 px**.

> [!question]- Una foto de 3:2 horizontal se publica a 1080 de ancho. ¿Qué alto tendrá sin recortar?
> 1080 × 2/3 = **720 px**.

> [!question]- Quieres mostrar una foto 3:2 horizontal como 4:5 sin recortar. ¿Qué haces?
> Agregar bordes (marco) arriba y abajo: lienzo de 1080 × 1350 con la foto a 1080 × 720 centrada.

## Autoevaluación

Contesta primero en voz alta o en papel; luego despliega la respuesta.

> [!question]- ¿Qué tamaño y proporción es seguro para una foto vertical en Instagram?
> **1080 × 1350 px (4:5)**. Instagram también ha aceptado formato 3:4 (1080 × 1440) en su cuadrícula reciente (Pendiente de confirmar).

> [!question]- ¿Y para historias o Reels?
> **1080 × 1920 px (9:16)**; deja márgenes arriba y abajo libres de texto importante.

> [!question]- ¿Por qué no subir el archivo de 48 MP directo?
> La plataforma lo recomprime a su manera y puede verse peor; exportar al tamaño exacto en sRGB te da más control.

## Práctica

- [[Práctica Edición 11 - Exportar para Instagram e impresión]]

