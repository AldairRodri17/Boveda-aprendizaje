---
proyecto: Conocimiento
tipo: concepto
nivel: intermedio
progreso: pendiente
estado: pendiente
area: IA para modelado 3D
tags: [ia-modelado-3d]
---
# Stable Fast 3D

Parte de: [[39-IA-Modelado-3D/03 Modelos/_Índice Modelos|Modelos]]

> [!info] Cómo avanzar
> `progreso`: **pendiente** → **viendo** (estudiando) → **visto** (contestaste la autoevaluación sin abrir las respuestas) → **dominado** (hiciste el laboratorio o lo aplicaste de verdad).

## Contenido

### Qué es
**Stable Fast 3D (SF3D)** es un modelo de Stability AI que, a partir de una imagen de 512×512, genera una malla **con UV, textura y parámetros de material**, de pocos polígonos, en menos de un segundo en su hardware de referencia. Es de reconstrucción directa, como TripoSR, pero pensado para entregar un asset "listo para juego": malla ligera, textura limpia y material (rugosidad y metalicidad). Exporta **GLB**. Su licencia es la **Stability AI Community License**.

### Cómo funciona
1. **Preprocesado:** quita el fondo y centra el objeto (puedes ajustar el tamaño relativo del objeto).
2. **Red de reconstrucción:** un transformer predice una representación 3D a partir de la imagen.
3. **Malla y UV:** extrae una malla de pocos polígonos y la **desenvuelve** (UV) automáticamente.
4. **Textura y material:** hornea el color en una textura y estima parámetros de material.
5. **Iluminación:** separa la iluminación de la imagen para que la textura no traiga sombras "pintadas".

Resultado: un GLB que se ve bien en un visor o motor de juego. Para imprimir, la textura y el material no se usan; lo que importa es la forma.

### Requisitos
- Según su [README](https://github.com/Stability-AI/stable-fast-3d): unos **6 GB de VRAM** con opciones por defecto para una imagen. Justo en tu límite: cierra todo lo demás.
- **Windows es experimental:** piden Visual Studio (probado con 2022) y PyTorch/CUDA acordes a tu sistema.
- Sin GPU usa CPU; para forzarlo, la variable `SF3D_USE_CPU=1`.

### El paso extra: modelo restringido
El modelo es **gated** en Hugging Face: antes de descargarlo debes aceptar las condiciones en la [página del modelo](https://huggingface.co/stabilityai/stable-fast-3d), crear un **token de lectura** en tu cuenta y autenticarte.

### Ejemplo: instalación en Windows
```powershell
cd C:\IA3D
git clone https://github.com/Stability-AI/stable-fast-3d.git
cd stable-fast-3d
py -3.10 -m venv .venv; .venv\Scripts\Activate.ps1
pip install torch torchvision --index-url https://download.pytorch.org/whl/cu126
pip install -U setuptools==69.5.1
pip install wheel
pip install -r requirements.txt

# Acepta la licencia en la página del modelo y luego:
huggingface-cli login     # pega tu token de lectura (no lo guardes en el repo)

# Genera (revisa en el README el nombre exacto del script y sus opciones)
python run.py C:\fotos\taza.png --output-dir out\
```
El token es un secreto: no lo pongas en scripts que subas a Git. Nombre exacto de las opciones de `run.py`: revisa el README de la versión que clones (Pendiente de confirmar).

### ¿Puedes vender impresiones hechas con SF3D?
Con la [Stability AI Community License](https://stability.ai/community-license-agreement): **sí**, uso comercial gratuito si tú (y tus afiliadas) tienen ingresos anuales de hasta **1 millón de dólares**, y debes **registrarte** con Stability AI para uso comercial. Los resultados son tuyos en la medida en que la ley lo permita. Detalle en [[Licencia Stability AI Community]].

### SF3D frente a TripoSR para imprimir
| | TripoSR | Stable Fast 3D |
|---|---|---|
| Licencia | MIT | Stability AI Community |
| VRAM | ~6 GB | ~6 GB |
| Malla | Densa, color por vértice | Ligera, UV y textura |
| Instalación | Pública | Modelo restringido + token |
| Windows | Funciona (compilar torchmcubes) | Experimental |
| Para imprimir | Más triángulos = más detalle de forma | Pocos polígonos: puede perder detalle fino |

Para una figura que vas a imprimir, la malla ligera de SF3D puede verse facetada al escalarla; Voxel Remesh y un suavizado leve en Blender lo compensan ([[Limpiar un modelo generado por IA en Blender]]).

### Cuándo usarlo y cuándo no
- **Úsalo** cuando quieras una referencia de color con UV limpias (para pintar a mano o para separar colores en Blender) o un asset para pantalla.
- **Prefiere TripoSR** si solo te interesa la forma y quieres evitar el registro.
- **No** cuentes con él si no quieres aceptar los términos de Stability.

### Errores comunes
1. **No aceptar el acceso en Hugging Face.** La descarga falla con error 401/403.
2. **Saltarte la versión fija de `setuptools`.** La instalación de dependencias compiladas puede fallar.
3. **Vender sin registrarte.** La licencia pide registro para uso comercial.
4. **Esperar mucho detalle de forma.** Está optimizado para pocos polígonos.

### Para profundizar
- [Stable Fast 3D en GitHub](https://github.com/Stability-AI/stable-fast-3d)
- [Tarjeta del modelo en Hugging Face](https://huggingface.co/stabilityai/stable-fast-3d)
- [Stability AI Community License](https://stability.ai/community-license-agreement)

## Autoevaluación

Contesta primero en voz alta o en papel; luego despliega la respuesta.

> [!question]- ¿Qué es Stable Fast 3D (SF3D) y qué entrega?
> Un modelo de Stability AI que genera en menos de un segundo (en su hardware de referencia) una malla **con UV y textura**, de pocos polígonos, con parámetros de material, a partir de una imagen de 512×512 ([tarjeta del modelo](https://huggingface.co/stabilityai/stable-fast-3d)). Exporta **GLB**.

> [!question]- ¿Cuánta VRAM pide y corre en Windows?
> Según su [README](https://github.com/Stability-AI/stable-fast-3d), unos **6 GB** con opciones por defecto para una imagen. El soporte de Windows es **experimental**: pide Visual Studio (probado con 2022) y PyTorch/CUDA acordes a tu sistema. Sin GPU usa CPU; para forzarlo, `SF3D_USE_CPU=1`.

> [!question]- ¿Qué paso extra tiene la instalación?
> El modelo está **restringido** (*gated*) en Hugging Face: pides acceso en la página del modelo, creas un token de lectura y haces `huggingface-cli login` antes de correrlo. Instalación: `pip install -U setuptools==69.5.1`, `pip install wheel`, `pip install -r requirements.txt`.

> [!question]- ¿Puedes vender impresiones hechas con SF3D?
> Con la [Stability AI Community License](https://stability.ai/community-license-agreement): uso comercial gratuito si tú (y tus afiliadas) tienen ingresos anuales de hasta 1 millón de dólares, **registrándote** con Stability AI para uso comercial. Ver [[Licencia Stability AI Community]].

