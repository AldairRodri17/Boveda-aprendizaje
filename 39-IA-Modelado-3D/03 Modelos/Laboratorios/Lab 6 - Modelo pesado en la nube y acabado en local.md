---
proyecto: Conocimiento
tipo: laboratorio
nivel: avanzado
progreso: pendiente
estado: borrador
area: IA para modelado 3D
herramienta: Space de Hugging Face (TRELLIS o TRELLIS.2, MIT) o Google Colab gratuito, Blender, ElegooSlicer
tags: [ia-modelado-3d, laboratorio]
contenido: completo
---
# Lab 6 - Modelo pesado en la nube y acabado en local

Parte de: [[39-IA-Modelado-3D/03 Modelos/_Índice Modelos|Modelos]]

**Cubre:** [[TRELLIS]] · [[Modelos de imagen a 3D en la nube gratuita (Hugging Face y Colab)]] · [[Limpiar un modelo generado por IA en Blender]]

> [!info] Al terminar
> Si todos los criterios quedan marcados, cambia a `dominado` el `progreso` de este laboratorio y de las notas que cubre. Guarda tus archivos de práctica en esta misma carpeta.

## Objetivo

Generar un modelo con un modelo pesado que no cabe en 6 GB usando la nube gratuita y terminarlo en local hasta dejarlo listo para imprimir.

## Pasos

1. Abre el [Space de TRELLIS](https://huggingface.co/spaces/microsoft/TRELLIS) o el de [TRELLIS.2](https://huggingface.co/spaces/microsoft/TRELLIS.2) (inicia sesión en Hugging Face para tener más cuota).
2. Sube la misma imagen del Lab 3 (fondo quitado). Genera con los parámetros por defecto y luego cambia la semilla una vez.
3. Descarga el **GLB** (no el splat). Anota cuánto tardó y si tuviste que esperar cola.
4. En Blender: importa, revisa si es manifold con 3D Print Toolbox, escala a 60 mm y limpia como en el Lab 4.
5. Compara contra tus resultados de TripoSR y Hunyuan3D-2mini a la misma escala.
6. Exporta STL, lamina y, si vale la pena, imprime.

## Criterios de terminado

- [ ] Tengo el GLB del Space y el STL final a 60 mm.
- [ ] El STL pasa *Solid* e *Intersections* en 3D Print Toolbox.
- [ ] Agregué TRELLIS a mi tabla de comparación del Lab 3 (tiempo, triángulos, calificación).
- [ ] Anoté la licencia del modelo usado (MIT) y si el Space indicaba alguna condición extra.

## Solución

> [!success]- Si el Space no tiene cuota o está caído
> Espera, inicia sesión o busca otro Space oficial del mismo modelo. Como alternativa usa un cuaderno de Colab de la comunidad (revisa el código antes de ejecutarlo y no pongas tokens personales en cuadernos ajenos). Que exista un cuaderno oficial de Colab: (Pendiente de confirmar).

> [!success]- Si el GLB trae transparencia o se ve negro en Blender
> TRELLIS.2 exporta GLB en modo OPACO por defecto y con materiales PBR. Para imprimir no importa el material: revisa la malla en modo *Solid*.

> [!success]- Por qué no intentarlo en local
> TRELLIS pide ≥ 16 GB y TRELLIS.2 ≥ 24 GB oficialmente, y solo están probados en Linux. Los forks de poca VRAM son experimentales; vale la pena solo si ya dominas los labs 1–3.

## Notas personales

_Qué me costó, errores que cometí, dudas._
