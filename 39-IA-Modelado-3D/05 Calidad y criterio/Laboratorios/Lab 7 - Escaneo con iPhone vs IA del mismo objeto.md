---
proyecto: Conocimiento
tipo: laboratorio
nivel: intermedio
progreso: pendiente
estado: borrador
area: IA para modelado 3D
herramienta: iPhone 15 Pro (Reality Composer con Object Capture o Scaniverse), TripoSR o Hunyuan3D-2mini, Blender, calibrador o regla
tags: [ia-modelado-3d, laboratorio]
contenido: completo
---
# Lab 7 - Escaneo con iPhone vs IA del mismo objeto

Parte de: [[39-IA-Modelado-3D/05 Calidad y criterio/_Índice Calidad y criterio|Calidad y criterio]]

**Cubre:** [[IA vs CAD vs escaneo (cuándo usar cada uno)]] · [[Qué imágenes funcionan mejor para IA 3D]] · [[Errores típicos de los modelos generados por IA]]

> [!info] Al terminar
> Si todos los criterios quedan marcados, cambia a `dominado` el `progreso` de este laboratorio y de las notas que cubre. Guarda tus archivos de práctica en esta misma carpeta.

## Objetivo

Obtener el mismo objeto real por escaneo y por IA desde una foto, medir diferencias y decidir cuándo conviene cada técnica.

## Pasos

1. Elige un objeto mate de 8–15 cm con detalle y una parte trasera distinta del frente (una figura, una piña de pino, un zapato pequeño). Mide 3 dimensiones con calibrador.
2. Escanéalo con el iPhone como en [[Fotogrametría y escaneo con iPhone]] y límpialo según [[Limpiar un escaneo y hacerlo imprimible]].
3. Toma una foto de tres cuartos del mismo objeto, quita el fondo y genera el 3D con TripoSR o Hunyuan3D-2mini.
4. En Blender escala ambos con la **misma** dimensión de referencia (la altura real).
5. Mide las otras 2 dimensiones en cada modelo y calcula el error porcentual contra el objeto.
6. Compara visualmente frente, lados y parte trasera; anota tiempo total de cada flujo (captura + limpieza).

## Criterios de terminado

- [ ] Tengo una tabla con las 3 medidas reales, del escaneo y de la IA, con error en %.
- [ ] Tengo capturas de la parte trasera de ambos modelos.
- [ ] Escribí una regla personal: cuándo escaneo, cuándo uso IA y cuándo CAD.

## Solución

> [!success]- Resultado esperado
> El escaneo suele quedar a pocos puntos porcentuales de las medidas reales y copia la parte trasera; la IA acierta la silueta del frente pero sus proporciones de fondo y la parte de atrás son inventadas, con errores bastante mayores. La IA gana cuando el objeto no existe o no se puede escanear.

> [!success]- Cómo calcular el error
> error % = |medida del modelo − medida real| / medida real × 100. Ejemplo: real 52 mm, modelo 47 mm → 5 / 52 × 100 ≈ 9.6 %.

## Notas personales

_Qué me costó, errores que cometí, dudas._
