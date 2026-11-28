---
proyecto: Conocimiento
tipo: laboratorio
nivel: basico
progreso: pendiente
estado: borrador
area: Edición de fotografía
herramienta: darktable/Ansel + un ProRAW del iPhone 15 Pro
tags: [edicion-foto, laboratorio]
contenido: completo
---
# Práctica Edición 02 - Primer revelado de un ProRAW en darktable

Parte de: [[32-Edicion-Foto/02 Programas gratuitos/_Índice Programas gratuitos|Programas gratuitos]]

**Cubre:** [[darktable y Ansel]] · [[Revelado RAW vs retoque]] · [[Edición no destructiva]] · [[Exposición, blancos y negros en el revelado]]

> [!info] Al terminar
> Si todos los criterios quedan marcados, cambia a `dominado` el `progreso` de este laboratorio y de las notas que cubre. Guarda tus archivos de práctica en esta misma carpeta.

## Objetivo

Revelar de principio a fin un ProRAW con los módulos esenciales y comparar con el HEIF que hizo el iPhone de la misma escena.

## Pasos

1. Toma la misma escena dos veces: una en ProRAW y otra en HEIF (ver [[Práctica Foto 01 - Las tres cámaras del iPhone y los formatos]]).
2. Pásalas a la PC con originales. darktable no abre los ProRAW de Apple tal cual (usan una compresión que no soporta). Antes de importar, elige una vía:
   - **Convertir**: abre el DNG en el **Adobe DNG Converter** (gratuito), en Preferencias elige una compatibilidad anterior a DNG 1.7 y convierte; importa en darktable el DNG convertido.
   - **Otra app al disparar**: toma la foto con una app que guarde **DNG estándar** (p. ej. la cámara Profesional de [[Lightroom móvil (versión gratuita)]] en DNG) y usa ese archivo.
   - **Otro revelador**: si nada de lo anterior funciona, haz esta práctica en [[RawTherapee]] con los módulos equivalentes.
   Importa el archivo elegido en darktable y comprueba que abra en el cuarto oscuro.
3. En el cuarto oscuro, crea un grupo de módulos con: exposición, filmic rgb o sigmoid, calibración de color, balance de color rgb, ecualizador de tonos, recortar, rotar y perspectiva.
4. Ajusta **exposición** hasta que los medios tonos se vean bien.
5. Ajusta **filmic** o **sigmoid** para recuperar luces y sombras.
6. Corrige el balance de blancos en **calibración de color**.
7. Da un toque de saturación o vibrancia en **balance de color rgb**.
8. Endereza y recorta. Exporta en sRGB.
9. Compara lado a lado con el HEIF original. Revisa el historial y regresa a un paso anterior para ver que nada se perdió.

## Criterios de terminado

- [ ] El DNG (convertido o estándar) abre en el cuarto oscuro de darktable sin error.
- [ ] Tengo una exportación del ProRAW revelado y la comparación con el HEIF.
- [ ] En el histograma de mi versión no hay altas luces importantes recortadas.
- [ ] Puedo volver al estado original desde el historial y regresar a mi edición.
- [ ] Escribí en notas personales 3 diferencias entre mi revelado y el del iPhone.

## Solución

> [!success]- Orden sugerido y valores de inicio
> Exposición: empieza con +0.5 a +1 EV si la ves oscura. Filmic/sigmoid: ajusta el contraste suave al principio. Calibración de color: usa el selector para tomar un área neutra (pared gris o blanca).

> [!success]- Si se ve deslavado
> Es normal al principio: el RAW no trae el contraste y color del procesado del iPhone. Sube contraste en filmic/sigmoid y vibrancia en balance de color rgb.

> [!success]- Si tu versión es Ansel
> Los nombres de algunos módulos y la organización de pestañas cambian, pero los módulos clave (exposición, filmic, calibración de color, balance de color rgb) existen.

## Notas personales

_Qué me costó, errores que cometí, dudas._
