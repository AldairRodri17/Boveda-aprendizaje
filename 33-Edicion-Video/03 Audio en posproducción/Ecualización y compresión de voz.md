---
proyecto: Conocimiento
tipo: concepto
nivel: intermedio
progreso: pendiente
estado: pendiente
area: Edición de video
tags: [edicion-video]
---
# Ecualización y compresión de voz

Parte de: [[33-Edicion-Video/03 Audio en posproducción/_Índice Audio en posproducción|Audio en posproducción]]

> [!info] Cómo avanzar
> `progreso`: **pendiente** → **viendo** (estudiando) → **visto** (contestaste la autoevaluación sin abrir las respuestas) → **dominado** (hiciste el laboratorio o lo aplicaste de verdad).

## Contenido

### Qué es
Son las dos herramientas básicas para que una voz se oiga clara y pareja:
- El **ecualizador (EQ)** sube o baja **rangos de frecuencia**: quita retumbe, lodo o aspereza y resalta la inteligibilidad.
- El **compresor** controla la **dinámica**: baja automáticamente las partes fuertes cuando pasan un umbral, para que la diferencia entre lo bajo y lo alto sea menor y luego puedas subir todo.

### Cómo funciona la EQ en la voz
| Rango | Qué tiene | Acción típica |
|---|---|---|
| Debajo de 80–100 Hz | Retumbe, golpes, tráfico | **Filtro pasa altos** (high-pass) |
| 200–400 Hz | "Lodo", sonido de caja | Bajar 2–4 dB si suena opaco |
| 2–5 kHz | Presencia, inteligibilidad | Subir 1–3 dB con cuidado |
| 5–8 kHz | Sibilancia (eses) | De-esser si molesta |

Principio: **corta antes de subir**. Corta con filtros estrechos lo que molesta y sube con curvas anchas y poco.

### Cómo funciona un compresor
- **Umbral (threshold)**: nivel a partir del cual actúa.
- **Ratio**: cuánto reduce lo que pasa el umbral (3:1 = por cada 3 dB arriba, solo deja salir 1).
- **Ataque y liberación**: qué tan rápido empieza y deja de actuar.
- **Ganancia de compensación (make-up)**: sube todo después de reducir los picos.

```mermaid
flowchart LR
    V[Voz] --> HP[Pasa altos 80 Hz] --> EQ[EQ correctiva] --> CMP[Compresor 3:1<br/>3-6 dB de reducción] --> DS[De-esser] --> L[Limitador -1 dBTP]
```

Para voz hablada, un punto de partida: ratio 2:1 a 4:1, ataque 5–20 ms, liberación 50–150 ms, umbral para que el medidor de reducción marque 3–6 dB en las partes normales.

### Ejemplo en Resolve (Fairlight)
Voz del [[DJI Mic Mini 2]] para un tutorial:
1. Pista de voz → panel de mezclador → **EQ** de la pista: banda 1 como pasa altos en 90 Hz, banda 2 −3 dB en 300 Hz (Q medio), banda 4 +2 dB en 4 kHz.
2. **Dynamics** de la pista: compresor ratio 3:1, umbral para 4 dB de reducción, make-up +4 dB.
3. Si las eses pican, agrega el efecto **De-Esser** de Fairlight FX.
4. Mide el loudness al final ([[Loudness (LUFS) por plataforma]]).

### Caso: habla bajito y de pronto grita
Primero ataca el problema de **nivel**, no de tono. Lo que más ayuda es ajustar el volumen del clip o con keyframes en la parte del grito (o cortar el clip y bajarle ganancia) y luego un **compresor** para emparejar; un **limitador** al final evita que el grito sature. La EQ no resuelve diferencias de volumen. Si el grito ya llegó saturado (clipping) en la grabación, no hay compresor que lo arregle: de ahí la importancia de grabar en 32 bits flotantes o con margen.

### Cuándo usarlas y cuándo no
Úsalas en casi toda voz para video. Si la voz ya suena bien, aplica poco. Para música de fondo usa EQ para hacer espacio a la voz ([[Mezcla de voz y música]]).

### Errores comunes
- **Comprimir demasiado**: la voz suena aplastada y sin vida, el ruido de fondo y las respiraciones suben entre frases, se oye "bombeo" (el fondo sube y baja). Busca 3–6 dB de reducción, no 15.
- **Subir graves para "voz de locutor"**: queda retumbando en bocinas pequeñas y celulares.
- **Ecualizar en solo**: ajusta escuchando la mezcla completa.
- **Orden incorrecto**: limpia ruido antes de comprimir.

### Para profundizar
- [Fairlight en DaVinci Resolve](https://www.blackmagicdesign.com/products/davinciresolve/fairlight)
- [Manual de Audacity (efectos de compresión y EQ)](https://manual.audacityteam.org/)

## Autoevaluación

Contesta primero en voz alta o en papel; luego despliega la respuesta.

> [!question]- ¿Qué hace un ecualizador (EQ) en la voz?
> Sube o baja frecuencias. Típico: filtro **pasa altos** alrededor de 80–100 Hz (quita retumbos), bajar algo de 200–400 Hz si suena "encajonada", y un leve realce de 3–5 kHz para claridad.

> [!question]- ¿Qué hace un compresor?
> Reduce la diferencia entre las partes fuertes y suaves de la voz, para que se entienda pareja. Parámetros: umbral, ratio, ataque, liberación y ganancia de compensación.

> [!question]- Caso: la persona habla bajito y de pronto grita. ¿Qué herramienta ayuda primero?
> Ajustar el volumen por partes (clip gain o automatización) y luego un compresor moderado (ratio 3:1 o 4:1) para emparejar.

> [!question]- Error común: comprimir demasiado. ¿Qué se escucha?
> Voz sin vida, ruido de fondo subiendo en las pausas y respiraciones exageradas.

## Práctica

- [[Lab 4 - Limpieza de voz y loudness para YouTube]]

