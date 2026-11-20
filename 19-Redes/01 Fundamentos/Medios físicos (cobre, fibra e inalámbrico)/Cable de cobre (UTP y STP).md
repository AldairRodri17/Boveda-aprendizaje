---
proyecto: Conocimiento
tipo: concepto
nivel: basico
progreso: pendiente
estado: pendiente
area: Redes
tags: [redes]
---
# Cable de cobre (UTP y STP)

Parte de: [[Medios físicos (cobre, fibra e inalámbrico)]]

> [!info] Cómo avanzar
> `progreso`: **pendiente** → **viendo** (estudiando) → **visto** (contestaste la autoevaluación sin abrir las respuestas) → **dominado** (hiciste el laboratorio o lo aplicaste de verdad).

## Contenido

### Qué es
El cable de cobre de **par trenzado** es el medio más común en las LAN. Tiene 8 hilos agrupados en 4 pares trenzados y termina en conectores RJ45. Hay dos variantes principales: **UTP** (Unshielded Twisted Pair, sin blindaje) y **STP** o sus variantes F/UTP y S/FTP (con blindaje de lámina o malla). Transporta Ethernet hasta **100 metros** por enlace y puede llevar energía con PoE.

### Cómo funciona
**Por qué se trenzan los pares**: cada par lleva una señal y su inversa. Al trenzarlos, el ruido externo afecta casi por igual a ambos hilos y el receptor, que mide la diferencia, lo cancela. El trenzado también reduce la **diafonía** (crosstalk): que la señal de un par "se cuele" en otro. Cada par tiene un paso de trenzado distinto para minimizarla.

**UTP frente a STP**:
- **UTP**: más barato, delgado, flexible y fácil de instalar. Suficiente en oficinas.
- **STP / F/UTP / S/FTP**: añade blindaje que protege de interferencia electromagnética. Útil en fábricas, cerca de motores, ascensores o equipos médicos. Exige conectores blindados y una buena conexión a tierra; un blindaje mal aterrizado puede empeorar el ruido.

**Categorías** (las más relevantes):

| Categoría | Ancho de banda | Uso típico |
|---|---|---|
| Cat 5e | 100 MHz | 1 Gbps a 100 m |
| Cat 6 | 250 MHz | 1 Gbps a 100 m; 10 Gbps hasta unos 55 m |
| Cat 6A | 500 MHz | 10 Gbps a 100 m |
| Cat 8 | 2000 MHz | 25/40 Gbps hasta 30 m (centros de datos) |

Para 10 Gbps a 100 m la elección es **Cat 6A**. Los estándares 2.5GBASE-T y 5GBASE-T permiten 2.5 y 5 Gbps sobre Cat 5e y Cat 6, útil para APs Wi-Fi 6.

**Distancia**: el canal Ethernet sobre cobre admite 100 m en total (90 m de cable fijo más cordones). Más allá, la señal se atenúa y aparecen errores. Para más distancia, fibra.

**PoE** (802.3af, at y bt) entrega energía por los mismos pares para teléfonos IP, cámaras y APs.

### Ejemplo
Vas a cablear tu estudio en casa, donde editas los videos del DJI Osmo Pocket y la a6700 y los subes a un NAS:
- Del router al switch y del switch a la PC y al NAS: **Cat 6** sobra para 1 Gbps y te deja margen para 2.5 Gbps en tramos cortos.
- Si en el futuro pones un switch de 10 Gbps para el NAS: tiende **Cat 6A** desde ahora en los tramos largos.
- Mantén el cable lejos del cableado eléctrico y no lo dobles en ángulos cerrados.

En un switch Cisco puedes verificar la velocidad negociada y, en modelos que lo soportan, diagnosticar el cable:

```text
SW1# show interfaces g0/5 status
Port      Name   Status       Vlan   Duplex  Speed  Type
Gi0/5     NAS    connected    10     a-full  a-1000 10/100/1000BaseTX
SW1# test cable-diagnostics tdr interface g0/5
SW1# show cable-diagnostics tdr interface g0/5
```

Si un puerto gigabit negocia solo 100 Mbps, sospecha de un par dañado: Fast Ethernet usa 2 pares y Gigabit necesita los 4.

### Cuándo usarlo y cuándo no
Usa cobre para conectar usuarios, impresoras, APs y cámaras dentro de 100 m, sobre todo si necesitas PoE. Prefiere fibra para backbone, distancias largas o entornos con mucha interferencia.

### Errores comunes
- **Comprar cable CCA** (aluminio recubierto de cobre): barato, pero con más atenuación y riesgo con PoE. Busca cobre sólido.
- **Destrenzar los pares más de lo necesario** al ponchar.
- **Usar STP sin aterrizar**: el blindaje se convierte en antena.
- **Asumir que Cat 6 da 10 Gbps a 100 m**: para eso es Cat 6A.

### Para profundizar
- [IEEE 802.3 Ethernet Working Group](https://www.ieee802.org/3/)
- [TIA: estándares de cableado](https://tiaonline.org/what-we-do/standards/)

## Autoevaluación

Contesta primero en voz alta o en papel; luego despliega la respuesta.

> [!question]- ¿Qué diferencia hay entre UTP y STP?
> **UTP**: pares trenzados sin blindaje, barato y flexible. **STP**: pares con blindaje metálico; resiste mejor la interferencia pero es más caro y debe aterrizarse bien.

> [!question]- ¿Cuál es la distancia máxima de un enlace Ethernet sobre cobre?
> **100 metros** (90 m de cableado horizontal + 10 m de cables de parcheo).

> [!question]- ¿Por qué los pares van trenzados?
> El trenzado cancela la interferencia electromagnética y la diafonía (*crosstalk*) entre pares.

> [!question]- ¿Qué categoría usarías para 10 Gbps a 100 m?
> **Cat 6A** (Cat 6 solo llega a 10 Gbps en distancias cortas, alrededor de 55 m).

