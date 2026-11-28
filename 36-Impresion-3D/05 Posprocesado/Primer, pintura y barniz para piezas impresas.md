---
proyecto: Conocimiento
tipo: concepto
nivel: basico
progreso: pendiente
estado: pendiente
area: Impresión 3D
tags: [impresion-3d]
contenido: completo
---
# Primer, pintura y barniz para piezas impresas

Parte de: [[36-Impresion-3D/05 Posprocesado/_Índice Posprocesado|Posprocesado]]

> [!info] Cómo avanzar
> `progreso`: **pendiente** → **viendo** (estudiando) → **visto** (contestaste la autoevaluación sin abrir las respuestas) → **dominado** (hiciste el laboratorio o lo aplicaste de verdad).

## Contenido

### Qué es
Pintar una pieza impresa sigue tres capas: **primer** (imprimación) para preparar la superficie, **pintura** para el color y **barniz** para proteger y definir el acabado. Bien hecho, el resultado parece una figura de colección; mal hecho, la pintura se descarapela o resalta cada línea de capa.

### Cómo funciona
**Primer.** Es una capa base que:
- Se **adhiere** bien al plástico y a la vez da una superficie a la que la pintura se pega mejor.
- **Uniformiza** el color (pintar amarillo sobre PLA negro sin primer no cubre).
- **Revela imperfecciones**: con un color neutro (gris) ves líneas y rayas que no veías.
- El **primer de relleno** además rellena líneas finas (ver [[Lijado y relleno de piezas impresas]]).

**Pintura.**

| Tipo | Ventajas | Uso |
|---|---|---|
| Acrílicos de modelismo (base agua) | Poco olor, se limpian con agua, gran variedad | Pincel o aerógrafo; figuras y detalles |
| Aerosol | Cubre rápido grandes superficies | Piezas grandes, color base |
| Esmaltes / solventes | Duros y resistentes | Con buena ventilación; menos recomendados para empezar |

Regla de oro: **capas delgadas**, dejando secar entre una y otra. Varias capas finas cubren mejor y no tapan detalles.

**Barniz.** Protege la pintura del roce y la luz, y define el acabado: **mate**, **satinado** o **brillante**. Se aplica al final en capas finas. Puedes combinar: mate general y brillante solo en ojos o metal.

```mermaid
flowchart LR
  A[Pieza lijada y limpia] --> B[Primer en capas finas]
  B --> C[Lijar fino si hace falta]
  C --> D[Color base]
  D --> E[Detalles, sombras y luces]
  E --> F[Barniz]
```

### Ejemplo
Una figura de PLA para tu escritorio:
1. Lavas con agua y jabón para quitar grasa y polvo; secas.
2. Primer gris en aerosol **al aire libre**, a 20–30 cm, en pasadas cortas; dos capas finas.
3. Color base con acrílicos y pincel o aerógrafo; capas diluidas.
4. Lavado (*wash*) oscuro para sombras y pincel seco (*drybrush*) para luces.
5. Barniz mate en aerosol, otra vez afuera.
6. Dejas curar el tiempo que indique el fabricante antes de manipular mucho.

Fotografías el resultado con la Sony a6700 y luz suave: el barniz mate evita reflejos.

### Cuándo pintar y cuándo no
- **Pintar**: figuras, cosplay, prototipos de presentación, piezas que deben verse "terminadas".
- **No hace falta**: piezas funcionales o multicolor con el CANVAS, que ya salen con color.
- Para piezas que se manipulan mucho, el barniz es obligatorio.

### Seguridad
Los aerosoles (primer, pintura y barniz) liberan solventes y gotas finas. Úsalos **al aire libre** o con muy buena ventilación, lejos de flamas y de la impresora; con mucho volumen, usa respirador con cartuchos para vapores orgánicos. Lee la etiqueta de cada producto.

### Errores comunes
1. **Aerosoles en un cuarto cerrado.** Vapores y pintura en todo el cuarto; pinta afuera o con extracción.
2. **Capas gruesas**: escurren, tapan detalles y tardan en secar.
3. **Pintar sobre grasa o polvo**: la pintura se descarapela.
4. **Saltarse el primer**: mala adhesión y colores que no cubren.
5. **Barnizar sin dejar secar** la pintura: se arruga o queda pegajoso.

### Para profundizar
- Laboratorio: [[Lab 7 - Posprocesado soportes, lijado, primer y pintura]]
- Relacionado: [[Seguridad al imprimir (ventilación, emisiones y ubicación)]]
- [UL Chemical Insights — boletín sobre impresoras 3D y aire interior (PDF)](https://chemicalinsights.org/wp-content/uploads/2023/06/TB-540_3D-Printers_fin.pdf)

## Autoevaluación

Contesta primero en voz alta o en papel; luego despliega la respuesta.

> [!question]- ¿Para qué sirve el primer?
> Da una base uniforme que se adhiere al plástico y a la que la pintura se adhiere mejor; además revela imperfecciones antes de pintar.

> [!question]- ¿Qué pintura usar?
> Acrílicos de modelismo con pincel o aerógrafo, o aerosol para cubrir grande. Capas delgadas, dejando secar entre capas.

> [!question]- ¿Por qué barnizar al final?
> Protege la pintura del roce y define el acabado (mate, satinado o brillante).

> [!question]- Error común: aerosoles en un cuarto cerrado.
> Píntalos al aire libre o con buena ventilación y cubrebocas para vapores orgánicos si es mucho.

## Práctica

- [[Lab 7 - Posprocesado soportes, lijado, primer y pintura]]

