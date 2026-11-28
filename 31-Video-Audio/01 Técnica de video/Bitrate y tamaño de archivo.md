---
proyecto: Conocimiento
tipo: concepto
nivel: basico
progreso: pendiente
estado: pendiente
area: Video y audio
tags: [video-audio]
contenido: completo
---
# Bitrate y tamaño de archivo

Parte de: [[31-Video-Audio/01 Técnica de video/_Índice Técnica de video|Técnica de video]]

> [!info] Cómo avanzar
> `progreso`: **pendiente** → **viendo** (estudiando) → **visto** (contestaste la autoevaluación sin abrir las respuestas) → **dominado** (hiciste el laboratorio o lo aplicaste de verdad).

## Contenido

### Qué es
El **bitrate** (tasa de bits) es la cantidad de datos que se usan por cada segundo de video, normalmente en **megabits por segundo (Mbps)**. Con el mismo códec y resolución, más bitrate significa menos compresión: más detalle en escenas complejas y archivos más grandes. Sirve para dos cosas prácticas: **calcular cuánto espacio necesitas** y **saber si tu tarjeta o disco aguanta** la grabación.

### Cómo funciona
**Bits vs. bytes.** El bitrate se mide en **megabits** (Mb, con *b* minúscula); el espacio y la velocidad de las tarjetas, en **megabytes** (MB, con *B* mayúscula). **1 byte = 8 bits.** Confundirlos te hace calcular 8 veces mal.

**Fórmula del tamaño:**

tamaño (MB) = bitrate (Mbps) × segundos / 8  ·  tamaño (GB) ≈ MB / 1000

Ejemplos verificados:
- 3 min a 50 Mbps → 50 × 180 / 8 = **1,125 MB ≈ 1.1 GB**.
- ProRes 422 HQ en 4K a ~30 fps (Apple lo ubica en unos 700 Mbps) → 700 × 60 / 8 = 5,250 MB ≈ **5.3 GB por minuto**.
- Quieres 10 min en 1 GB → 1000 MB × 8 / 600 s ≈ **13.3 Mbps** totales (video + audio).

```python
# Calculadora rápida de tamaño de video (tamaños decimales: 1 GB = 1000 MB)
def tamano_gb(mbps: float, segundos: float) -> float:
    return mbps * segundos / 8 / 1000

def bitrate_para(gb: float, segundos: float) -> float:
    return gb * 1000 * 8 / segundos

print(tamano_gb(50, 180))      # 1.125
print(tamano_gb(600, 3600))    # 270.0  -> 1 h de XAVC S-I 4K60 en la a6700
print(bitrate_para(1, 600))    # 13.33
```

**Velocidad de escritura.** Para grabar un flujo de 400 Mbps necesitas escribir 400 / 8 = **50 MB/s sostenidos**; una tarjeta que solo sostiene 30 MB/s no alcanza y la cámara se detiene o se niega a grabar. Para ProRes 4K60 en un SSD externo, Apple pide como mínimo **220 MB/s** de escritura. Mira [[Tarjetas de memoria (clases de velocidad)]] y [[SSD externo para grabar y respaldar]].

**Por qué aparecen bloques.** Con un bitrate fijo, las escenas con **mucho detalle que cambia en cada cuadro** (confeti, agua, follaje con viento, lluvia) necesitan más datos de los que hay. El códec descarta detalle y aparecen **bloques** (*macroblocking*) y "lodo" en las texturas.

| Bitrate | GB por minuto | GB por hora |
|---|---|---|
| 8 Mbps (YouTube 1080p) | 0.06 | 3.6 |
| 100 Mbps | 0.75 | 45 |
| 200 Mbps | 1.5 | 90 |
| 600 Mbps | 4.5 | 270 |

### Ejemplo
Planeas grabar una boda de 2 h con la [[Sony a6700]]:
- En **XAVC HS 4K** (H.265, 10 bits) a 100 Mbps → unos 90 GB. Cabe en una tarjeta de 128 GB con margen.
- En **XAVC S-I 4K 59.94p** a 600 Mbps → 540 GB y escritura sostenida de 75 MB/s: necesitas tarjetas V90/UHS-II grandes, y la cámara puede calentarse.
- Con el [[iPhone 15 Pro para foto y video]] en ProRes, calcula antes con la fórmula y lleva el SSD.

