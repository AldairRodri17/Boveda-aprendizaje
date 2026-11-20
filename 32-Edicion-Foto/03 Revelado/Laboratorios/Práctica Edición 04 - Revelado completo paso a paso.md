---
proyecto: Conocimiento
tipo: laboratorio
nivel: basico
progreso: pendiente
estado: borrador
area: Edición de fotografía
herramienta: darktable/Ansel
tags: [edicion-foto, laboratorio]
---
# Práctica Edición 04 - Revelado completo paso a paso

Parte de: [[32-Edicion-Foto/03 Revelado/_Índice Revelado|Revelado]]

**Cubre:** [[Exposición, blancos y negros en el revelado]] · [[Curvas de tonos]] · [[HSL y gradación de color]] · [[Nitidez y reducción de ruido]] · [[Corrección de lente y perspectiva]] · [[Recorte y enderezado]]

> [!info] Al terminar
> Si todos los criterios quedan marcados, cambia a `dominado` el `progreso` de este laboratorio y de las notas que cubre. Guarda tus archivos de práctica en esta misma carpeta.

## Objetivo

Revelar tres fotos de distinto tipo (paisaje, retrato y nocturna con ISO alto) aplicando todo el flujo de revelado global.

## Pasos

1. Elige tres RAW: un paisaje con cielo, un retrato y una foto nocturna con ruido.
2. Para cada una: exposición → filmic/sigmoid → balance de blancos → curva o contraste → HSL o balance de color → ruido → nitidez → corrección de lente/perspectiva → recorte.
3. Paisaje: oscurece y satura ligeramente los azules del cielo sin teñir lo demás.
4. Retrato: ajusta naranjas para piel natural; no exageres la nitidez.
5. Nocturna: reduce primero el ruido de color con *denoise (profiled)* y luego aplica nitidez suave.
6. Si hay edificios, endereza verticales con *rotate and perspective*.
7. Guarda capturas del historial de cada foto en esta carpeta.

## Criterios de terminado

- [ ] Las tres fotos exportadas tienen horizonte o verticales rectas.
- [ ] En el paisaje, el cielo es más profundo y la piel/vegetación no cambió de tono de forma rara.
- [ ] En la nocturna, al 100 % no hay manchas de color y el detalle no se ve plástico.
- [ ] Tengo el historial de cada foto y sé explicar para qué sirvió cada paso.

## Solución

> [!success]- Errores típicos y su corrección
> Halos en el horizonte: bajaste demasiado los azules con contraste local alto, reduce intensidad. Piel naranja: baja saturación de naranjas. Ruido "de gusano" tras nitidez: reduce nitidez o aumenta el umbral.

> [!success]- Por qué ese orden
> Primero la base tonal y de color (afecta todo), luego el detalle (ruido y nitidez) y al final la geometría y recorte, que no cambian el resto de ajustes.

## Notas personales

_Qué me costó, errores que cometí, dudas._
