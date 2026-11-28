---
proyecto: Conocimiento
tipo: laboratorio
nivel: intermedio
progreso: pendiente
estado: borrador
area: Equipo
herramienta: Sony a6700, tarjeta SD V60/V90, DaVinci Resolve (gratuito)
tags: [equipo, laboratorio]
contenido: completo
---
# Lab 3 - Primer día con la Sony a6700 (perfiles, LUT y tarjetas)

Parte de: [[34-Equipo/02 Próximo equipo/_Índice Próximo equipo|Próximo equipo]]

**Cubre:** [[Sony a6700]] · [[Sony S-Cinetone y S-Log3]] · [[Lentes para la Sony a6700]]

> [!info] Al terminar
> Si todos los criterios quedan marcados, cambia a `dominado` el `progreso` de este laboratorio y de las notas que cubre. Guarda tus archivos de práctica en esta misma carpeta.

## Objetivo

Dejar la a6700 configurada para video y comparar S-Cinetone contra S-Log3 con LUT en la misma escena. Hazlo cuando tengas la cámara.

## Pasos

1. Actualiza el firmware desde la página de soporte de Sony y formatea la tarjeta en la cámara.
2. Configura: XAVC S 4K 23.98p o 29.97p 10 bits 4:2:2, obturación 1/50 o 1/60, AF-C con detección de rostro/ojos, zebras, audio por la entrada de 3.5 mm con el receptor del Mic Mini 2.
3. Crea dos memorias (MR) o ajustes personalizados: **S-Cinetone** y **S-Log3**.
4. Copia una LUT de S-Log3 a Rec.709 (.cube) a `PRIVATE/SONY/PRO/LUT` de la tarjeta e impórtala ([soporte Sony](https://www.sony.co.in/electronics/support/e-mount-body-ilce-6000-series/articles/00283315)). Actívala solo para monitoreo.
5. Graba la misma escena con contraste (ventana + interior) en ambos perfiles; en S-Log3 sobreexpón ~+1 paso.
6. Prueba 4K120: anota cómo cambia el encuadre por el recorte.
7. En Resolve corrige el clip S-Log3 (Color Space Transform o LUT) y compáralo con el S-Cinetone sin tocar.

## Criterios de terminado

- [ ] La tarjeta no dio errores grabando 4K 10 bits (anoté su clase V).
- [ ] Tengo dos ajustes guardados (S-Cinetone y S-Log3) que recupero en un toque.
- [ ] La LUT aparece en la lista de LUTs de usuario y se ve en pantalla al grabar en S-Log3.
- [ ] Anoté el cambio de encuadre en 4K120.
- [ ] Escribí cuál perfil elegiría para 3 situaciones distintas.

## Solución

> [!success]- Qué deberías notar
> S-Cinetone sale bien sin trabajo; S-Log3 corregido recupera más la ventana pero requiere exposición cuidadosa y tiempo de edición. Con poca luz S-Log3 mete más ruido si lo subexpones.

> [!success]- Si la LUT no carga
> Revisa la ruta exacta de carpetas, que el archivo sea `.cube` y su tamaño de malla compatible; consulta la guía de ayuda de la a6700.

## Notas personales

_Qué me costó, errores que cometí, dudas._