Para la entrega, YouTube recomienda para SDR: **8 Mbps** en 1080p a 24–30 fps, **12 Mbps** a 48–60 fps, y **35–45 Mbps** en 4K a 24–30 fps.

### Cuándo usarlo y cuándo no
- **Bitrate alto** para material que vas a corregir mucho, escenas con mucho detalle o Log.
- **Bitrate moderado** (H.265) para clips largos y uso diario.
- **No subas el bitrate de exportación sin límite**: la plataforma lo recomprime; sigue sus recomendaciones.

### Errores comunes
1. **Confundir MB con Mb**: 100 Mbps no son 100 MB por segundo, son 12.5 MB/s.
2. **Comprar tarjetas por capacidad y no por velocidad sostenida**.
3. **Bajar el bitrate para ahorrar** y grabar agua o follaje: bloques irreparables.
4. **Olvidar el audio** al calcular un tamaño objetivo (PCM estéreo de 48 kHz/24 bits ronda 2.3 Mbps).
5. **No dejar margen**: la tarjeta llena a media toma es la falla más común.

### Para profundizar
- [Acerca de Apple ProRes en el iPhone (Apple)](https://support.apple.com/es-lamr/109041)
- [Recommended upload encoding settings (YouTube)](https://support.google.com/youtube/answer/1722171?hl=en)
- [Especificaciones de la Sony a6700 (Sony)](https://www.sony.co.in/interchangeable-lens-cameras/products/ilce-6700/spec)

## Ejercicios

Resuélvelos en papel o en la herramienta; la solución está plegada.

> [!question]- ¿Cuánto ocupa un clip de 3 min a 50 Mbps?
> 50 × 180 / 8 = **1125 MB ≈ 1.1 GB**.

> [!question]- Una tarjeta/SSD escribe de forma sostenida 30 MB/s. ¿Puede grabar un flujo de 400 Mbps?
> 400 / 8 = 50 MB/s > 30 MB/s → **no**; se cortaría la grabación.

> [!question]- Apple indica que ProRes 422 HQ en 4K a ~30 fps ronda los 700 Mbps. ¿Cuántos GB ocupa 1 min, aproximadamente?
> 700 × 60 / 8 = 5250 MB ≈ **5.3 GB por minuto** (por eso se agota el almacenamiento del iPhone tan rápido).

> [!question]- Quieres un archivo de 1 GB para 10 min de video. ¿Qué bitrate total (video + audio) necesitas?
> 1000 MB × 8 / 600 s ≈ **13.3 Mbps** totales.

## Autoevaluación

Contesta primero en voz alta o en papel; luego despliega la respuesta.

> [!question]- ¿Qué es el bitrate?
> La cantidad de datos por segundo de video, en **Mbps** (megabits por segundo). Más bitrate = más detalle preservado, sobre todo en escenas con mucho movimiento o textura, y archivos más grandes.

> [!question]- ¿Cómo calculas el tamaño de un archivo a partir del bitrate?
> Tamaño (MB) ≈ Mbps × segundos / 8 (porque 8 bits = 1 byte). Ejemplo: 100 Mbps × 60 s / 8 = 750 MB por minuto.

> [!question]- Caso: grabas confeti, agua o follaje con viento y ves bloques en la imagen. ¿Qué pasa?
> El bitrate no alcanza para tanto detalle cambiante y el códec "aplana" la imagen (macrobloques). Sube el bitrate o usa un códec menos comprimido (ProRes) en esas tomas.

> [!question]- Error común: confundir MB (megabytes) con Mb (megabits). ¿Cuál es la diferencia?
> 1 byte = 8 bits. 80 Mbps son **10 MB por segundo**, no 80 MB por segundo.

## Práctica

- [[Lab 1 - Obturación a 180 grados y cámara lenta]]

