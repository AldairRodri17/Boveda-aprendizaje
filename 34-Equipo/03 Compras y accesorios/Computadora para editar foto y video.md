---
proyecto: Conocimiento
tipo: concepto
nivel: basico
progreso: pendiente
estado: pendiente
area: Equipo
tags: [equipo]
contenido: completo
---
# Computadora para editar foto y video

Parte de: [[34-Equipo/03 Compras y accesorios/_Índice Compras y accesorios|Compras y accesorios]]

> [!info] Cómo avanzar
> `progreso`: **pendiente** → **viendo** (estudiando) → **visto** (contestaste la autoevaluación sin abrir las respuestas) → **dominado** (hiciste el laboratorio o lo aplicaste de verdad).

## Contenido

### Qué es
La computadora de edición es donde tu material se convierte en video terminado. Para foto basta casi cualquier equipo moderno; para video 4K, sobre todo con [[DaVinci Resolve (versión gratuita)]], importa mucho **qué** componentes tienes, porque Resolve hace la mayor parte del trabajo en la **tarjeta gráfica**. Esta nota te ayuda a entender qué pide el programa, qué tan lejos está tu PC actual (GTX 1660 Ti de 6 GB) y qué hacer antes de gastar.

### Cómo funciona
**Requisitos mínimos publicados por Blackmagic** para DaVinci Resolve 21 ([notas de la versión 21.1.1](https://www.blackmagicdesign.com/support/content/readme/dfbb6e4233c144d1b328c109c7ebc11e)):

| | Windows | Mac |
|---|---|---|
| Sistema | Windows 10 Creators Update o posterior | Apple Silicon, macOS 15 Sequoia o posterior |
| RAM | 16 GB (32 GB con Fusion) | 8 GB (16 GB con Fusion) |
| GPU | Integrada o dedicada con al menos 4 GB de VRAM | integrada |
| Controlador | NVIDIA Studio 581.57 o más reciente | — |

Tu GTX 1660 Ti con 6 GB **cumple** el mínimo de VRAM.

**Qué hace cada pieza**:

```mermaid
flowchart LR
  D[Disco: lee los clips] --> CPU[CPU: decodifica H.264/H.265]
  CPU --> GPU[GPU: color, efectos, ruido, escalado]
  GPU --> RAM[RAM: línea de tiempo, Fusion, caché]
  RAM --> V[Visor fluido]
```

1. **GPU y su VRAM**: color, reducción de ruido, estabilización y efectos. Si se queda sin VRAM, Resolve muestra errores de memoria de GPU o va muy lento.
2. **RAM**: 16 GB funciona para proyectos sencillos; 32 GB da aire con 4K, Fusion o muchas pistas.
3. **SSD NVMe** para medios y caché: el disco no debe ser el cuello de botella.
4. **CPU**: decodificar H.264/H.265 de celulares y drones es pesado. La aceleración por hardware de esos códecs depende de la GPU, del formato (4:2:0 o 4:2:2, 8 o 10 bits) y de si usas la versión gratuita o Studio (Pendiente de confirmar para tu combinación).

### Ejemplo
Tus clips HEVC 4K del iPhone se traban en la línea de tiempo. Antes de comprar otra computadora:

1. Actualiza al controlador **NVIDIA Studio** recomendado.
2. Pon la **caché** y los medios en un SSD, no en un disco mecánico.
3. Genera **proxies** u *optimized media* (clic derecho en los clips del Media Pool) y edita con ellos; al exportar, Resolve usa los originales. Ver [[Proxies y rendimiento al editar]].
4. Baja la resolución de reproducción a media o un cuarto (*Playback > Timeline Proxy Resolution*).
5. Si grabas en Blackmagic Camera, considera **ProRes**: pesa mucho más, pero se decodifica con muy poco esfuerzo.

Con eso, una PC como la tuya edita 4K del iPhone y del [[DJI Mini 4K]] con fluidez razonable. El salto a S-Log3 10 bits 4:2:2 de la a6700 es más pesado: prueba con proxies antes de decidir.

**Si un día compras**: prioriza GPU con más VRAM (8 GB o más), 32 GB de RAM y un NVMe de 1 TB para proyectos, antes que el procesador más caro.

### Cuándo invertir y cuándo no
- **No compres todavía** si el problema se resuelve con proxies, caché en SSD y controladores al día.
- **Considera mejorar** cuando el trabajo de color (nodos, reducción de ruido) sea lento incluso con proxies, cuando la VRAM se agote, o cuando edites varias cámaras 4K a la vez.
- Para foto (Lightroom, darktable), la RAM y el SSD importan más que la GPU.

### Errores comunes
- **Comprar solo por el procesador e ignorar la GPU**: en Resolve, la corrección de color y los efectos van lentos o fallan por falta de VRAM.
- **Editar desde un disco mecánico o una USB lenta**: el visor se traba aunque la PC sea buena.
- **Usar controladores "Game Ready" viejos**: Blackmagic recomienda la rama Studio.
- **Dejar la caché en el disco del sistema casi lleno**: Resolve se vuelve inestable.
- **No usar proxies por orgullo**: los editores profesionales los usan todo el tiempo.

### Para profundizar
- [Blackmagic: notas de la versión 21.1.1 (requisitos)](https://www.blackmagicdesign.com/support/content/readme/dfbb6e4233c144d1b328c109c7ebc11e)
- [Apple: grabar ProRes en el iPhone](https://support.apple.com/en-us/109041)

## Autoevaluación

Contesta primero en voz alta o en papel; luego despliega la respuesta.

> [!question]- ¿Qué requisitos mínimos publica Blackmagic para DaVinci Resolve 21 en Windows?
> **Windows 10 Creators Update** o posterior, **16 GB de RAM (32 GB si usas Fusion)**, GPU integrada o dedicada con **al menos 4 GB de VRAM**, y controladores oficiales (NVIDIA Studio 581.57 o más reciente). En Mac: **Apple Silicon**, macOS 15 Sequoia o posterior, 8 GB (16 GB con Fusion). Fuente: [notas de la versión 21.1.1](https://www.blackmagicdesign.com/support/content/readme/dfbb6e4233c144d1b328c109c7ebc11e).

> [!question]- ¿Qué componentes importan más para editar en DaVinci Resolve?
> La **GPU** (Resolve procesa color y efectos en la tarjeta gráfica), luego **RAM** y un **SSD NVMe** para medios y caché. El procesador importa para decodificar H.264/H.265 de celulares y drones.

> [!question]- Caso: tu laptop cumple el mínimo pero los clips HEVC 4K del iPhone se traban. ¿Qué haces antes de comprar otra?
> Genera **proxies** u *optimized media* y edita con ellos; baja la resolución de reproducción; pon la caché en el SSD. Ver [[Proxies y rendimiento al editar]].

> [!question]- Error común: comprar una PC solo por el procesador e ignorar la GPU. ¿Qué pasa en Resolve?
> Corrección de color, reducción de ruido y efectos van lentos o fallan por falta de **VRAM**. Para Resolve, una GPU con más memoria suele rendir más que un CPU más caro.

