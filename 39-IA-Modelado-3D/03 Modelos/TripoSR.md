---
proyecto: Conocimiento
tipo: concepto
nivel: intermedio
progreso: pendiente
estado: pendiente
area: IA para modelado 3D
tags: [ia-modelado-3d]
---
# TripoSR

Parte de: [[39-IA-Modelado-3D/03 Modelos/_Índice Modelos|Modelos]]

> [!info] Cómo avanzar
> `progreso`: **pendiente** → **viendo** (estudiando) → **visto** (contestaste la autoevaluación sin abrir las respuestas) → **dominado** (hiciste el laboratorio o lo aplicaste de verdad).

## Contenido

### Qué es
**TripoSR** es un modelo de reconstrucción rápida de **imagen a 3D** publicado por Tripo AI (VAST) y Stability AI. A partir de una sola imagen genera una malla en segundos, con color por vértice o textura horneada. Su licencia es **MIT** para código, modelo preentrenado y demo, así que puedes usarlo, modificarlo y vender lo que imprimas. Según su README, usa unos **6 GB de VRAM** por imagen con las opciones por defecto: justo lo que tiene tu GTX 1660 Ti, así que es tu modelo de entrada.

### Cómo funciona
1. **Preprocesado:** quita el fondo con `rembg` y reescala el objeto para que ocupe el 85 % del cuadro (`--foreground-ratio 0.85`).
2. **Codificación:** un transformer de visión convierte la imagen en tokens.
3. **Triplano:** otro transformer predice en una sola pasada tres planos de características que funcionan como un campo de radiancia ([[NeRF (campos de radiancia neuronal)]]).
4. **Malla:** se evalúa la densidad en una rejilla de `--mc-resolution` (256 por defecto) y se aplica marching cubes con `torchmcubes`.
5. **Salida:** OBJ (por defecto) o GLB, con color por vértice o, con `--bake-texture`, una textura.

Si no encuentra CUDA, `run.py` cambia solo a CPU: funciona, pero mucho más lento.

### Opciones de `run.py` que más usarás
| Opción | Por defecto | Para qué |
|---|---|---|
| `--output-dir` | `output/` | Carpeta de salida (una subcarpeta por imagen) |
| `--model-save-format` | `obj` | `obj` o `glb` |
| `--bake-texture` | apagado | Textura en vez de color por vértice |
| `--mc-resolution` | 256 | Detalle de la malla; más = más VRAM y tiempo |
| `--chunk-size` | 8192 | Menor = menos VRAM, más lento |
| `--no-remove-bg` | apagado | Si ya quitaste el fondo (espera fondo gris) |
| `--foreground-ratio` | 0.85 | Tamaño del objeto en el cuadro |
| `--render` | apagado | Guarda un video girando |

### Ejemplo: instalación y primera malla en Windows
```powershell
cd C:\IA3D
git clone https://github.com/VAST-AI-Research/TripoSR.git
cd TripoSR
py -3.10 -m venv .venv; .venv\Scripts\Activate.ps1
# PyTorch con CUDA primero (ver nota de Python, CUDA y PyTorch)
pip install torch torchvision --index-url https://download.pytorch.org/whl/cu126
pip install --upgrade setuptools
pip install -r requirements.txt

# Una foto propia, salida GLB, cuidando memoria
python run.py C:\fotos\alcancia.jpg --output-dir out\ --model-save-format glb --chunk-size 4096
```
Mide con `nvidia-smi -l 1` en otra terminal. Luego abre `out\0\mesh.glb` en Blender.

### Cómo obtienes un STL
TripoSR no exporta STL. Importa el GLB u OBJ en Blender, límpialo, escálalo a milímetros y exporta **STL** (Blender 4.2+ lo exporta de forma nativa) o **3MF** con la extensión correspondiente. Ver [[Limpiar un modelo generado por IA en Blender]].

