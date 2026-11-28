---
proyecto: Conocimiento
tipo: concepto
nivel: intermedio
progreso: pendiente
estado: pendiente
area: Modelado 3D técnico
tags: [modelado-3d-tecnico]
contenido: completo
---
# Medir piezas e ingeniería inversa

Parte de: [[37-Modelado-3D-Tecnico/03 Diseño para impresión 3D/_Índice Diseño para impresión 3D|Diseño para impresión 3D]]

> [!info] Cómo avanzar
> `progreso`: **pendiente** → **viendo** (estudiando) → **visto** (contestaste la autoevaluación sin abrir las respuestas) → **dominado** (hiciste el laboratorio o lo aplicaste de verdad).

## Contenido

### Qué es
**Ingeniería inversa** es reconstruir en CAD una pieza que ya existe (para repararla, copiarla o hacer algo que encaje con ella) a partir de mediciones. El reto es que medir bien es más difícil de lo que parece y que un error de 0.3 mm basta para que un encaje no funcione. Este tema cubre cómo medir con y sin [[Calibrador digital (vernier)]], cómo calcar contornos desde una foto y cómo validar antes de imprimir la pieza completa.

### Cómo funciona
**Herramientas, de más a menos precisa**:

1. **Calibrador digital**: ±0.02–0.05 mm en manos cuidadosas. Es la mejor primera compra para este tema.
2. **Regla metálica o escuadra**: ±0.5 mm leyendo con cuidado.
3. **Trucos sin calibrador**:
   - **Diámetros**: rodea el tubo con una tira de papel, marca dónde se cruza, mide el largo y divide entre π.
   - **Grosores**: apila varias piezas iguales y divide (10 tarjetas = 7.5 mm → 0.75 mm cada una).
   - **Galgas impresas**: imprime una placa con agujeros o ranuras de varias medidas y prueba cuál encaja.
4. **App Medición del iPhone**: útil para muebles y cuartos, pero con errores de varios milímetros; no sirve para holguras de décimas.

**Calcar desde una foto**:

```mermaid
flowchart LR
  A[Pieza plana + regla en el mismo plano] --> B[Foto perpendicular, lejos y con zoom]
  B --> C[Insertar imagen en el boceto]
  C --> D[Escalar con dos marcas de la regla]
  D --> E[Dibujar encima y restringir]
  E --> F[Imprimir solo la zona que encaja]
```

Con el [[iPhone 15 Pro para foto y video]], usa el teleobjetivo (3× o 5×) desde más lejos: reduce la distorsión por perspectiva. La regla debe estar **en el mismo plano** que el contorno que vas a calcar; si está más arriba o más abajo, la escala sale mal. Para escalar: si 100 mm de regla miden 812 px, cada pixel vale 100/812 mm.

Dónde insertar la foto: Onshape (*Insert image* en el boceto), Fusion (*Insert → Canvas*), FreeCAD (workbench *Image*).

**Interpretar las medidas**: las piezas se diseñaron con valores "redondos". Si mides 7.9 mm, probablemente era 8 mm con desgaste o error de medición; si mides 1/4" (6.35 mm), es una medida imperial. Anota cada medida con dos o tres lecturas y en qué punto la tomaste.

### Ejemplo
Reponer la tapa de la batería de un control remoto:

1. Mide largo, ancho y grosor de la abertura con calibrador en tres puntos; usa el mínimo.
2. Foto cenital con regla junto a la abertura para el contorno de la pestaña.
3. En Onshape, inserta la foto, escálala y calca la pestaña con arcos tangentes.
4. Aplica holgura deslizante de 0.2 por lado al contorno exterior de la tapa.
5. Imprime **solo una lámina de 1 mm** con el contorno y la pestaña (10 minutos) y pruébala.
6. Corrige y después imprime la tapa completa.

