---
proyecto: Conocimiento
tipo: laboratorio
nivel: basico
progreso: pendiente
estado: borrador
area: Edición de fotografía
herramienta: darktable/Ansel (y un laboratorio de impresión de tu ciudad)
tags: [edicion-foto, laboratorio]
---
# Práctica Edición 11 - Exportar para Instagram e impresión

Parte de: [[32-Edicion-Foto/08 Exportación/_Índice Exportación|Exportación]]

**Cubre:** [[Formatos de exportación de imagen (JPEG, PNG, TIFF, HEIC, WebP y AVIF)]] · [[Tamaños para web y redes sociales]] · [[Marca de agua]] · [[Impresión fotográfica (DPI, perfiles y prueba en pantalla)]]

> [!info] Al terminar
> Si todos los criterios quedan marcados, cambia a `dominado` el `progreso` de este laboratorio y de las notas que cubre. Guarda tus archivos de práctica en esta misma carpeta.

## Objetivo

Exportar la misma foto para tres destinos (Instagram feed, historia e impresión 20 × 30 cm) con los ajustes correctos e imprimirla.

## Pasos

1. Elige una foto terminada.
2. Recorte 4:5 → exporta JPEG sRGB a **1080 × 1350 px**, calidad ~90, con marca de agua pequeña.
3. Recorte 9:16 → exporta **1080 × 1920 px** para historia.
4. Para impresión 20 × 30 cm: calcula los píxeles a 300 ppp y verifica que tu foto los tiene.
5. Pregunta al laboratorio qué perfil o espacio de color piden. Si tienen perfil ICC, actívalo en la **prueba en pantalla** de darktable y corrige.
6. Exporta en JPEG máxima calidad o TIFF según pida el laboratorio, sin marca de agua.
7. Imprime y compara la impresión con la pantalla.

## Criterios de terminado

- [ ] El archivo de feed mide exactamente 1080 × 1350 px y está en sRGB (revisado en las propiedades del archivo).
- [ ] El archivo de historia mide 1080 × 1920 px.
- [ ] El archivo de impresión tiene al menos ~2362 × 3543 px (20 × 30 cm a 300 ppp).
- [ ] Tengo la impresión y anoté la diferencia con la pantalla (más oscura, más cálida, etc.).

## Solución

> [!success]- Cálculo de impresión
> 20 cm / 2.54 × 300 ≈ 2362 px; 30 cm / 2.54 × 300 ≈ 3543 px.

> [!success]- Si la impresión salió oscura
> El monitor estaba muy brillante. Bájalo y, para la próxima impresión, aclara un poco la foto o usa la prueba en pantalla con el perfil del laboratorio.

> [!success]- Marca de agua en darktable
> Módulo watermark con un SVG sencillo; tamaño ~3–5 % del lado y opacidad 40–60 %. Guárdalo en un estilo "Redes".

## Notas personales

_Qué me costó, errores que cometí, dudas._
