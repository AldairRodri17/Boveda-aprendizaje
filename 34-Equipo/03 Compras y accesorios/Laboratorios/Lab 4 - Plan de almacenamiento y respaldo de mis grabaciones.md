---
proyecto: Conocimiento
tipo: laboratorio
nivel: basico
progreso: pendiente
estado: borrador
area: Equipo
herramienta: Hoja de cálculo (Google Sheets o LibreOffice Calc), Python opcional
tags: [equipo, laboratorio]
---
# Lab 4 - Plan de almacenamiento y respaldo de mis grabaciones

Parte de: [[34-Equipo/03 Compras y accesorios/_Índice Compras y accesorios|Compras y accesorios]]

**Cubre:** [[Tarjetas de memoria (clases de velocidad)]] · [[SSD externo para grabar y respaldar]] · [[Qué comprar primero (guía para principiantes)]]

> [!info] Al terminar
> Si todos los criterios quedan marcados, cambia a `dominado` el `progreso` de este laboratorio y de las notas que cubre. Guarda tus archivos de práctica en esta misma carpeta.

## Objetivo

Calcular cuánto espacio y qué velocidad necesitas por equipo y diseñar tu respaldo 3-2-1 antes de comprar.

## Pasos

1. Haz una tabla con: equipo, modo de grabación, bitrate (Mb/s), MB/s (= Mb/s / 8), GB por minuto (= MB/s × 60 / 1000), clase mínima de tarjeta.
2. Llena filas para: iPhone HEVC 4K30, iPhone ProRes 422 HQ 4K30 (884 Mb/s) y 4K60 (1768 Mb/s) ([white paper ProRes](https://www.apple.com/cl/final-cut-pro/docs/Apple_ProRes.pdf)), Mini 4K (100 Mb/s), a6700 XAVC S 4K 100 Mb/s, XAVC HS 4K120 280 Mb/s y XAVC S-I 4K60 600 Mb/s.
3. Estima cuántos minutos graba cada uno en una salida típica de fin de semana.
4. Calcula el espacio total por mes y el tamaño de disco de respaldo necesario para un año.
5. Dibuja tu esquema 3-2-1: dónde viven las 3 copias, los 2 medios y la copia externa.
6. Con eso arma tu lista de compras ordenada (etapas 1–3).

## Criterios de terminado

- [ ] La tabla tiene al menos 7 filas con MB/s y GB/min correctos.
- [ ] Cada fila indica la clase mínima (U3/V30, V60, V90 o SSD ≥ 220 MB/s).
- [ ] Tengo una cifra de GB/mes y de TB/año.
- [ ] Mi esquema 3-2-1 nombra medios concretos (SSD, disco de respaldo, nube).

## Solución

> [!success]- Valores de referencia
> | Modo | Mb/s | MB/s | GB/min | Mínimo |
> |---|---|---|---|---|
> | iPhone ProRes HQ 4K30 | 884 | 110.5 | 6.6 | interno 256 GB+ o SSD |
> | iPhone ProRes HQ 4K60 | 1768 | 221 | 13.3 | SSD ≥ 220 MB/s |
> | Mini 4K 4K30 | 100 | 12.5 | 0.75 | U3/V30 |
> | a6700 XAVC S 4K | 100 | 12.5 | 0.75 | V30 |
> | a6700 XAVC HS 4K120 | 280 | 35 | 2.1 | V60 |
> | a6700 XAVC S-I 4K60 | 600 | 75 | 4.5 | V90 |

> [!success]- Script de apoyo (opcional)
> ```python
> modos = {"Mini 4K": 100, "a6700 S-I": 600, "iPhone HQ 4K60": 1768}
> for m, mbps in modos.items():
>     mbs = mbps / 8
>     print(m, round(mbs, 1), "MB/s", round(mbs * 60 / 1000, 2), "GB/min")
> ```

## Notas personales

_Qué me costó, errores que cometí, dudas._
