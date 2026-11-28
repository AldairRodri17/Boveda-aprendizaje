---
proyecto: Conocimiento
tipo: concepto
nivel: basico
progreso: pendiente
estado: pendiente
area: IA para modelado 3D
tags: [ia-modelado-3d]
contenido: completo
---
# Qué imágenes funcionan mejor para IA 3D

Parte de: [[39-IA-Modelado-3D/05 Calidad y criterio/_Índice Calidad y criterio|Calidad y criterio]]

> [!info] Cómo avanzar
> `progreso`: **pendiente** → **viendo** (estudiando) → **visto** (contestaste la autoevaluación sin abrir las respuestas) → **dominado** (hiciste el laboratorio o lo aplicaste de verdad).

## Contenido

### Qué es
Es el criterio para elegir o preparar una imagen que dé un buen modelo 3D. Un modelo de imagen a 3D solo tiene una vista del objeto: todo lo que esté **ambiguo** en la imagen (dónde termina, qué tan profundo es, si hay un hueco) lo tendrá que **adivinar**. Las imágenes que funcionan bien son las que dejan poco por adivinar. Saberlo te ahorra cuota en la nube, tiempo de GPU y horas en Blender.

### Cómo funciona: qué "lee" el modelo
- **La silueta** define la forma exterior. Una silueta clara y completa es lo más importante.
- **El sombreado** indica volumen: dónde algo sobresale o se hunde. Por eso un dibujo de línea da formas planas.
- **La perspectiva** indica proporciones. Un gran angular cercano agranda lo que está cerca de la lente.
- **Los colores y bordes** separan partes. Si el asa de una taza es del mismo color que el fondo detrás de ella, el modelo no sabe que hay un hueco.

### Características que dan buen 3D
| Característica | Por qué |
|---|---|
| Un solo objeto, completo | Sin cortes ni objetos extra que se fundan |
| Fondo limpio o quitado | Nada de planos ni bultos extra |
| Luz difusa, sin sombras duras | Las sombras duras se leen como forma |
| Vista de tres cuartos | Frente, lado y algo de arriba: más información de volumen |
| Poca perspectiva | Proporciones reales (lente 2×–3× en el iPhone, 50–85 mm equivalentes en la a6700) |
| Silueta clara | Bordes definidos contra el fondo |
| Materiales mate | Sin reflejos que confundan |
| Resolución suficiente | Al menos 512×512 en el objeto, sin compresión fuerte |

### Objetos que suelen salir mal
- **Transparentes o reflejantes:** vidrio, acrílico, metal pulido, cerámica muy brillante. El modelo ve lo que hay detrás o el reflejo.
- **Muy delgados:** alambres, rejas, cabello suelto, antenas, ramas finas. Se pierden o salen como láminas gruesas.
- **Con huecos o interiores:** asas, aros, jaulas, objetos huecos. Los huecos se rellenan si no se ven claramente.
- **Con textos o logotipos pequeños:** salen como manchas; agrégalos en Blender o en el laminador.
- **Con simetría oculta:** objetos cuya espalda es muy distinta al frente (una mochila con correas).

### Ejemplo: la taza con el asa sólida
Tomaste una foto de frente a tu taza favorita y TripoSR te dio el asa como un bloque sin agujero. Pasó porque, desde ese ángulo, el hueco del asa casi no se ve y su interior era del mismo tono que la pared de fondo.
1. **Cambia el ángulo:** fotografía de tres cuartos con el asa de lado, para que se vea claramente el hueco.
2. **Aumenta el contraste:** fondo de cartulina de color distinto a la taza, visible a través del asa.
3. **Ilumina parejo:** luz de ventana indirecta, sin sombras dentro del asa.
4. **Si aún falla:** corrige en Blender con un Boolean *Difference* usando un cilindro o un toro con la forma del hueco ([[Modificador Boolean en Blender]]).

Regla general: **lo que no se ve en la imagen no aparece en el modelo.**

### Lista de revisión antes de generar
1. ¿Hay un solo objeto y está completo?
2. ¿El fondo es liso o ya lo quitaste?
3. ¿Se ve en tres cuartos?
4. ¿Los huecos importantes se ven claramente?
5. ¿Hay partes más delgadas de lo que se podrá imprimir? (Engrósalas en el diseño.)
6. ¿Es mate?

### Cuándo usarlo y cuándo no
- **Siempre** antes de gastar cuota en la nube o tiempo de GPU.
- Si el objeto es reflejante o muy delgado y **existe**, quizá el escaneo tampoco funcione bien; para piezas funcionales usa CAD ([[IA vs CAD vs escaneo (cuándo usar cada uno)]]).

### Errores comunes
1. **Usar la foto "bonita" y no la útil.** Una foto artística con sombras dramáticas da mal 3D.
2. **Fotografiar de frente perfecto.** Pierdes la profundidad; gira el objeto.
3. **Ignorar el fondo detrás de los huecos.** Si no contrasta, el hueco desaparece.
4. **Usar capturas de pantalla comprimidas.** Los artefactos JPEG se vuelven ruido en la malla.

### Para profundizar
- [TripoSR en GitHub (ejemplos de entrada)](https://github.com/VAST-AI-Research/TripoSR)
- [Space de TRELLIS (ejemplos de imágenes que funcionan)](https://huggingface.co/spaces/microsoft/TRELLIS)
- [Tarjeta de Hunyuan3D-2mini](https://huggingface.co/tencent/Hunyuan3D-2mini)

## Autoevaluación

Contesta primero en voz alta o en papel; luego despliega la respuesta.

> [!question]- ¿Qué características hacen que una imagen dé buen 3D?
> Objeto único y completo, fondo limpio, luz difusa, vista de tres cuartos, poca perspectiva (lente normal o tele, no gran angular), silueta clara y materiales mate.

> [!question]- ¿Qué tipo de objetos suelen salir mal?
> Transparentes o reflejantes (vidrio, metal pulido), muy delgados (alambres, cabello suelto, rejas), con huecos o interiores complejos, y textos o logotipos pequeños.

> [!question]- Caso: tu foto de una taza sale con el asa sólida (sin agujero). ¿Cómo lo mejoras?
> Toma la foto desde un ángulo donde se vea claramente el hueco del asa y con fondo que contraste; o corrige en Blender con un Boolean. Elementos que no se ven en la imagen no aparecen en el modelo.

## Práctica

- [[Lab 7 - Escaneo con iPhone vs IA del mismo objeto]]

