---
proyecto: Conocimiento
tipo: concepto
nivel: intermedio
progreso: pendiente
estado: pendiente
area: Edición de video
tags: [edicion-video]
---
# Nodos de color en DaVinci Resolve

Parte de: [[33-Edicion-Video/04 Color/_Índice Color|Color]]

> [!info] Cómo avanzar
> `progreso`: **pendiente** → **viendo** (estudiando) → **visto** (contestaste la autoevaluación sin abrir las respuestas) → **dominado** (hiciste el laboratorio o lo aplicaste de verdad).

## Contenido

### Qué es
En la página **Color** de Resolve, cada corrección vive en un **nodo**: una caja que recibe la imagen, le aplica ajustes (ruedas, curvas, ventanas, LUTs, efectos) y la pasa al siguiente. El conjunto de nodos de un clip forma un **árbol** que se procesa en orden de izquierda a derecha. Es como tener capas de ajuste, pero con más control sobre el orden y sobre qué afecta a qué.

### Cómo funciona
- **Nodo en serie (Serial)**: recibe el resultado del anterior. Es el más usado.
- **Nodo paralelo (Parallel)**: dos nodos reciben la misma imagen y sus resultados se mezclan; útil para correcciones independientes.
- **Nodo de capa (Layer)**: como paralelo, pero el inferior tiene prioridad; útil para combinar ajustes con prioridad.
- Cada nodo tiene entrada RGB (verde), salida y entrada de **key** (azul) para máscaras.

Atajos: **Alt+S** agrega un nodo en serie después del seleccionado; **Alt+P** un nodo paralelo; **Alt+L** uno de capa; **Ctrl+D** desactiva/activa un nodo (para comparar antes y después). También: clic derecho en el editor de nodos → *Add Node → Add Serial*. Ponle nombre a cada nodo (clic derecho → *Node Label*).

```mermaid
flowchart LR
    I[Entrada] --> N1[01 Balance y exposición]
    N1 --> N2[02 CST / LUT técnica]
    N2 --> N3[03 Contraste]
    N3 --> N4[04 Piel: qualifier]
    N4 --> N5[05 Look]
    N5 --> N6[06 Viñeta]
    N6 --> O[Salida]
```

### Estructura sencilla para empezar
1. **Balance/Exposición**: Offset, temperatura y tinte.
2. **Conversión** (si es Log): Color Space Transform o LUT técnica ([[Material Log y LUTs]]).
3. **Contraste**: curvas o Contrast/Pivot.
4. **Secundario de piel**: Qualifier + ajuste suave ([[Igualar tomas y tonos de piel]]).
5. **Look**: LUT creativa o gradación manual.
6. **Viñeta o ventana**: Power Window para dirigir la mirada ([[Máscaras y tracking en video]]).
Guárdala como plantilla: haz clic derecho en el visor → *Grab Still* y desde la *Gallery* aplícala a otros clips; o copia el árbol de un clip a otro con clic medio.

Además del árbol de cada clip, Resolve tiene **nodos de grupo** (pre-clip y post-clip) y del **timeline**, para aplicar el mismo look a varias tomas a la vez.

### Ejemplo
Entrevista con a6700 (S-Log3 convertido a DNxHR) e iPhone (Apple Log): mismo árbol en todos los clips, con el nodo 2 configurado para la cámara correspondiente. El nodo 5 (look) lo pones en un **grupo** "Entrevista" en *Post-Clip*, así cambias el look de todas las tomas en un solo lugar.

### Cuándo usar varios nodos
Siempre que hagas más de un ajuste. Un solo nodo está bien para una corrección rápida de una toma suelta.

### Errores comunes
- **Hacer todo en un solo nodo**: no puedes desactivar o ajustar una parte sin tocar las demás, el orden interno de operaciones (por ejemplo, la LUT va al final dentro del nodo) puede jugar en tu contra, y no podrás copiar solo el look a otras tomas. Un nodo, una tarea.
- **Nodos sin nombre**: en una semana no sabes qué hace cada uno.
- **Look antes que balance**: amplifica errores.
- **Copiar el árbol completo entre cámaras distintas** sin cambiar el nodo de conversión.

### Para profundizar
- [Página Color de DaVinci Resolve](https://www.blackmagicdesign.com/products/davinciresolve/color)
- [Capacitación oficial (guía del colorista)](https://www.blackmagicdesign.com/products/davinciresolve/training)

## Autoevaluación

Contesta primero en voz alta o en papel; luego despliega la respuesta.

> [!question]- ¿Qué es un nodo en la página Color?
> Un paso de corrección independiente; los nodos se conectan en cadena y cada uno hace un ajuste (exposición, balance, LUT, look).

> [!question]- ¿Qué estructura de nodos sencilla puedes usar?
> 1) Exposición/balance, 2) Saturación/contraste, 3) Conversión (LUT o CST), 4) Look creativo. Pon nombre a cada nodo.

> [!question]- ¿Cómo agregas un nodo en serie?
> Con **Alt+S** (Windows), o clic derecho en el editor de nodos > Add Node > Add Serial.

> [!question]- Error común: hacer todo en un solo nodo. ¿Qué problema tiene?
> No puedes apagar ni ajustar una parte sin afectar las demás; separar ajustes facilita comparar y corregir.

## Práctica

- [[Lab 5 - Corrección y gradación de Apple Log con scopes]]

