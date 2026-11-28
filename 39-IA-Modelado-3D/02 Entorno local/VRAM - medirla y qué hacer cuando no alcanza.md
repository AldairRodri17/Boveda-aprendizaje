---
proyecto: Conocimiento
tipo: concepto
nivel: intermedio
progreso: pendiente
estado: pendiente
area: IA para modelado 3D
tags: [ia-modelado-3d]
contenido: completo
---
# VRAM - medirla y qué hacer cuando no alcanza

Parte de: [[39-IA-Modelado-3D/02 Entorno local/_Índice Entorno local|Entorno local]]

> [!info] Cómo avanzar
> `progreso`: **pendiente** → **viendo** (estudiando) → **visto** (contestaste la autoevaluación sin abrir las respuestas) → **dominado** (hiciste el laboratorio o lo aplicaste de verdad).

## Contenido

### Qué es
La **VRAM** es la memoria de la tarjeta de video. Ahí deben caber los **pesos** del modelo, las **activaciones** (resultados intermedios), el contexto de CUDA y lo que ya usa Windows. Tu GTX 1660 Ti tiene 6 GB (6,144 MiB): es el límite que decide qué modelos corres en local, con qué parámetros y cuándo te conviene la nube.

### Cómo funciona: cuánto ocupa un modelo
**Pesos = parámetros × bytes por parámetro.** FP32 usa 4 bytes, FP16 2 bytes, FP8 1 byte.

| Modelo | Parámetros | FP32 | FP16 | FP8 |
|---|---|---|---|---|
| Hunyuan3D-2mini (forma) | 0.6 mil millones | ≈ 2.2 GiB | ≈ 1.1 GiB | ≈ 0.6 GiB |
| Hunyuan3D-2 (forma) | 1.1 mil millones | ≈ 4.1 GiB | ≈ 2.0 GiB | ≈ 1.0 GiB |
| TRELLIS.2 | 4 mil millones | ≈ 14.9 GiB | ≈ 7.5 GiB | ≈ 3.7 GiB |

Ejemplo: 0.6×10⁹ × 4 B = 2.4×10⁹ B ÷ 2³⁰ ≈ 2.2 GiB. Encima de los pesos van activaciones, el decodificador de forma o textura y unos cientos de MB de contexto CUDA. Por eso el **uso real** es bastante mayor que el cálculo de pesos. Con TRELLIS.2, ni los pesos en FP16 caben en 6 GB; en FP8 cabrían los pesos pero no el resto, y tu GTX 16 no acelera FP8. Conclusión: TRELLIS.2 es para la nube.

### Cómo medirla
```powershell
# En otra terminal, mientras corre el modelo: uso cada segundo
nvidia-smi --query-gpu=name,memory.used,memory.total --format=csv -l 1
```
```python
import torch, time
torch.cuda.reset_peak_memory_stats()
t0 = time.time()
# ... aquí llamas al modelo ...
print(f"Tiempo: {time.time() - t0:.1f} s")
print(f"Pico PyTorch: {torch.cuda.max_memory_allocated() / 2**20:.0f} MiB")
```
`max_memory_allocated` mide lo que reservaron los tensores de PyTorch; `nvidia-smi` siempre marca más porque incluye el contexto CUDA, la caché del asignador y otros procesos.

**Antes de generar**, mira cuánto usa ya el escritorio. Si `nvidia-smi` marca 1,100 MiB usados y tu prueba necesita un pico de 5,600 MiB, la suma es 6,700 MiB, más que 6,144: no alcanza por unos 556 MiB.

### Qué hacer cuando sale `CUDA out of memory` (de lo más fácil a lo más costoso)
1. **Cierra** navegador (usa aceleración por hardware), juegos, launchers, ComfyUI u otro modelo abierto.
2. **Baja parámetros de memoria:** `--chunk-size` en TripoSR (8192 → 4096 o 2048), `num_chunks` u `octree_resolution` en Hunyuan3D, resolución de malla.
3. **Modo de poca VRAM:** `--low_vram_mode` en Hunyuan3D, perfiles de Hunyuan3D-2GP (`--profile 4` para 6–9 GB), `--lowvram` en ComfyUI. Descargan partes del modelo a la RAM.
4. **Versión más pequeña:** mini, Turbo, solo forma sin textura.
5. **Nube:** Spaces de Hugging Face o Colab ([[Modelos de imagen a 3D en la nube gratuita (Hugging Face y Colab)]]).

### El precio del *offloading*
Descargar a CPU funciona porque los pesos viven en la RAM y solo pasan a la GPU cuando se necesitan. Cuesta **tiempo** (cada viaje por PCIe es lento) y **RAM**: el paquete WinPortable de Hunyuan3D pide al menos 24 GB de RAM y advierte que entre menos VRAM, más RAM necesitas. Revisa cuánta RAM tiene tu PC antes de elegir esa ruta (Pendiente de confirmar en tu equipo).

### Ejemplo: GPU contra CPU
Mides TripoSR en GPU: 25 s por imagen. Si en CPU tardara 20 veces más (supuesto: mide el tuyo), 12 imágenes serían 5 minutos en GPU contra 100 minutos en CPU. Por eso vale la pena pelear por que el modelo quepa en la VRAM.

### Cuándo usarlo y cuándo no
- **Mide siempre** la primera vez que corres un modelo o cambias un parámetro de resolución; anota pico y tiempo en la nota del laboratorio ([[Lab 3 - Hunyuan3D-2mini en poca VRAM vs TripoSR]]).
- **No** pelees semanas con un modelo que oficialmente pide 16–24 GB: la nube es más barata que tu tiempo.

