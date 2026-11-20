---
proyecto: Conocimiento
tipo: laboratorio
nivel: intermedio
progreso: pendiente
estado: borrador
area: Modelado 3D artístico
herramienta: iPhone 15 Pro con Reality Composer (Object Capture) o Scaniverse (gratuitos), Blender, Elegoo Centauri 2 Combo
tags: [modelado-3d-artistico, laboratorio]
---
# Lab 7 - Escanear un objeto con el iPhone y dejarlo imprimible

Parte de: [[38-Modelado-3D-Artistico/04 Escaneo 3D/_Índice Escaneo 3D|Escaneo 3D]]

**Cubre:** [[Fotogrametría y escaneo con iPhone]] · [[Limpiar un escaneo y hacerlo imprimible]] · [[Reparar mallas para impresión]]

> [!info] Al terminar
> Si todos los criterios quedan marcados, cambia a `dominado` el `progreso` de este laboratorio y de las notas que cubre. Guarda tus archivos de práctica en esta misma carpeta.

## Objetivo

Escanear un objeto propio de unos 10–20 cm, limpiarlo en Blender y obtener un modelo imprimible a escala.

## Pasos

1. Elige un objeto mate y con textura (una piedra, una fruta, una escultura tuya, un zapato). Mide con regla una dimensión de referencia.
2. Colócalo sobre una mesa con fondo con textura y luz difusa. Escanéalo con Object Capture en Reality Composer (o con Scaniverse) dando 2–3 vueltas a distintas alturas.
3. Exporta (USDZ desde Reality Composer; GLB/FBX/PLY desde Scaniverse) y pásalo a la PC.
4. En Blender importa el archivo, orienta el objeto y borra el piso y los restos.
5. Escala con la medida de referencia.
6. Aplica Voxel Remesh (prueba 0.5 y 1 mm) para cerrarlo; corta una base plana con Boolean o *Bisect* y rellena.
7. Decimate si supera ~500 mil triángulos, revisa con 3D Print Toolbox y exporta STL.
8. Imprime a 50 % para validar.

## Criterios de terminado

- [ ] El modelo es manifold y tiene base plana.
- [ ] La medida de referencia del modelo coincide con la real (± 2 %).
- [ ] La impresión de prueba es reconocible y no necesitó reparación en el laminador.
- [ ] Escribí qué falló del escaneo (zonas brillantes, huecos) y cómo lo resolví.

## Solución

> [!success]- Si el escaneo sale con huecos
> Faltaron fotos en esa zona (abajo, dentro de concavidades). Repite con una vuelta más baja o voltea el objeto y escanea otra pasada si la app lo permite. En Blender, Voxel Remesh cierra huecos pequeños.

> [!success]- Por qué Voxel Remesh y no reparar a mano
> Los escaneos traen miles de defectos pequeños; Remesh reconstruye una malla cerrada de una sola vez. Elige un tamaño de vóxel que conserve el detalle que te importa.

## Notas personales

_Qué me costó, errores que cometí, dudas._