### Práctica en tu Centauri 2
Imprime una **galga de ranuras** de 0.8 a 1.4 mm en pasos de 0.1 y otra de agujeros de Ø3 a Ø8 en pasos de 0.2. Úsalas para "medir" tarjetas, cables y tornillos de tu escritorio sin calibrador; comprueba después contra el calibrador para saber cuánto confiar en el método.

### Cuándo usarlo y cuándo no
Para repuestos y adaptadores de piezas simples (tapas, perillas, clips, soportes), funciona muy bien. Para formas orgánicas complejas, considera escanear (ver el área de [[Fotogrametría y escaneo con iPhone]]), aunque el escaneo de un iPhone no tiene precisión de encaje. No intentes copiar piezas de seguridad o con carga crítica.

### Errores comunes
- **Tomar las medidas como exactas**: redondea a valores de diseño lógicos y valida con una pieza de prueba recortada.
- **Foto en ángulo o regla fuera del plano**: la escala queda mal en un borde y bien en otro.
- **Medir una pieza desgastada o deformada**: si está rota, mide la contraparte (el hueco donde entra).
- **Imprimir la pieza completa para probar**: horas perdidas por un error de 0.3 mm. Imprime solo la interfaz.

### Para profundizar
- [Insertar imágenes en bocetos (Image Workbench, wiki de FreeCAD)](https://wiki.freecad.org/Image_Workbench)
- [Onshape Learning Center](https://learn.onshape.com/)

## Ejercicios

Resuélvelos en papel o en la herramienta; la solución está plegada.

> [!question]- Mides con una tira de papel el perímetro de un tubo: 78.5 mm. ¿Diámetro?
> 78.5 / π = **24.99 ≈ 25 mm**.

> [!question]- Apilas 10 tarjetas iguales y miden 7.5 mm con regla. ¿Grosor de cada una y qué ranura diseñas para que entren con holgura deslizante?
> 0.75 mm cada una. Ranura = 0.75 + 2·0.2 = **1.15 mm** (pruébala: con ranuras tan delgadas mejor hacer una galga de 1.0/1.1/1.2).

> [!question]- En una foto, dos marcas de la regla separadas 100 mm miden 812 px y la pieza mide 455 px. ¿Cuánto mide la pieza?
> 455 · 100/812 = **56.0 mm** (solo si la pieza está en el mismo plano que la regla).

## Autoevaluación

Contesta primero en voz alta o en papel; luego despliega la respuesta.

> [!question]- ¿Cómo mides sin calibrador?
> Con regla metálica o escuadra de buena calidad (±0.5 mm), midiendo **varias veces** y en varios puntos; para diámetros, mide el perímetro con una tira de papel y divide entre π; para grosores, apila varias piezas iguales y divide. Para encajes, imprime **galgas** con varias medidas y prueba. Un [[Calibrador digital (vernier)]] es la mejor primera compra para este tema.

> [!question]- ¿Por qué no usar la app de Medición del iPhone para un encaje?
> Su precisión es de orden de centímetros/milímetros grandes, útil para muebles, no para holguras de décimas. Para contornos, mejor una **foto cenital** con una regla junto a la pieza y calcarla en el CAD.

> [!question]- ¿Cómo calcas un contorno desde una foto en el CAD?
> Foto perpendicular (lo más lejos posible con zoom para reducir perspectiva) con regla en el mismo plano; insértala en el boceto (Onshape: *Insert image*; Fusion: *Canvas*; FreeCAD: *Image* workbench), escálala usando dos marcas de la regla y dibuja encima.

> [!question]- Error común en ingeniería inversa. ¿Cuál?
> Copiar medidas "medidas" como si fueran exactas. Redondea a valores lógicos de diseño (un agujero de 7.9 es probablemente 8), imprime una **pieza de prueba recortada** (solo la zona que encaja) y ajusta.

## Práctica

- [[Lab 4 - Soporte de iPhone para tripié con tuerca atrapada]]
- [[Lab 5 - Reparar o adaptar una pieza rota (ingeniería inversa)]]

