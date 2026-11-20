---
proyecto: Conocimiento
tipo: laboratorio
nivel: basico
progreso: pendiente
estado: borrador
area: Equipo
herramienta: iPhone 15 Pro, app Cámara, app Blackmagic Camera (gratuita), DaVinci Resolve (gratuito)
tags: [equipo, laboratorio]
---
# Lab 1 - Conoce tu iPhone 15 Pro (lentes, ProRAW y Blackmagic Camera a 180 grados)

Parte de: [[34-Equipo/01 Mi equipo actual/_Índice Mi equipo actual|Mi equipo actual]]

**Cubre:** [[iPhone 15 Pro para foto y video]] · [[Lentes y zoom del iPhone 15 Pro]] · [[Apple ProRAW en el iPhone 15 Pro]] · [[ProRes y Apple Log en el iPhone 15 Pro]] · [[Blackmagic Camera (app)]]

> [!info] Al terminar
> Si todos los criterios quedan marcados, cambia a `dominado` el `progreso` de este laboratorio y de las notas que cubre. Guarda tus archivos de práctica en esta misma carpeta.

## Objetivo

Saber qué lente y qué formato usar en cada caso y dejar la app Blackmagic Camera lista con obturación de 180°.

## Pasos

1. Revisa en **Ajustes > General > Información** la capacidad de tu iPhone y anota qué ProRes te permite (tabla de [Apple 109041](https://support.apple.com/en-us/109041)).
2. Activa **Ajustes > Cámara > Formatos > ProRAW y control de resolución** ([Apple 119916](https://support.apple.com/en-us/119916)).
3. Toma la **misma escena** (algo con cielo y sombra) con 0.5x, 1x, 2x y 3x, primero en HEIF y luego en RAW Max a 1x. Repite en interior con poca luz.
4. Pasa las fotos a la PC y compara: nitidez en bordes, ruido en sombras y cuánto recuperas del cielo al bajar luces en un revelador gratuito (ver [[darktable y Ansel]]).
5. Instala **Blackmagic Camera** y configura: códec HEVC (H.265), 3840×2160, **29.97 o 23.98 fps**, *Shutter* en **grados = 180°**, ISO manual, balance de blancos fijo, zebras activas y focus peaking.
6. Graba 3 clips de 20 s: (a) 180° con ISO bajo en sombra, (b) mismo clip a pleno sol, (c) con **Apple Log** y la LUT de previsualización.
7. En DaVinci Resolve aplica a (c) una conversión de Apple Log a Rec.709 y compárala con (a).
8. Escribe en *Notas personales* tu **tabla de decisión**: qué lente/formato usarás para paisaje, retrato, interior, video rápido para redes y video para colorear.

## Criterios de terminado

- [ ] Tengo anotado qué ProRes permite mi modelo (1080p30 en 128 GB o 4K30 en 256 GB o más).
- [ ] Tengo 4 fotos de la misma escena (0.5x/1x/2x/3x) y 1 RAW Max de 48 MP comparado con HEIF.
- [ ] La app Blackmagic Camera muestra 180° y, al cambiar entre 24 y 30 fps, la velocidad pasa a 1/48 y 1/60.
- [ ] El clip en Apple Log se ve gris en bruto y normal después de la conversión en Resolve.
- [ ] Escribí mi tabla de decisión con al menos 5 situaciones.

## Solución

> [!success]- Qué deberías observar en la comparación de lentes
> - **1x** gana en nitidez y ruido; con poca luz el **3x** suele verse peor (o el iPhone usa un recorte digital).
> - **0.5x** deforma los bordes y tiene más ruido en interior.
> - El RAW Max recupera más cielo y sombras pero pesa ~75 MB.

> [!success]- Por qué el clip (b) sale quemado
> Con 180° fijas la velocidad (1/48–1/60) y el iPhone no tiene diafragma: la única forma de bajar luz es ISO mínimo y, si no alcanza, un **ND**. Las zebras te avisan dónde se quema.

> [!success]- Tabla de decisión de ejemplo
> | Situación | Lente | Formato |
> |---|---|---|
> | Paisaje | 1x | RAW Max |
> | Retrato | 2x | HEIF o RAW 12 |
> | Interior oscuro | 1x | HEIF, modo Noche |
> | Video rápido para redes | 1x | HEVC 4K30 |
> | Video para colorear | 1x | ProRes Log / Apple Log |

## Notas personales

_Qué me costó, errores que cometí, dudas._
