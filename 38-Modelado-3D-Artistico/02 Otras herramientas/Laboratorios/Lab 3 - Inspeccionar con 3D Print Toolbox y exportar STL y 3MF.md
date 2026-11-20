---
proyecto: Conocimiento
tipo: laboratorio
nivel: basico
progreso: pendiente
estado: borrador
area: Modelado 3D artístico
herramienta: Blender 4.2+ con extensiones 3D Print Toolbox y 3MF Import/Export (gratuitas) + ElegooSlicer
tags: [modelado-3d-artistico, laboratorio]
---
# Lab 3 - Inspeccionar con 3D Print Toolbox y exportar STL y 3MF

Parte de: [[38-Modelado-3D-Artistico/02 Otras herramientas/_Índice Otras herramientas|Otras herramientas]]

**Cubre:** [[Preparar un modelo de Blender para imprimir]] · [[Reparar mallas para impresión]]

> [!info] Al terminar
> Si todos los criterios quedan marcados, cambia a `dominado` el `progreso` de este laboratorio y de las notas que cubre. Guarda tus archivos de práctica en esta misma carpeta.

## Objetivo

Revisar un modelo propio (la maceta o el busto) con 3D Print Toolbox, corregir lo que salga mal y exportarlo en STL y 3MF con la escala correcta.

## Pasos

1. Instala desde *Edit → Preferences → Get Extensions* las extensiones [3D Print Toolbox](https://extensions.blender.org/add-ons/print3d-toolbox/) y [3MF Import/Export](https://extensions.blender.org/add-ons/threemf-io/) (requiere permitir acceso en línea).
2. Abre tu modelo, comprueba las unidades (mm) y aplica todas las transformaciones (`Ctrl+A → All Transforms`).
3. En la barra lateral (`N`), pestaña de 3D Print, ejecuta las comprobaciones (*Check All*) y anota cuántos problemas hay de cada tipo.
4. Edit Mode: *Select All by Trait → Non Manifold*, revisa qué marca. Aplica `M → By Distance`, `Shift+N` y tapa huecos o usa *Make Manifold*.
5. Revisa el grosor mínimo con 1.2 mm y engrosa lo que salga marcado.
6. Mide volumen y usa la escala a límites para que quepa en 100 mm de alto si es necesario.
7. Exporta a STL (*File → Export → STL*) y a 3MF. Ábrelos en ElegooSlicer y compara medidas.

## Criterios de terminado

- [ ] Las comprobaciones dan 0 aristas no manifold.
- [ ] El grosor mínimo está resuelto o justificado.
- [ ] El STL y el 3MF abren en ElegooSlicer con el tamaño en mm esperado y sin aviso de errores.
- [ ] Anoté volumen del modelo y el estimado de filamento del laminador.

## Solución

> [!success]- Qué significa cada comprobación clásica
> **Solid / Non manifold**: bordes abiertos o aristas con más de dos caras. **Intersections**: caras que atraviesan otras. **Degenerate**: caras de área cero. **Thickness**: paredes más delgadas que el valor. **Overhang**: caras que necesitarán soporte. Los nombres exactos del panel pueden variar entre versiones de la extensión.

> [!success]- Si Make Manifold no lo arregla
> Duplica el objeto y aplica Voxel Remesh con un tamaño menor al detalle más fino: la copia queda cerrada. Compara y quédate con la que conserve el detalle.

## Notas personales

_Qué me costó, errores que cometí, dudas._
