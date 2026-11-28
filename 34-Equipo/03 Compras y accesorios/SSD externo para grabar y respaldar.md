---
proyecto: Conocimiento
tipo: concepto
nivel: basico
progreso: pendiente
estado: pendiente
area: Equipo
tags: [equipo]
contenido: completo
---
# SSD externo para grabar y respaldar

Parte de: [[34-Equipo/03 Compras y accesorios/_Índice Compras y accesorios|Compras y accesorios]]

> [!info] Cómo avanzar
> `progreso`: **pendiente** → **viendo** (estudiando) → **visto** (contestaste la autoevaluación sin abrir las respuestas) → **dominado** (hiciste el laboratorio o lo aplicaste de verdad).

## Contenido

### Qué es
Un SSD externo es un disco de estado sólido portátil que conectas por USB-C. Cumple dos papeles en tu flujo: **grabar** directo desde el iPhone 15 Pro (necesario para ProRes 4K60 y útil para no llenar el teléfono) y **trabajar o respaldar** en la computadora. No es lo mismo un disco para grabar que uno para guardar: el primero necesita velocidad sostenida; el segundo, capacidad y confiabilidad.

### Cómo funciona
**Requisitos de Apple para grabar ProRes 4K60** en el iPhone 15 Pro ([Apple 109041](https://support.apple.com/en-us/109041)):

- Escritura de **al menos 220 MB/s**.
- Consumo de **máximo 4.5 W** (el iPhone lo alimenta).
- Formato **exFAT**; sin cifrado con contraseña.
- Cable **USB 3 de 10 Gb/s** o más (un cable de carga USB 2 no sirve).

El disco se puede formatear desde la app **Archivos** del iPhone. ¿Por qué 220 MB/s? ProRes 422 HQ 4K60 son unos 1768 Mb/s; entre 8, unos **221 MB/s**. Cualquier caída por debajo de eso corta la grabación.

**Velocidad anunciada contra velocidad sostenida.** Muchos SSD anuncian "hasta 1000 MB/s", pero escriben rápido solo mientras llenan una caché interna; al llenarse, o al calentarse, bajan mucho. Para grabar importa la escritura **sostenida**, que suele aparecer en reseñas, no en la caja.

**SSD contra disco mecánico (HDD).**

```mermaid
flowchart LR
  G[Grabar en iPhone] --> S1[SSD rápido y sostenido]
  S1 --> PC[Editar desde SSD]
  PC --> B1[Respaldo en HDD barato]
  PC --> B2[Copia fuera de casa / nube]
```

El SSD tiene acceso inmediato: la línea de tiempo responde al saltar entre clips. Un HDD tiene un brazo mecánico que busca los datos: sirve para **archivo y respaldo** barato, no para editar 4K.

**Regla 3-2-1.** Tres copias de tus archivos, en dos medios distintos, una fuera de casa. El SSD de trabajo **no es respaldo**: es una de las tres copias. Ver [[Respaldo 3-2-1 de fotos y videos]].

### Ejemplo
Vas a grabar un concierto pequeño en ProRes Log 4K60 con el iPhone:

1. Formateas el SSD en exFAT desde Archivos y conectas con un cable de 10 Gb/s (no el que trae el cargador).
2. Haces una prueba de 5 minutos continuos: si aparece "velocidad de grabación lenta", el disco no sostiene la escritura o el cable es USB 2.
3. Calculas espacio: 4K60 en ProRes 422 HQ ocupa ~13.3 GB por minuto; en 1 TB caben unos 75 minutos.
4. Al llegar a casa: copias el SSD a la PC (copia 1, de trabajo), a un HDD externo (copia 2) y subes lo esencial a la nube o lo guardas en otro lugar (copia 3). Solo después formateas el SSD.

Si el disco falla a los pocos minutos de grabar, revisa en este orden: cable, temperatura (déjalo enfriar o que no esté pegado al teléfono caliente) y escritura sostenida del modelo.

### Cuándo usarlo y cuándo no
- **Úsalo para grabar** si haces ProRes 4K60 o si tu iPhone es de poca capacidad.
- **Úsalo para editar** como disco de medios y caché de DaVinci Resolve.
- **No lo uses como única copia** de nada.
- **Para archivo de largo plazo**, un HDD grande es más económico por GB.

### Errores comunes
- **Elegir por la velocidad máxima anunciada** y no por la sostenida.
- **Cable USB 2**: el iPhone no alcanza los 220 MB/s.
- **Cifrar el disco con contraseña**: el iPhone no lo acepta para grabar.
- **Formatear antes de tener las tres copias**.
- **Llevar el SSD suelto en la mochila**: aunque no tenga partes móviles, el conector USB-C se daña. Usa una funda.

### Para profundizar
- [Apple: grabar video ProRes en el iPhone (requisitos del disco)](https://support.apple.com/en-us/109041)
- [Apple ProRes white paper (PDF)](https://www.apple.com/cl/final-cut-pro/docs/Apple_ProRes.pdf)

## Autoevaluación

Contesta primero en voz alta o en papel; luego despliega la respuesta.

> [!question]- ¿Qué características debe cumplir un SSD para grabar ProRes 4K60 desde el iPhone 15 Pro?
> Escritura sostenida **≥ 220 MB/s**, consumo **≤ 4.5 W**, formato **exFAT**, sin cifrado, y cable **USB 3 (10 Gb/s)**. Se formatea desde la app Archivos. Fuente: [Apple 109041](https://support.apple.com/en-us/109041).

> [!question]- ¿Por qué un SSD es mejor que un disco duro mecánico para editar?
> Lecturas mucho más rápidas y acceso inmediato: la línea de tiempo responde al saltar entre clips. Un HDD mecánico sirve para **respaldo** barato de archivo, no para editar 4K.

> [!question]- Caso: tu SSD externo graba bien unos minutos y luego aparece "velocidad de grabación lenta". ¿Qué pudo pasar?
> Muchos SSD bajan la velocidad cuando se llena su caché o se calientan (escritura sostenida menor a la anunciada), o el cable es USB 2. Prueba con cable de 10 Gb/s, deja enfriar y elige modelos con buena escritura **sostenida**, no solo "hasta X MB/s".

> [!question]- ¿Qué es la regla 3-2-1 de respaldo?
> **3 copias** de tus archivos, en **2 medios distintos**, y **1 fuera de casa** (nube u otro lugar). El SSD de trabajo no es respaldo. Ver [[Respaldo 3-2-1 de fotos y videos]].

## Práctica

- [[Lab 4 - Plan de almacenamiento y respaldo de mis grabaciones]]

