---
proyecto: Conocimiento
tipo: laboratorio
nivel: intermedio
progreso: pendiente
estado: borrador
area: IA para modelado 3D
herramienta: TripoSR (MIT), Python + PyTorch del Lab 1, Git, Blender (gratuitos)
tags: [ia-modelado-3d, laboratorio]
contenido: completo
---
# Lab 2 - Primer modelo con TripoSR desde una foto propia

Parte de: [[39-IA-Modelado-3D/02 Entorno local/_Índice Entorno local|Entorno local]]

**Cubre:** [[TripoSR]] · [[Quitar el fondo de una imagen]] · [[Interfaces Gradio de los modelos 3D]] · [[Python, CUDA y PyTorch para IA 3D en Windows]]

> [!info] Al terminar
> Si todos los criterios quedan marcados, cambia a `dominado` el `progreso` de este laboratorio y de las notas que cubre. Guarda tus archivos de práctica en esta misma carpeta.

## Objetivo

Instalar TripoSR en Windows, generar tu primer modelo desde una foto propia y obtener OBJ, GLB y STL.

## Pasos

1. En `C:\IA3D`, con el entorno activo: `git clone https://github.com/VAST-AI-Research/TripoSR.git` y entra a la carpeta `TripoSR`.
2. Según el README: `pip install --upgrade setuptools` y luego `pip install -r requirements.txt`. (PyTorch ya está del Lab 1.)
3. Toma una foto de un objeto tuyo pequeño y mate (una figura, una fruta, un juguete) con fondo liso, luz pareja y vista de tres cuartos. Guárdala como `foto.jpg`.
4. Prueba el ejemplo: `python run.py examples/chair.png --output-dir output/`.
5. Genera desde tu foto dejando que TripoSR quite el fondo: `python run.py foto.jpg --output-dir out_obj/` (OBJ con color por vértice).
6. Repite con `--model-save-format glb --output-dir out_glb/`. Mientras corre, vigila `nvidia-smi` y anota el pico de VRAM y el tiempo.
7. Opcional: quita el fondo tú mismo, deja fondo gris y usa `--no-remove-bg`; compara.
8. Prueba la interfaz web con `python gradio_app.py`.
9. Importa el GLB en Blender, escálalo a ~60 mm de alto, aplica escala y exporta STL (*File → Export → STL*).

## Criterios de terminado

- [ ] Existen `out_obj/0/mesh.obj` y `out_glb/0/mesh.glb` (la carpeta numerada puede variar) generados desde **tu** foto.
- [ ] Anoté el pico de VRAM y el tiempo de generación.
- [ ] El STL abre en ElegooSlicer con ~60 mm de alto.
- [ ] La interfaz Gradio abrió en el navegador y generó un modelo.

## Solución

> [!success]- Error: torchmcubes no compilado con CUDA
> Mensajes `module 'torchmcubes_module' has no attribute 'mcubes_cuda'` o `torchmcubes was not compiled with CUDA support`. Según el README: confirma que la versión mayor de CUDA local coincide con `torch.version.cuda` y que `setuptools>=49.6.0`; luego
> ```
> pip uninstall torchmcubes
> pip install git+https://github.com/tatsy/torchmcubes.git
> ```
> En Windows la compilación necesita Visual Studio Build Tools (C++) y el CUDA Toolkit; ejecútala desde una terminal donde `cl` y `nvcc` respondan.

> [!success]- Error: CUDA out of memory
> Cierra programas que usen la GPU y baja `--chunk-size` (por defecto 8192): `python run.py foto.jpg --chunk-size 4096 --output-dir out/`. Si sigue, prueba 2048. No subas `--mc-resolution` en 6 GB hasta que el valor por defecto funcione.

> [!success]- Si el modelo sale plano o con trozos del fondo
> La máscara del fondo quedó mal o la foto es frontal sin volumen. Vuelve a tomarla en tres cuartos, con fondo más contrastado, o quita el fondo a mano y usa `--no-remove-bg` con fondo gris.

> [!success]- Comando final de referencia
> ```
> python run.py foto.jpg --output-dir out_glb/ --model-save-format glb
> ```
> Con `--bake-texture` obtienes `texture.png` en lugar de color por vértice.

## Notas personales

_Qué me costó, errores que cometí, dudas._