### El error de `torchmcubes`
`torchmcubes was not compiled with CUDA support` aparece cuando la extensión se compiló sin CUDA. Según el README:
1. Verifica que la **versión mayor** del CUDA Toolkit local (`nvcc --version`) coincida con la de PyTorch (`torch.version.cuda`).
2. Asegúrate de tener `setuptools>=49.6.0`.
3. En Windows, ten **Visual Studio Build Tools** con *Desarrollo para el escritorio con C++*.
4. Reinstala:
```powershell
pip uninstall torchmcubes
pip install git+https://github.com/tatsy/torchmcubes.git
```
Ver [[Python, CUDA y PyTorch para IA 3D en Windows]].

### Cuándo usarlo y cuándo no
- **Úsalo** para iterar rápido, probar si una imagen "funciona" y para figuras sencillas que vas a limpiar e imprimir en un color.
- **Pasa a [[Hunyuan3D-2mini]]** si necesitas formas más definidas y tienes tiempo.
- **Pasa a la nube** ([[TRELLIS]]) si necesitas mucho detalle o textura PBR.

### Errores comunes
1. **Instalar PyTorch después de `requirements.txt`.** Puede quedarse la versión CPU; instala PyTorch con CUDA primero.
2. **Usar `--no-remove-bg` con fondo blanco o de color.** Espera fondo gris; si no, deja que TripoSR quite el fondo.
3. **Subir `--mc-resolution` sin bajar `--chunk-size`.** OOM seguro en 6 GB.
4. **Imprimir la malla sin escalar.** Sale en "unidades" arbitrarias (p. ej. 1.6 de alto).

### Para profundizar
- [TripoSR en GitHub (README y licencia)](https://github.com/VAST-AI-Research/TripoSR)
- [run.py de TripoSR (todas las opciones)](https://github.com/VAST-AI-Research/TripoSR/blob/main/run.py)
- [torchmcubes](https://github.com/tatsy/torchmcubes)

## Autoevaluación

Contesta primero en voz alta o en papel; luego despliega la respuesta.

> [!question]- ¿Qué es TripoSR, qué licencia tiene y cuánta VRAM pide?
> Un modelo de reconstrucción rápida de imagen a 3D de Tripo AI y Stability AI. Licencia **MIT** (código, modelo preentrenado y demo). Según su [README](https://github.com/VAST-AI-Research/TripoSR), usa unos **6 GB de VRAM** por imagen con opciones por defecto. Si no hay CUDA, `run.py` cambia solo a CPU (más lento).

> [!question]- ¿Qué opciones de `run.py` usarás más?
> Según [run.py](https://github.com/VAST-AI-Research/TripoSR/blob/main/run.py): `--output-dir`, `--model-save-format obj|glb` (por defecto `obj`), `--bake-texture` (textura en vez de color por vértice), `--mc-resolution` (por defecto 256), `--chunk-size` (por defecto 8192; menor = menos VRAM y más lento), `--no-remove-bg` (si ya quitaste el fondo y dejaste fondo gris) y `--foreground-ratio` (0.85).

> [!question]- ¿Cómo obtienes un STL si TripoSR solo exporta OBJ o GLB?
> Importa el GLB/OBJ en Blender, límpialo y escálalo, y exporta STL (Blender 4.2+ lo exporta de forma nativa) o 3MF con la extensión. Ver [[Limpiar un modelo generado por IA en Blender]].

> [!question]- Error común: `torchmcubes was not compiled with CUDA support`. ¿Cómo se arregla?
> Según el README: verifica que la versión mayor de CUDA local y la de PyTorch coincidan y que `setuptools>=49.6.0`, y reinstala:
> ```
> pip uninstall torchmcubes
> pip install git+https://github.com/tatsy/torchmcubes.git
> ```
> En Windows necesitas además Visual Studio Build Tools con C++ y el CUDA Toolkit para compilar.

## Práctica

- [[Lab 2 - Primer modelo con TripoSR desde una foto propia]]
- [[Lab 3 - Hunyuan3D-2mini en poca VRAM vs TripoSR]]
- [[Lab 4 - Del dibujo al objeto impreso con IA 3D]]

