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
# Accesorios impresos para foto, video y dron

Parte de: [[37-Modelado-3D-Tecnico/04 Proyectos técnicos/_Índice Proyectos técnicos|Proyectos técnicos]]

> [!info] Cómo avanzar
> `progreso`: **pendiente** → **viendo** (estudiando) → **visto** (contestaste la autoevaluación sin abrir las respuestas) → **dominado** (hiciste el laboratorio o lo aplicaste de verdad).

## Contenido

### Qué es
Es uno de los usos más rentables de tu impresora: piezas pequeñas que resuelven problemas concretos de tu equipo de foto y video. Soportes de iPhone para tripié, clips para el [[DJI Mic Mini 2]], porta-tarjetas microSD, protectores para guardar el gimbal, bases de aterrizaje, adaptadores para la Osmo Pocket o tapas para la a6700. Combina casi todo lo del área: medir, holguras, tuercas atrapadas, orientación y material.

### Cómo funciona
**La rosca de tripié**: casi todo el equipo de foto usa tornillo **1/4"-20 UNC** (diámetro mayor de 6.35 mm, 20 hilos por pulgada, es decir paso de 1.27 mm); algunas rótulas y bases grandes usan 3/8"-16. En una pieza impresa no conviene imprimir esa rosca: queda débil y se barre. Las dos soluciones buenas:

- **Tuerca 1/4"-20 atrapada** en un alojamiento hexagonal (7/16" = 11.11 mm entre caras en la mayoría; mide la tuya). Ver [[Agujeros, roscas impresas, insertos y tuercas atrapadas]].
- **Inserto de latón 1/4"-20** puesto con cautín.

**Medir tu equipo**: mide **con la funda que realmente usas**; la diferencia entre un iPhone desnudo y con funda puede ser de 2–3 mm por lado. Usa las medidas oficiales del fabricante solo como referencia y valida siempre con una pieza de prueba.

```mermaid
flowchart LR
  A[Medir equipo con funda] --> B[Diseñar interfaz con holgura]
  B --> C[Imprimir solo la interfaz]
  C --> D{¿Encaja y sujeta?}
  D -- No --> B
  D -- Sí --> E[Pieza completa en PETG]
  E --> F[Probar carga y vibración]
```

**Material**: [[PETG]] es buena opción por defecto: más tenaz que el [[PLA]] y aguanta mejor el calor de un coche o del sol en una sesión al aire libre. Para clips que se abren mucho, PETG o [[TPU (filamento flexible)]].

**Dron**: con el [[DJI Mini 4K]] ten especial cuidado. Su gran ventaja es pesar menos de 250 g; cualquier accesorio que **vuele con él** (protectores de hélice impresos, soportes de luces, adaptadores) puede pasar ese peso, cambiar la categoría regulatoria y afectar la aerodinámica y la autonomía. Revisa [[NOM-107-SCT3-2019 y registro ante la AFAC]]. Accesorios seguros son los que **no vuelan**: estuche, protector del gimbal para transporte, porta-baterías, base de aterrizaje, porta-tarjetas.

### Ejemplo
Base de clip para el transmisor del DJI Mic Mini 2 que se atornilla en una rosca de tripié o zapata fría:

1. Mide el ancho del clip del transmisor y el grosor donde muerde.
2. Diseña una placa de 10 mm con alojamiento para tuerca 1/4"-20 abierto por arriba (la tensión del tornillo la empuja contra el piso de la placa).
3. Agrega una lengüeta de 2.1 mm de grosor (5 líneas) donde muerde el clip, con chaflán de entrada.
4. Imprime en PETG, acostado, 4 perímetros, 30 % gyroid.

Cálculo del hexágono para [[OpenSCAD]]:

```openscad
af = 11.11;      // 7/16" entre caras; mide tu tuerca
holgura = 0.2;
d_hex = (af + holgura) / cos(30);   // ≈ 13.06 mm entre vértices
alto = 10;
h_tuerca = 5.6;   // 7/32" aprox.; mide la tuya
difference() {
  cube([30, 30, alto]);
  translate([15, 15, -1]) cylinder(d = 6.6, h = alto + 2, $fn = 48);             // paso del tornillo 1/4"
  translate([15, 15, alto - h_tuerca - 0.2]) cylinder(d = d_hex, h = h_tuerca + 1, $fn = 6); // tuerca desde arriba
}
```

Fíjate en el sentido: el tornillo del tripié entra **por abajo** y, al apretar, jala la tuerca hacia abajo. Por eso el hexágono se abre por la cara **superior** y la tuerca queda apoyada sobre 4 mm de plástico. Si lo abrieras por abajo, la propia tensión la sacaría de su alojamiento.

### Práctica en tu Centauri 2
Haz el [[Lab 4 - Soporte de iPhone para tripié con tuerca atrapada]]. Antes, imprime solo la placa con el hexágono (10 minutos) y comprueba que la tuerca entra sin forzar y que el tornillo del tripié enrosca derecho. Luego diseña un porta-microSD de 8 ranuras para tus tarjetas del dron y la a6700.

### Cuándo usarlo y cuándo no
Accesorios de soporte, organización y transporte: sí. Piezas que sostienen equipo caro **por encima de la gente** o a gran altura (brazos para luces en el techo), con precaución: valida con el doble de carga. Nada que se monte en el dron en vuelo sin revisar peso y normativa. Para monturas de cámara con carga y palanca (jaulas, brazos largos), el metal sigue siendo mejor.

### Errores comunes
- **Medir sin funda** y luego no cabe el teléfono.
- **Rosca 1/4" impresa**: se barre a las pocas veces.
- **PLA en el coche**: se deforma con el calor.
- **Accesorios de vuelo que suben el peso del dron** por encima del límite.

### Para profundizar
- [Especificaciones del DJI Mini 4K (DJI)](https://www.dji.com/mini-4k/specs)
- [Página oficial de la Centauri 2 Combo (Elegoo)](https://us.elegoo.com/products/centauri-2-combo)

## Autoevaluación

Contesta primero en voz alta o en papel; luego despliega la respuesta.

> [!question]- ¿Qué rosca usan los tripiés y cómo la resuelves en una pieza impresa?
> Tornillo **1/4"-20 UNC**. En la pieza: tuerca 1/4"-20 **atrapada** en un alojamiento hexagonal, o inserto de latón; la rosca impresa de 1/4" queda débil.

> [!question]- ¿Qué cuidas si imprimes un accesorio para el [[DJI Mini 4K]]?
> Que **no vuele con él** si suma peso: el dron pesa casi el límite de 250 g y cualquier accesorio puede pasarlo (ver [[NOM-107-SCT3-2019 y registro ante la AFAC]]). Accesorios seguros: estuche, protector del gimbal para guardar, porta-tarjetas, base de aterrizaje.

> [!question]- Ideas útiles con tu equipo y cómo medirlas.
> Soporte de iPhone para tripié ([[iPhone 15 Pro para foto y video]], [[Tripiés y soportes]]), clip o base para el [[DJI Mic Mini 2]], porta-microSD. Mide con la funda que realmente usas; consulta las medidas oficiales del fabricante como referencia y confírmalas con una pieza de prueba.

## Práctica

- [[Lab 4 - Soporte de iPhone para tripié con tuerca atrapada]]

