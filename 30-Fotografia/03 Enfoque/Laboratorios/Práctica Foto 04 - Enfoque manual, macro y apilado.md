---
proyecto: Conocimiento
tipo: laboratorio
nivel: intermedio
progreso: pendiente
estado: borrador
area: Fotografía
herramienta: iPhone 15 Pro (macro con ultra gran angular) + cámara Profesional de Lightroom móvil para enfoque manual; Hugin (gratis) para apilar
tags: [fotografia, laboratorio]
contenido: completo
---
# Práctica Foto 04 - Enfoque manual, macro y apilado

Parte de: [[30-Fotografia/03 Enfoque/_Índice Enfoque|Enfoque]]

**Cubre:** [[Enfoque manual y focus peaking]] · [[Focus stacking (apilado de enfoque)]] · [[Modos de enfoque automático (AF-S, AF-C y seguimiento)]] · [[Fotografía macro]]

> [!info] Al terminar
> Si todos los criterios quedan marcados, cambia a `dominado` el `progreso` de este laboratorio y de las notas que cubre. Guarda tus archivos de práctica en esta misma carpeta.

## Objetivo

Fotografiar algo pequeño (flor, moneda, insecto quieto) muy de cerca, enfocar a mano y combinar varias fotos para que todo quede nítido.

## Pasos

1. En Ajustes → Cámara activa **Control de macro**. Acércate a 3–5 cm de una flor o moneda; verifica que aparezca el ícono de flor.
2. Toma una foto con enfoque automático y otra con bloqueo AE/AF en un punto diferente; compara qué parte queda nítida.
3. En la cámara Profesional de Lightroom móvil, toca el control de enfoque (Focus) y muévelo a mano. Amplía la vista y observa qué parte del objeto se vuelve nítida al mover el deslizador. (Blackmagic Camera tiene peaking, pero solo graba video; no sirve para estas fotos.)
4. Apoya el teléfono (sin tocarlo durante la serie si puedes, o con disparador de audífonos o temporizador). Toma 5–8 fotos moviendo el enfoque poco a poco del frente al fondo del objeto.
5. Pasa las fotos a la PC. Con Hugin instalado, alinea y combina con `align_image_stack` y `enfuse` (ve la solución si te atoras).
6. Compara una foto sola contra el resultado apilado.

## Criterios de terminado

- [ ] Tengo al menos una foto macro donde se ve detalle que a simple vista no se nota.
- [ ] Tengo una serie de 5+ fotos con el enfoque recorriendo el objeto, sin que cambie el encuadre.
- [ ] El resultado apilado tiene el objeto nítido de frente a fondo, sin franjas borrosas intermedias.

## Solución

> [!success]- Comandos de Hugin para apilar
> En una terminal, dentro de la carpeta de las fotos (exportadas a TIFF o JPEG):
> ```
> align_image_stack -m -a al_ foto*.tif
> enfuse --exposure-weight=0 --saturation-weight=0 --contrast-weight=1 --hard-mask --output=apilada.tif al_*.tif
> ```
> `-m` corrige el cambio de tamaño entre fotos al mover el enfoque.

> [!success]- Si salen franjas borrosas
> Los saltos de enfoque fueron demasiado grandes: toma más fotos con pasos más pequeños. Si hay "fantasmas", algo se movió (viento): protege el sujeto.

> [!success]- Por qué el macro del iPhone es con el ultra gran angular
> Esa cámara es la que enfoca más cerca; el iPhone cambia a ella automáticamente y recorta para que el encuadre se vea como 1x.

## Notas personales

_Qué me costó, errores que cometí, dudas._
