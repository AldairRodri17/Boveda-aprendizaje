---
proyecto: Conocimiento
tipo: concepto
nivel: intermedio
progreso: pendiente
estado: pendiente
area: Fotografía
tags: [fotografia]
contenido: completo
---
# Distancia hiperfocal

Parte de: [[30-Fotografia/03 Enfoque/_Índice Enfoque|Enfoque]]

> [!info] Cómo avanzar
> `progreso`: **pendiente** → **viendo** (estudiando) → **visto** (contestaste la autoevaluación sin abrir las respuestas) → **dominado** (hiciste el laboratorio o lo aplicaste de verdad).

## Contenido

### Qué es
La **distancia hiperfocal** (H) es la distancia a la que debes enfocar, con una focal y apertura dadas, para que la nitidez aceptable vaya **desde la mitad de esa distancia hasta el infinito**. Es la forma de exprimir al máximo la profundidad de campo en paisaje, calle y arquitectura sin recurrir al apilado.

### Cómo funciona
Todo depende del **círculo de confusión** (c): el tamaño máximo que puede tener un punto borroso en el sensor para que, al ver la foto a un tamaño normal, todavía lo percibas como un punto nítido. Como un sensor pequeño se amplía más para llegar al mismo tamaño de impresión, su c es más pequeño. Valores habituales: ≈ 0.03 mm en formato completo, ≈ 0.02 mm en APS-C (a6700) y bastante menos en el celular (del orden de 0.008 mm en la cámara principal del iPhone).

Fórmula:

**H ≈ f² / (N · c) + f**, con f = focal real en mm, N = número f, c = círculo de confusión en mm.

```python
def hiperfocal(f, N, c):
    return f * f / (N * c) + f   # resultado en mm

print(hiperfocal(16, 8, 0.02))   # 1616 mm ≈ 1.6 m
print(hiperfocal(24, 11, 0.02))  # ≈ 2642 mm ≈ 2.6 m
print(hiperfocal(16, 16, 0.02))  # 816 mm ≈ 0.8 m
```

Lectura:
- a6700 con **16 mm a f/8**: H ≈ **1.6 m**. Enfocando ahí, todo está nítido desde ≈ 0.8 m hasta el infinito.
- **24 mm a f/11**: H ≈ **2.6 m**; nitidez desde ≈ 1.3 m.
- Si cierras de f/8 a **f/16** con el 16 mm, H **se acerca** a ≈ 0.8 m (más profundidad de campo), pero pagas con difracción. Ver [[Apertura y profundidad de campo]].

```mermaid
flowchart LR
  C[Cámara] --- M[H/2: inicia la nitidez] --- H[H: punto de enfoque] --- I[Infinito: sigue nítido]
```

**¿Por qué en el iPhone casi no te preocupa?** Su focal real es de pocos milímetros: con la principal (≈ 6.9 mm a f/1.78) la hiperfocal queda alrededor de 3 m, y con la ultra gran angular mucho más cerca. Además el AF hace el trabajo y el procesamiento ayuda. Solo importa en primeros planos muy cercanos.

### Ejemplo
**Paisaje con una roca a 1 m y montañas al fondo, a6700 con 16 mm:**
1. Tripié, modo A a f/8, ISO 100.
2. Calcula H ≈ 1.6 m (o llévalo apuntado en una tabla pequeña en el celular).
3. Enfoca en MF algo que esté a ~1.6 m (mide con pasos: un paso ≈ 0.7–0.8 m). Usa la escala de distancia en pantalla si tu lente la muestra.
4. Dispara y revisa ampliado: la roca a 1 m y la montaña deben verse nítidas. Si la roca está a 0.6 m, ya queda fuera (H/2 = 0.8 m): cierra a f/11 o aplica [[Focus stacking (apilado de enfoque)]].

Ejercicio: haz la misma foto enfocando (a) la roca, (b) el infinito y (c) la hiperfocal. Compara las tres al 100 % en primer plano y fondo.

### Cuándo usarlo y cuándo no
- **Sí**: paisaje con primer plano, calle con enfoque "por zona" (preenfocas a 2–3 m y disparas rápido), astrofotografía de paisaje con primer plano lejano, arquitectura.
- **No**: retrato o cuando quieres fondo desenfocado; macro (la PdC es de milímetros); cuando el primer plano está más cerca de H/2.
- **Alternativa moderna**: si dudas, enfoca un poco **más allá** de la hiperfocal; perder un poco de nitidez en el primer plano suele molestar menos que un infinito suave.

### Errores comunes
1. **Usar la focal equivalente en la fórmula**: va la focal **real** (16 mm, no 24 mm), con el c correspondiente al sensor.
2. **Tomar el "nítido aceptable" como nitidez máxima**: los extremos de la zona están en el límite; para impresiones grandes usa un c más pequeño (más exigente).
3. **Cerrar a f/22 para acercar H**: ganas PdC pero pierdes nitidez general por difracción.
4. **Calcular perfecto y enfocar al ojo**: estimar mal la distancia arruina el cálculo; apóyate en la escala del lente o en ampliación.

### Para profundizar
- [Understanding the Hyperfocal Distance (Cambridge in Colour)](https://www.cambridgeincolour.com/tutorials/hyperfocal-distance.htm)
- [Understanding Depth of Field (Cambridge in Colour)](https://www.cambridgeincolour.com/tutorials/depth-of-field.htm)

## Ejercicios

Resuélvelos en papel o en la herramienta; la solución está plegada.

> [!question]- Fórmula: H ≈ f² / (N · c) + f. Calcula H para la a6700 con 16 mm, f/8 y c = 0.02 mm.
> 16² / (8 × 0.02) + 16 = 256 / 0.16 + 16 = 1600 + 16 = **≈1.6 m**. Nitidez desde ≈0.8 m hasta infinito.

> [!question]- Mismo cálculo para 24 mm a f/11.
> 576 / 0.22 + 24 ≈ 2618 + 24 = **≈2.6 m** (nítido desde ≈1.3 m).

> [!question]- Si cierras de f/8 a f/16 con el 16 mm, ¿la hiperfocal se acerca o se aleja?
> Se **acerca** a la mitad aproximadamente (≈0.8 m), porque N está en el denominador. Pero a f/16 empiezas a perder nitidez por difracción.

## Autoevaluación

Contesta primero en voz alta o en papel; luego despliega la respuesta.

> [!question]- ¿Qué es la distancia hiperfocal?
> La distancia de enfoque que da la mayor profundidad de campo: si enfocas ahí, todo desde la mitad de esa distancia hasta el infinito se ve aceptablemente nítido.

> [!question]- ¿Qué es el círculo de confusión y por qué cambia con el sensor?
> El tamaño máximo de un punto borroso que todavía se ve nítido en una copia normal. Se usa ≈0.03 mm en formato completo y ≈**0.02 mm** en APS-C.

> [!question]- ¿Por qué en el iPhone casi nunca te preocupas por la hiperfocal?
> Con focales reales tan cortas, la hiperfocal está a muy poca distancia: casi todo ya sale nítido.

