---
proyecto: Conocimiento
tipo: laboratorio
nivel: basico
progreso: pendiente
estado: borrador
area: IA para modelado 3D
herramienta: Windows, driver NVIDIA, Python 3.10, PyTorch con CUDA (gratuitos), terminal de PowerShell
tags: [ia-modelado-3d, laboratorio]
contenido: completo
---
# Lab 1 - Preparar Python, CUDA y PyTorch y verificar la GPU

Parte de: [[39-IA-Modelado-3D/02 Entorno local/_Índice Entorno local|Entorno local]]

**Cubre:** [[Python, CUDA y PyTorch para IA 3D en Windows]] · [[VRAM - medirla y qué hacer cuando no alcanza]] · [[Tarjetas GTX 16 y media precisión (FP16)]]

> [!info] Al terminar
> Si todos los criterios quedan marcados, cambia a `dominado` el `progreso` de este laboratorio y de las notas que cubre. Guarda tus archivos de práctica en esta misma carpeta.

## Objetivo

Dejar un entorno de Python aislado con PyTorch que use la NVIDIA GTX 1660 Ti de 6 GB, y un script que mida la VRAM, listo para los demás laboratorios.

## Pasos

1. Actualiza el driver de NVIDIA desde la página o la app de NVIDIA y reinicia. En PowerShell ejecuta `nvidia-smi` y anota el nombre de la GPU, la memoria total y el "CUDA Version".
2. Instala Python 3.10 de 64 bits (marca *Add python.exe to PATH*). Comprueba con `py -3.10 --version`.
3. Crea la carpeta `C:\IA3D` y dentro el entorno: `py -3.10 -m venv .venv` y actívalo con `.venv\Scripts\Activate.ps1`. Ver [[Python - entornos virtuales y pip]].
4. En [pytorch.org](https://pytorch.org/get-started/locally/) elige Windows, Pip, Python y una versión de CUDA **igual o menor** a la que mostró `nvidia-smi`; ejecuta el comando que te da.
5. Verifica con: `python -c "import torch; print(torch.__version__, torch.version.cuda, torch.cuda.is_available(), torch.cuda.get_device_name(0), torch.cuda.get_device_capability(0))"`.
6. Crea `vram.py` que reserve un tensor grande en la GPU, imprima `torch.cuda.max_memory_allocated()` en MiB y lo compare con lo que ves en `nvidia-smi` en otra terminal.
7. Solo si harás los labs 2 y 3 con compilación: instala **Visual Studio Build Tools** con la carga *Desarrollo para el escritorio con C++* y el **CUDA Toolkit** de la misma versión mayor que tu PyTorch. Comprueba con `nvcc --version`.
8. Anota en esta nota las versiones finales (driver, Python, PyTorch, CUDA de PyTorch, CUDA Toolkit).

## Criterios de terminado

- [ ] `nvidia-smi` muestra la GTX 1660 Ti con ~6 GB.
- [ ] El comando de verificación imprime `True`, el nombre de la GPU y `(7, 5)`.
- [ ] `torch.__version__` **no** termina en `+cpu`.
- [ ] `vram.py` reporta un pico cercano al tamaño del tensor que reservaste y `nvidia-smi` muestra un uso mayor o igual.
- [ ] (Si compilarás) `nvcc --version` muestra la misma versión mayor de CUDA que `torch.version.cuda`.

## Solución

> [!success]- vram.py de referencia
> ```python
> import torch
> assert torch.cuda.is_available(), "No hay GPU visible"
> dev = torch.device("cuda")
> x = torch.empty(256 * 1024 * 1024, dtype=torch.float32, device=dev)  # 1 GiB
> torch.cuda.synchronize()
> print("Pico (MiB):", torch.cuda.max_memory_allocated() / 2**20)
> print("Libre/total (MiB):", [v / 2**20 for v in torch.cuda.mem_get_info()])
> input("Mira nvidia-smi y presiona Enter...")
> ```
> Debe imprimir ~1024 MiB de pico. `mem_get_info()` devuelve memoria libre y total de la GPU.

> [!success]- Si `is_available()` da False
> Si la versión termina en `+cpu`: `pip uninstall torch torchvision` y reinstala con el comando de pytorch.org que incluye `--index-url .../cuXXX`. Si la versión es de CUDA pero sigue en False: actualiza el driver (el "CUDA Version" de `nvidia-smi` debe ser ≥ al de PyTorch) y confirma con `where python` que estás en `.venv`.

> [!success]- Por qué Python 3.10
> Es la versión que piden algunos repos de esta área (Hunyuan3D-2GP) y tiene ruedas precompiladas para la mayoría de dependencias. Si un README pide otra, crea otro entorno: un entorno por proyecto evita conflictos.

## Notas personales

_Qué me costó, errores que cometí, dudas._
