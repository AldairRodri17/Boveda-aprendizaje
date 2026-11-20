---
proyecto: Conocimiento
tipo: laboratorio
nivel: avanzado
progreso: pendiente
estado: borrador
area: Edición de fotografía
herramienta: GIMP (y módulo retouch de darktable para la limpieza)
tags: [edicion-foto, laboratorio]
---
# Práctica Edición 06 - Limpieza y retoque de piel

Parte de: [[32-Edicion-Foto/04 Ajustes locales y retoque/_Índice Ajustes locales y retoque|Ajustes locales y retoque]]

**Cubre:** [[Clonar y parchar]] · [[Retoque de piel con separación de frecuencias]] · [[Ética del retoque]]

> [!info] Al terminar
> Si todos los criterios quedan marcados, cambia a `dominado` el `progreso` de este laboratorio y de las notas que cubre. Guarda tus archivos de práctica en esta misma carpeta.

## Objetivo

Limpiar una foto (manchas del sensor, basura, cables) y hacer un retoque de piel natural con separación de frecuencias. Usa una foto tuya o de alguien que te dé permiso.

## Pasos

1. En darktable, usa el módulo **retouch** para quitar 3 elementos pequeños (basura, mancha, cable) con *corregir* o *clonar*.
2. Exporta la foto de retrato en TIFF de 16 bits y ábrela en GIMP.
3. Haz la separación de frecuencias (manual o con Descomposición wavelet).
4. En la capa de **baja frecuencia**, empareja manchas de tono con un pincel suave o desenfoque local.
5. En la capa de **alta frecuencia**, quita granitos pasajeros con la herramienta Saneado, conservando poros.
6. Compara al 100 % antes y después; pide su opinión a la persona fotografiada.

## Criterios de terminado

- [ ] Los 3 elementos quitados no dejan patrones repetidos visibles.
- [ ] La piel retocada conserva textura (poros visibles al 100 %) y no hay manchas de color.
- [ ] La persona fotografiada aprobó el retoque (o anoté qué quería distinto).

## Solución

> [!success]- Separación manual en GIMP (resumen)
> 1) Duplica la capa dos veces: "Baja" (abajo) y "Alta" (arriba). 2) Desenfoque gaussiano en "Baja" hasta que desaparezcan los poros. 3) Pon "Alta" en modo **Extracción de granulado**, oculta el original y usa **Capa → Nueva desde lo visible**. 4) Pon esa capa nueva en modo **Combinación de granulado** y elimina la "Alta" anterior. Con "Baja" debajo, la imagen debe verse idéntica al original. Atajo: Filtros → Mejorar → Descomposición wavelet.

> [!success]- Señales de exceso
> Piel uniforme como plástico, cejas o pestañas borradas, cambio de forma de la cara. Regresa y reduce.

## Notas personales

_Qué me costó, errores que cometí, dudas._
