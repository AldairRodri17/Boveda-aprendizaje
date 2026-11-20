---
proyecto: Conocimiento
tipo: concepto
nivel: basico
progreso: pendiente
estado: pendiente
area: Redes
tags: [redes]
---
# Fibra óptica (monomodo y multimodo)

Parte de: [[Medios físicos (cobre, fibra e inalámbrico)]]

> [!info] Cómo avanzar
> `progreso`: **pendiente** → **viendo** (estudiando) → **visto** (contestaste la autoevaluación sin abrir las respuestas) → **dominado** (hiciste el laboratorio o lo aplicaste de verdad).

## Contenido

### Qué es
La **fibra óptica** transmite datos como pulsos de luz a través de un hilo de vidrio (o plástico) muy delgado. El núcleo está rodeado por un revestimiento con menor índice de refracción, lo que hace que la luz rebote dentro del núcleo por **reflexión interna total**. Hay dos tipos: **monomodo (SMF)**, para grandes distancias, y **multimodo (MMF)**, para distancias cortas dentro de edificios y centros de datos.

### Cómo funciona
| Característica | Monomodo (SMF) | Multimodo (MMF) |
|---|---|---|
| Diámetro del núcleo | Unos 9 µm | 50 µm (OM2 a OM5) o 62.5 µm (OM1) |
| Fuente de luz | Láser | LED o láser VCSEL |
| Modos de luz | Uno solo, viaja casi en línea recta | Muchos caminos (modos) |
| Distancia | Kilómetros a decenas de kilómetros | Cientos de metros |
| Color de chaqueta habitual | Amarillo | Naranja (OM1/OM2), aguamarina (OM3/OM4), verde lima (OM5) |
| Costo | Cable similar, transceptores más caros | Transceptores más baratos |

En multimodo, los distintos modos llegan en tiempos ligeramente distintos (**dispersión modal**), lo que limita la distancia a altas velocidades. En monomodo esa dispersión casi no existe. Una regla práctica: 10 Gbps en OM3 llega a unos 300 m y en OM4 a unos 400 m; en monomodo, con el transceptor adecuado, a 10 km o más.

Ventajas de la fibra sobre el cobre:
- **Inmune a interferencia electromagnética** y no genera emisiones; difícil de intervenir sin detectarse.
- **Mucho mayor distancia y ancho de banda**.
- **Aislamiento eléctrico**: ideal entre edificios, evita diferencias de potencial y daños por rayos.
- Cables más ligeros para la misma capacidad.

**Conectores** comunes: **LC** (pequeño, el más usado en transceptores SFP), **SC** (cuadrado, de empuje), **ST** (bayoneta, antiguo) y **MPO/MTP** (multifibra para 40/100 Gbps). En los switches la fibra se conecta mediante **transceptores** SFP, SFP+, QSFP, cada uno para un tipo de fibra y distancia (por ejemplo, 10GBASE-SR para multimodo y 10GBASE-LR para monomodo).

### Ejemplo
Unir el switch de distribución del edificio A con el del edificio B a 450 m:
- 450 m supera los 100 m del cobre: fibra.
- Con multimodo OM4 a 10 Gbps rondarías el límite; con **monomodo y transceptores 10GBASE-LR** tienes margen de sobra.
- Usa fibra con conectores LC en ambos extremos, dos hilos (uno transmite y otro recibe) y verifica que el TX de un lado llegue al RX del otro.

```text
SW-A(config)# interface te1/1/1
SW-A(config-if)# description Enlace fibra a Edificio B
SW-A(config-if)# end
SW-A# show interfaces te1/1/1 transceiver
SW-A# show interfaces te1/1/1 status
```

El comando de transceptor (disponible en muchos modelos) muestra la potencia óptica recibida: si es demasiado baja, revisa conectores sucios, curvaturas o el tipo de fibra.

### Cuándo usarlo y cuándo no
Fibra para backbone de edificio, entre edificios, enlaces de centro de datos de 10 Gbps o más y entornos con ruido eléctrico. Multimodo dentro del edificio o centro de datos; monomodo para campus grandes y enlaces de proveedor. Para conectar PCs de escritorio, el cobre sigue siendo más práctico y permite PoE.

### Errores comunes
- **Mezclar transceptor monomodo con fibra multimodo** (o viceversa): enlace inestable o caído.
- **Conectores sucios**: la principal causa de fallas en fibra; límpialos antes de conectar.
- **Doblar la fibra más allá de su radio mínimo**: pierde luz.
- **Mirar directo un conector activo**: el láser puede dañar la vista.

### Para profundizar
- [IEEE 802.3 Ethernet Working Group](https://www.ieee802.org/3/)
- [TIA: estándares de cableado](https://tiaonline.org/what-we-do/standards/)

## Autoevaluación

Contesta primero en voz alta o en papel; luego despliega la respuesta.

> [!question]- ¿Qué diferencia hay entre fibra monomodo y multimodo?
> **Monomodo (SMF)**: núcleo muy delgado (~9 µm), un solo haz de luz con láser, llega a decenas de km. **Multimodo (MMF)**: núcleo más ancho (50 o 62.5 µm), varios haces, más barata y para cientos de metros.

> [!question]- ¿Qué ventajas tiene la fibra sobre el cobre?
> Inmune a interferencia electromagnética, mayor distancia, más ancho de banda y no conduce electricidad (útil entre edificios).

> [!question]- Menciona dos tipos de conector de fibra.
> **LC** (pequeño, muy común en switches con SFP) y **SC** (cuadrado, de presión). También existe ST (bayoneta).

