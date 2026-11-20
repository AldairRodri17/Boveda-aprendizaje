---
proyecto: Conocimiento
tipo: concepto
nivel: basico
progreso: pendiente
estado: pendiente
area: Redes
tags: [redes]
---
# Cableado estructurado y estándares (T568A y T568B)

Parte de: [[19-Redes/01 Fundamentos/_Índice Fundamentos|Fundamentos]]

> [!info] Cómo avanzar
> `progreso`: **pendiente** → **viendo** (estudiando) → **visto** (contestaste la autoevaluación sin abrir las respuestas) → **dominado** (hiciste el laboratorio o lo aplicaste de verdad).

## Contenido

### Qué es
El **cableado estructurado** es un sistema de cableado diseñado con normas (principalmente ANSI/TIA-568 en América e ISO/IEC 11801 a nivel internacional) para que la infraestructura de un edificio sea ordenada, documentada, reemplazable y soporte varias generaciones de equipos. En lugar de tender cables "de la PC al switch" como salga, se definen espacios, tramos, distancias y etiquetas. **T568A** y **T568B** son los dos esquemas de asignación de colores a los 8 pines de un conector RJ45.

### Cómo funciona
Componentes de un sistema estructurado:
- **Cuarto de telecomunicaciones (IDF)** por piso, con racks, paneles de parcheo y switches de acceso.
- **Cableado horizontal**: del panel de parcheo a la roseta de pared; máximo **90 m** de cable fijo, más hasta **10 m** de cordones de parcheo, para un canal total de 100 m.
- **Cableado vertical (backbone)**: une los cuartos de cada piso con el cuarto principal (MDF), normalmente con fibra.
- **Área de trabajo**: la roseta y el cordón hasta el equipo.
- **Etiquetado y documentación**: cada punto con un identificador que coincide en roseta, panel y plano.

Orden de colores, pin 1 a pin 8:

| Pin | T568A | T568B |
|---|---|---|
| 1 | blanco-verde | blanco-naranja |
| 2 | verde | naranja |
| 3 | blanco-naranja | blanco-verde |
| 4 | azul | azul |
| 5 | blanco-azul | blanco-azul |
| 6 | naranja | verde |
| 7 | blanco-café | blanco-café |
| 8 | café | café |

La diferencia es solo que se intercambian los pares naranja y verde. Eléctricamente rinden igual.

- **Cable directo** (straight-through): ambos extremos con el mismo esquema (B-B o A-A). Une equipos distintos: PC a switch, router a switch.
- **Cable cruzado** (crossover): un extremo A y el otro B. Históricamente unía equipos iguales: switch a switch, PC a PC, router a router. Hoy casi todos los puertos tienen **Auto-MDIX**, que detecta y ajusta, así que el directo funciona en casi todo.
- **Cable de consola** (rollover): para conectarte al puerto de consola de un equipo Cisco; hoy es común el adaptador USB-serial o el puerto USB de consola.

### Ejemplo
Vas a cablear una pequeña oficina de 12 puestos para tu equipo de desarrollo:
1. Elige un esquema para todo el edificio (en México lo más común es **T568B**) y documéntalo.
2. Tiende cable Cat 6 desde cada roseta hasta un panel de parcheo de 24 puertos en el rack; ningún tramo fijo pasa de 90 m.
3. Etiqueta: roseta `P1-07` = puerto 7 del panel = `Fa0/7` del switch.
4. Certifica o al menos prueba cada punto con un probador de continuidad que muestre los 8 hilos en orden.
5. Usa cordones de parcheo cortos del panel al switch.

```text
SW-PISO1(config)# interface fa0/7
SW-PISO1(config-if)# description P1-07 Escritorio Ana
```

Así, cuando alguien diga "no tengo red en el lugar 7", sabes exactamente qué puerto revisar.

### Cuándo usarlo y cuándo no
Siempre que se cablee una instalación permanente (oficinas, escuelas, naves industriales) conviene cableado estructurado: el costo inicial es mayor, pero cada cambio posterior es barato. Para una prueba rápida en casa o un laboratorio temporal puedes usar cordones prefabricados sin paneles, pero respeta igualmente la categoría y las distancias.

### Errores comunes
- **Mezclar T568A y T568B en la instalación**: un tramo con A en la roseta y B en el panel se vuelve un cruzado; con Auto-MDIX "funciona", pero confunde al diagnosticar y puede romper equipos sin Auto-MDIX o PoE de cuatro pares.
- **Destrenzar demasiado los pares** al ponchar: degrada la diafonía y el enlace negocia a menor velocidad. Mantén el trenzado hasta unos milímetros del conector.
- **Superar los 100 m** de canal: el enlace sube a ratos o con errores CRC.
- **No etiquetar**: el rack se vuelve un "espagueti" imposible de mantener.

### Para profundizar
- [TIA: estándares de telecomunicaciones (TIA-568)](https://tiaonline.org/what-we-do/standards/)
- [IEEE 802.3 Ethernet Working Group](https://www.ieee802.org/3/)

## Autoevaluación

Contesta primero en voz alta o en papel; luego despliega la respuesta.

> [!question]- ¿Qué es el cableado estructurado?
> Un diseño estandarizado (ANSI/TIA-568, ISO/IEC 11801) de cableado del edificio: cuarto de telecomunicaciones, backbone, cableado horizontal, áreas de trabajo, etiquetado y pruebas.

> [!question]- ¿Cuál es el orden de colores de T568B?
> Blanco-naranja, naranja, blanco-verde, azul, blanco-azul, verde, blanco-café, café.

> [!question]- ¿Qué diferencia hay entre un cable directo y uno cruzado?
> **Directo**: mismo estándar en ambos extremos (B-B), para PC-switch. **Cruzado**: A en un extremo y B en el otro, tradicionalmente para equipos iguales (switch-switch, PC-PC). Hoy Auto-MDIX lo resuelve en la mayoría de los puertos.

> [!question]- Error común: mezclar T568A y T568B en un mismo edificio. ¿Qué problema causa?
> Si los patch panels están en un estándar y los jacks en otro, se crean cables cruzados por accidente y se dificulta el mantenimiento. Elige uno y úsalo en todo el sitio.