### Errores comunes
1. **Leer "6 GB" en un README y suponer que corre en tu tarjeta de 6 GB.** Windows ya ocupa parte, y el dato suele medirse en Linux sin pantalla. Estás al límite: deja margen.
2. **Medir solo con `max_memory_allocated`.** Subestima; confirma con `nvidia-smi`.
3. **Cambiar varias cosas a la vez.** No sabrás qué causó el error. Un parámetro por prueba.
4. **Dejar un modelo cargado en otra pestaña.** Gradio y ComfyUI retienen la VRAM hasta que cierras la consola.

### Para profundizar
- [torch.cuda.max_memory_allocated (documentación de PyTorch)](https://docs.pytorch.org/docs/2.3/generated/torch.cuda.max_memory_allocated.html)
- [Hunyuan3D-2GP (perfiles de poca VRAM)](https://github.com/deepbeepmeep/Hunyuan3D-2GP)
- [Hunyuan3D-2-WinPortable](https://github.com/YanWenKun/Hunyuan3D-2-WinPortable)

## Ejercicios

Resuélvelos en papel o en la herramienta; la solución está plegada.

> [!question]- Un modelo tiene 0.6 mil millones de parámetros. ¿Cuánta VRAM ocupan solo los pesos en FP32 y en FP16?
> FP32 = 4 bytes: 0.6×10⁹ × 4 = 2.4×10⁹ B ≈ **2.2 GiB**. FP16 = 2 bytes: 1.2×10⁹ B ≈ **1.1 GiB**. A eso se suman activaciones, el decodificador de forma y el contexto de CUDA, así que el uso real es bastante mayor.

> [!question]- TRELLIS.2 tiene 4 mil millones de parámetros. ¿Caben sus pesos en tu GPU de 6 GB en FP16? ¿Y en FP8?
> FP16: 4×10⁹ × 2 B = 8×10⁹ B ≈ 7.5 GiB > 6 GB: **no caben** ni siquiera los pesos; necesitarías *offloading* a RAM. FP8: 4×10⁹ B ≈ 3.7 GiB: los pesos cabrían, pero las activaciones y el decodificador pueden rebasar la memoria. Además tu GTX 16 no acelera FP8. Conclusión: no cuentes con TRELLIS.2 en local; úsalo en la nube.

> [!question]- Antes de generar, `nvidia-smi` marca 1,100 MiB usados de 6,144 MiB. Tu prueba con TripoSR en otra PC tuvo un pico de 5,600 MiB. ¿Te alcanza? ¿Qué haces?
> 1,100 + 5,600 = 6,700 MiB > 6,144 MiB: **no alcanza** (te faltan ~556 MiB). Cierra programas que usan la GPU (navegador con aceleración por hardware, launchers) y baja `--chunk-size` (p. ej. de 8192 a 4096), que según `run.py` reduce VRAM a cambio de tiempo.

> [!question]- Mediste TripoSR en GPU: 25 s por imagen. Supón que en CPU tarda 20 veces más. ¿Cuánto tardarías con 12 imágenes en cada caso?
> GPU: 12 × 25 s = 300 s = **5 min**. CPU: 25 × 20 = 500 s por imagen; 12 × 500 = 6,000 s = **100 min (1 h 40 min)**. El factor de 20 es un supuesto del ejercicio: mide el tuyo.

## Autoevaluación

Contesta primero en voz alta o en papel; luego despliega la respuesta.

> [!question]- ¿Cómo mides la VRAM mientras corre un modelo?
> En otra terminal:
> ```
> nvidia-smi --query-gpu=name,memory.used,memory.total --format=csv -l 1
> ```
> imprime el uso cada segundo. Desde Python, al final de la generación: `torch.cuda.max_memory_allocated() / 2**30` da el pico en GiB que reservaron los tensores de PyTorch (el total real en `nvidia-smi` es algo mayor).

> [!question]- ¿Qué hacer cuando sale `CUDA out of memory`? Ordena de lo más fácil a lo más costoso.
> 1) Cierra navegador, juegos y otros programas que usen la GPU. 2) Baja parámetros de memoria del modelo (`--chunk-size` en TripoSR, `num_chunks` u `octree_resolution` en Hunyuan3D). 3) Usa el modo de poca VRAM (`--low_vram_mode`, perfiles de Hunyuan3D-2GP) que descarga partes a la RAM/CPU. 4) Usa una versión más pequeña (mini, turbo). 5) Pasa a la nube ([[Modelos de imagen a 3D en la nube gratuita (Hugging Face y Colab)]]).

> [!question]- ¿Qué cuesta usar descarga a CPU (*offloading*)?
> Tiempo y RAM: los pesos viajan entre RAM y VRAM en cada paso. Por ejemplo, el paquete WinPortable de Hunyuan3D pide **al menos 24 GB de RAM**, y dice que entre menos VRAM, más RAM se necesita.

> [!question]- Error común: leer "6 GB de VRAM" en un README y suponer que corre en tu tarjeta de 6 GB.
> Windows y el escritorio ya ocupan parte de la VRAM (a veces cientos de MB o más), y el dato del README suele medirse en Linux sin pantalla conectada. Con 6 GB exactos estás al límite: mide y deja margen.

## Práctica

- [[Lab 3 - Hunyuan3D-2mini en poca VRAM vs TripoSR]]

