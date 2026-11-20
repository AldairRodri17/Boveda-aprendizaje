---
proyecto: Conocimiento
tipo: concepto
nivel: basico
progreso: pendiente
estado: pendiente
area: Edición de video
tags: [edicion-video]
---
# J-cut y L-cut (edición)

Parte de: [[Tipos de corte (J-cut, L-cut, jump cut y match cut)]]

> [!info] Cómo avanzar
> `progreso`: **pendiente** → **viendo** (estudiando) → **visto** (contestaste la autoevaluación sin abrir las respuestas) → **dominado** (hiciste el laboratorio o lo aplicaste de verdad).

## Contenido

### Qué es
Los **J-cut** y **L-cut** son *cortes divididos* (split edits): el corte del audio y el de la imagen no ocurren en el mismo cuadro. El nombre viene de la forma que dibujan los clips en la línea de tiempo.
- **J-cut**: el **audio de la siguiente toma empieza antes** que su imagen. Oyes lo que viene y luego lo ves.
- **L-cut**: la **imagen cambia, pero el audio de la toma anterior continúa**. Sigues oyendo a la persona mientras ves otra cosa.

### Cómo funciona
```text
J-cut                          L-cut
V1: [ Toma A ][ Toma B    ]    V1: [ Toma A ][ Toma B    ]
A1: [ A ][ Audio B        ]    A1: [ Audio A      ][ B   ]
         ^ el audio B entra antes        ^ el audio A sigue
```
Funcionan porque el sonido **guía la atención**: un J-cut prepara al espectador para el cambio (por eso se siente natural), y un L-cut mantiene la continuidad del discurso mientras muestras otra cosa. Es lo que ocurre en la vida real: oyes una voz y después volteas a ver quién habla.

### Cómo hacerlos en Resolve
1. Pon las dos tomas con corte directo en la línea de tiempo.
2. Desvincula temporalmente audio y video: activa/desactiva **Linked Selection** (Ctrl+Shift+L) o mantén **Alt** al hacer clic para seleccionar solo el audio o solo el video.
3. Con la herramienta de recorte (**T**, *Trim Edit Mode*), arrastra el punto de edición **solo del audio** (J-cut: el audio de B hacia la izquierda) o **solo del video** (L-cut: el video de B hacia la izquierda mientras el audio de A se extiende).
4. Alternativa: usa *roll* (rodar) sobre el punto de edición del audio para moverlo sin cambiar la duración total.
5. Revisa con un fundido corto de audio (2–4 cuadros) para evitar clics.

En Kdenlive y Shotcut el proceso es igual: desagrupas o seleccionas solo la pista de audio y mueves su borde.

### Ejemplo: entrevista con L-cut
La persona dice "empecé a imprimir en 3D hace dos años". En la mitad de la frase cortas la imagen a b-roll de la impresora trabajando, mientras su voz sigue: L-cut. Cuando dice "y ahora vendo piezas", regresas a su cara. Ves la acción sin perder el hilo y además puedes tapar un corte de muletilla.

Un **J-cut** típico: antes de cortar a la toma del dron sobre el mar, haces que el sonido de las olas entre un segundo antes.

### Cuándo usarlos
- **L-cut en entrevistas**: para mostrar b-roll o la reacción de quien escucha sin interrumpir la respuesta; para salir de una escena dejando que la frase termine sobre la siguiente imagen.
- **J-cut**: para entrar a una escena nueva o a un cambio de lugar; para que la respuesta de la otra persona se oiga antes de verla en un diálogo.
- Cuando el corte directo se siente brusco.

No los necesitas en montajes musicales donde la imagen corta al beat y el audio es solo la canción.

### Errores comunes
- **Arrastrar sin desvincular**: mueves audio y video juntos y no logras el desfase.
- **Desfasar demasiado**: si el audio adelantado dura 3 segundos, el espectador se confunde sobre de dónde viene.
- **Dejar desincronizado el clip** al moverlo: Resolve marca en rojo el desfase en el clip; corrígelo con clic derecho → *Slip into place* (o vuelve a vincular).
- **Olvidar el ambiente**: si el audio de A tiene ambiente distinto al de B, el L-cut deja un "hueco" sonoro; agrega una pista de ambiente continua.

### Para profundizar
- [Capacitación oficial de DaVinci Resolve (recorte y edición)](https://www.blackmagicdesign.com/products/davinciresolve/training)
- [Manual de Kdenlive](https://docs.kdenlive.org/)

## Autoevaluación

Contesta primero en voz alta o en papel; luego despliega la respuesta.

> [!question]- ¿Qué es un J-cut?
> El **audio** de la siguiente toma empieza **antes** que su imagen. En la línea de tiempo el clip forma una J (el audio se extiende a la izquierda).

> [!question]- ¿Qué es un L-cut?
> La **imagen** cambia pero el **audio** de la toma anterior continúa. Forma una L (el audio se extiende a la derecha).

> [!question]- ¿Cuándo usarías un L-cut en una entrevista?
> Cuando la persona sigue hablando y cortas a su b-roll o a la reacción de quien escucha.

> [!question]- ¿Cómo se hacen en Resolve?
> Desvinculando temporalmente audio y video (Link) o recortando solo la pista de audio con Alt/Opción mientras arrastras el borde del clip.

## Práctica

- [[Lab 2 - Montaje de entrevista con J-cuts y L-cuts]]

