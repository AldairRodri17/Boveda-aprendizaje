---
proyecto: Conocimiento
tipo: concepto
nivel: intermedio
progreso: pendiente
estado: pendiente
area: Edición de video
tags: [edicion-video]
contenido: completo
---
# Rampas de velocidad

Parte de: [[33-Edicion-Video/05 Gráficos y efectos/_Índice Gráficos y efectos|Gráficos y efectos]]

> [!info] Cómo avanzar
> `progreso`: **pendiente** → **viendo** (estudiando) → **visto** (contestaste la autoevaluación sin abrir las respuestas) → **dominado** (hiciste el laboratorio o lo aplicaste de verdad).

## Contenido

### Qué es
Una **rampa de velocidad** (*speed ramp*) es un cambio **gradual** de velocidad dentro de un mismo clip: por ejemplo, el video va a velocidad normal, se frena a cámara lenta en el momento clave y vuelve a acelerar. A diferencia de cambiar la velocidad de todo el clip, la rampa crea énfasis y energía, muy usada en reels, deportes y montajes musicales.

### Cómo funciona
El editor decide qué cuadro mostrar en cada instante según la curva de velocidad. Si la velocidad es menor al 100 %, necesita **más cuadros de los que hay**: si el clip se grabó a más fps que la línea de tiempo, los tiene; si no, los inventa.

| Grabado | Línea de tiempo | Cámara lenta limpia hasta |
|---|---|---|
| 60 fps | 30 fps | 50 % |
| 120 fps | 30 fps | 25 % |
| 240 fps | 30 fps | 12.5 % |
| 30 fps | 30 fps | 100 % (más lento = cuadros repetidos o interpolados) |

La fórmula: velocidad mínima limpia = fps de la línea de tiempo / fps grabados.

Cuando no hay cuadros suficientes, el *Retime Process* decide cómo rellenar:
- **Nearest**: repite cuadros (se ve a saltos).
- **Frame Blend**: mezcla cuadros (se ve fantasmal).
- **Optical Flow**: inventa cuadros intermedios analizando el movimiento; en la versión gratuita están los modos estándar; **Speed Warp** (con IA, mejor calidad) es de Studio.

### Qué material conviene
Material grabado en **alta cadencia** ([[Cámara lenta (high frame rate)]]):
- **Sony a6700**: hasta 4K 120p.
- **Osmo Pocket 4P**: hasta 4K a 240 fps en el gran angular.
- **iPhone 15 Pro**: cámara lenta hasta 1080p a 240 fps (y 4K a 60).
- **DJI Mini 4K**: hasta 60 fps en 2.7K y 1080p.
Además, obturación adecuada y buena luz (más fps exige más luz), y acción con un momento claro (un salto, un giro, una ola).

### Cómo se hace en Resolve
1. Clip en la línea de tiempo → **Ctrl+R** (*Retime Controls*): aparece una barra de velocidad sobre el clip.
2. Coloca el cabezal donde empieza la cámara lenta y, en el menú de la flecha de la barra, elige **Add Speed Point**; agrega otro donde termina.
3. Cambia la velocidad del segmento central (por ejemplo 25 %) desde su menú.
4. Abre la **Retime Curve** (clic derecho → *Retime Curve*) y suaviza los puntos para que la transición sea una rampa y no un salto.
5. En el Inspector → *Retime and Scaling* → *Retime Process*: **Optical Flow** si te faltan cuadros.
Si el clip de 120 fps quieres verlo todo lento, en el bin usa *Clip Attributes → Frame rate* = 30 para "conformarlo" (se reproduce al 25 %).

### Ejemplo
Reel de 30 s: tu amigo salta al agua grabado con la a6700 a 4K 120p en una línea de tiempo de 30 fps. Velocidad 100 % mientras corre, rampa a 25 % justo antes de despegar, se mantiene durante el salto y regresa a 100 % al tocar el agua, coincidiendo con un golpe de la música ([[Editar al ritmo de la música]]).

### Errores comunes
- **Rampa a 20 % en un clip de 30 fps**: solo hay 6 cuadros reales por segundo de tiempo de pantalla (30 × 0.20); el editor repite o mezcla cuadros y se ve entrecortado o con fantasmas. Optical Flow ayuda, pero genera deformaciones en bordes y objetos que se cruzan. Graba a alta cadencia si planeas cámara lenta.
- **Rampas sin suavizar**: cambios bruscos de velocidad.
- **Rampas en todos los clips**: pierden impacto.
- **Olvidar el audio**: el sonido del clip se distorsiona; silencia y usa música o efectos.

### Para profundizar
- [Capacitación oficial de DaVinci Resolve (retime)](https://www.blackmagicdesign.com/products/davinciresolve/training)
- [Diferencias de Resolve Studio (Speed Warp)](https://www.blackmagicdesign.com/products/davinciresolve/studio)

## Autoevaluación

Contesta primero en voz alta o en papel; luego despliega la respuesta.

> [!question]- ¿Qué es una rampa de velocidad?
> Un cambio gradual de velocidad dentro de un clip (normal → lento → normal).

> [!question]- ¿Qué material conviene para rampas?
> Grabado a 60 fps o más, para que la parte lenta se vea fluida.

> [!question]- ¿Cómo se hace en Resolve?
> Con **Retime Controls** (Ctrl+R) en el clip: agregas puntos de velocidad y los suavizas con la curva de retime.

> [!question]- Error común: rampa a 20 % en un clip de 30 fps. ¿Qué pasa?
> Cuadros repetidos o interpolación con artefactos.

## Práctica

- [[Lab 7 - Rampa de velocidad y estabilización]]

