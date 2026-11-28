---
proyecto: Conocimiento
tipo: laboratorio
nivel: intermedio
progreso: pendiente
estado: borrador
area: Impresión 3D
herramienta: Centauri 2 Combo + ElegooSlicer/OrcaSlicer
tags: [impresion-3d, laboratorio]
contenido: completo
---
# Lab 3 - Prueba de flujo y pressure advance

Parte de: [[36-Impresion-3D/04 Calibración y calidad/_Índice Calibración y calidad|Calibración y calidad]]

**Cubre:** [[Calibración de flujo y pressure advance]] · [[Perfiles de filamento e impresora]] · [[Subextrusión y sobreextrusión en FDM]]

> [!info] Al terminar
> Si todos los criterios quedan marcados, cambia a `dominado` el `progreso` de este laboratorio y de las notas que cubre. Guarda tus archivos de práctica en esta misma carpeta.

## Objetivo

Calibrar flow ratio y pressure advance de tu PLA usando las pruebas del laminador (sin calibrador).

## Pasos

1. Usa el perfil propio de PLA del Lab 2.
2. Abre **Calibración → Flow rate** paso 1 (bloques con modificadores). Imprime.
3. Elige el bloque con la cara superior más lisa (pasa el dedo y mira con luz lateral). Calcula el nuevo flujo con la fórmula que muestra el laminador.
4. Guarda el flujo en tu perfil y, si está disponible, haz el paso 2 de ajuste fino.
5. Abre **Calibración → Pressure advance** (patrón o torre). Imprime.
6. Elige el valor con esquinas más nítidas y líneas de grosor uniforme; guárdalo en el perfil.
7. Imprime un cubo de 20 mm con el perfil final y compáralo con uno hecho antes de calibrar.

## Criterios de terminado

- [ ] Anoté el flujo inicial, el bloque elegido y el flujo final calculado.
- [ ] Anoté el valor de pressure advance elegido y la altura/línea donde lo vi.
- [ ] El cubo final tiene caras superiores sin huecos ni crestas y esquinas sin abultamientos visibles.

## Solución

> [!success]- Ejemplo de cálculo
> Flujo base 0.98 y mejor bloque −5 → 0.98 × 0.95 = 0.931. Torre PA de 0 a 0.1 con 0.002/mm y mejor altura 18 mm → PA = 0.036.

> [!success]- Si la impresora calibra sola
> Si tu firmware hace calibración automática de flujo o PA (Pendiente de confirmar), compara su valor con el tuyo y quédate con el que dé mejores resultados.

## Notas personales

_Qué me costó, errores que cometí, dudas._
