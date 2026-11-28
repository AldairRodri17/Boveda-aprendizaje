---
proyecto: Conocimiento
tipo: laboratorio
nivel: intermedio
progreso: pendiente
estado: borrador
area: IA para modelado 3D
herramienta: Hunyuan3D-2mini (licencia Tencent Hunyuan 3D Community; uso permitido en México) vía WinPortable, Hunyuan3D-2GP o repo oficial; TripoSR del Lab 2
tags: [ia-modelado-3d, laboratorio]
contenido: completo
---
# Lab 3 - Hunyuan3D-2mini en poca VRAM vs TripoSR

Parte de: [[39-IA-Modelado-3D/03 Modelos/_Índice Modelos|Modelos]]

**Cubre:** [[Hunyuan3D-2mini]] · [[TripoSR]] · [[VRAM - medirla y qué hacer cuando no alcanza]] · [[Tarjetas GTX 16 y media precisión (FP16)]] · [[Cómo funciona un modelo de imagen a 3D]]

> [!info] Al terminar
> Si todos los criterios quedan marcados, cambia a `dominado` el `progreso` de este laboratorio y de las notas que cubre. Guarda tus archivos de práctica en esta misma carpeta.

## Objetivo

Generar **solo la forma** con Hunyuan3D-2mini en modo de poca VRAM y compararla con TripoSR usando las mismas 3 imágenes: calidad, tiempo y VRAM medida.

## Pasos

1. Elige 3 imágenes con fondo quitado: tu foto del Lab 2, un dibujo propio y un objeto con detalle fino.
2. Elige una ruta de instalación: **A)** [WinPortable](https://github.com/YanWenKun/Hunyuan3D-2-WinPortable) (descomprimir con 7-Zip en una ruta corta como `C:\AI\HY3D2`, `UPDATE.bat`, `RUN.bat`, sin activar textura) o **B)** [Hunyuan3D-2GP](https://github.com/deepbeepmeep/Hunyuan3D-2GP) en un entorno nuevo, o **C)** el repo oficial con `pip install -r requirements.txt` y `pip install -e .` (sin compilar los módulos de textura).
3. Con la ruta B: `python gradio_app.py --profile 4`. Con la C: `python gradio_app.py --model_path tencent/Hunyuan3D-2mini --subfolder hunyuan3d-dit-v2-mini --texgen_model_path tencent/Hunyuan3D-2 --low_vram_mode`.
4. Genera la forma de las 3 imágenes. En otra terminal deja corriendo `nvidia-smi --query-gpu=memory.used --format=csv -l 1` y anota el pico y el tiempo de cada una.
5. Genera las mismas 3 con TripoSR (Lab 2) y anota pico y tiempo.
6. Importa los 6 modelos en Blender lado a lado a la misma altura (60 mm). Califica del 1 al 5: silueta, detalle, parte trasera y limpieza de malla. Anota el número de triángulos.
7. Si te sobra tiempo, prueba la variante Turbo (`--subfolder hunyuan3d-dit-v2-mini-turbo --enable_flashvdm` en la ruta C) y compara tiempo.

## Criterios de terminado

- [ ] Tengo una tabla con 3 imágenes × 2 modelos: pico de VRAM (MiB), tiempo (s), triángulos y calificación.
- [ ] Al menos una forma de Hunyuan3D-2mini se generó en mi GTX 1660 Ti (o documenté el error exacto y lo que probé).
- [ ] Escribí una conclusión: qué modelo uso por defecto para figuras y cuál para pruebas rápidas.
- [ ] Confirmé en la licencia que mi uso (México, personal o venta pequeña) está permitido.

## Solución

> [!success]- Qué esperar
> Hunyuan3D-2mini usa difusión con muchos pasos: suele tardar más que TripoSR, pero da siluetas más definidas y mejores partes traseras. TripoSR es más rápido y predecible. Tus mediciones mandan: no hay cifras oficiales para la GTX 1660 Ti.

> [!success]- Si sale CUDA out of memory
> Ruta C: confirma que usas `--low_vram_mode`. Ruta B: prueba `--profile 5`. En scripts, baja `octree_resolution` (p. ej. de 380 a 256) o `num_chunks`. Cierra el navegador con otras pestañas pesadas (la interfaz web también usa algo de GPU si tiene aceleración).

> [!success]- Si sale una malla vacía, negra o con NaN
> Puede ser el problema de FP16 de las GTX 16 ([[Tarjetas GTX 16 y media precisión (FP16)]]). Prueba otra ruta (WinPortable usa optimizaciones mmgp) o un modo FP32 si el código lo permite. Estado para la 1660 Ti: (Pendiente de confirmar).

> [!success]- Si falla `python3 setup.py install` de los módulos de textura
> No los necesitas para este lab (solo forma). Si quieres textura más adelante, Hunyuan3D-2GP indica en Windows tener Visual Studio 2022 o más nuevo y, si falla, ejecutar `VsDevCmd` con `-arch=x64` para fijar el compilador; WinPortable pide CUDA Toolkit y Build Tools con C++.

> [!success]- Script de solo forma (ruta C), según la tarjeta del modelo
> ```python
> import time, torch
> from hy3dgen.shapegen import Hunyuan3DDiTFlowMatchingPipeline
> pipe = Hunyuan3DDiTFlowMatchingPipeline.from_pretrained(
>     'tencent/Hunyuan3D-2mini', subfolder='hunyuan3d-dit-v2-mini',
>     use_safetensors=True, device='cuda')
> t = time.time()
> mesh = pipe(image='foto_sin_fondo.png', num_inference_steps=30,
>             octree_resolution=380, num_chunks=20000,
>             generator=torch.manual_seed(12345), output_type='trimesh')[0]
> print('s:', time.time() - t, 'MiB:', torch.cuda.max_memory_allocated() / 2**20)
> mesh.export('hunyuan_mini.glb')
> ```
> Si no cabe, baja `octree_resolution` o `num_chunks`.

## Notas personales

_Qué me costó, errores que cometí, dudas._
