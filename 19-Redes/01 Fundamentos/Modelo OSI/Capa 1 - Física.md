---
proyecto: Conocimiento
tipo: concepto
nivel: basico
progreso: pendiente
estado: pendiente
area: Redes
tags: [redes]
contenido: completo
---
# Capa 1 - Física

Parte de: [[Modelo OSI]]

> [!info] Cómo avanzar
> `progreso`: **pendiente** → **viendo** (estudiando) → **visto** (contestaste la autoevaluación sin abrir las respuestas) → **dominado** (hiciste el laboratorio o lo aplicaste de verdad).

## Contenido

### Qué es
La capa física es la base del [[Modelo OSI]]: define cómo se transmiten los **bits** como señales por un medio. Aquí viven los cables, conectores, niveles de voltaje, pulsos de luz, ondas de radio, velocidades, distancias máximas y la distribución de pines. Su trabajo es simple de decir y difícil de hacer bien: que un 1 enviado de un lado llegue como 1 al otro, a la velocidad acordada.

### Cómo funciona
La tarjeta de red (NIC) recibe una trama de la [[Capa 2 - Enlace de datos]] y la convierte en una secuencia de símbolos según el estándar del medio:
1. **Codificación**: transforma bits en patrones de señal (por ejemplo, PAM en Ethernet sobre cobre o modulación OFDM en Wi-Fi).
2. **Señalización**: los pone en el medio como voltaje en un par trenzado, luz en una fibra o radiofrecuencia en el aire.
3. **Sincronización y negociación**: los dos extremos acuerdan velocidad y dúplex (autonegociación) antes de transmitir datos.
4. **Recepción**: el otro extremo detecta la señal, la decodifica y entrega los bits a la capa 2.

La capa 1 no entiende direcciones ni errores lógicos: no sabe qué es una MAC. Si la señal llega degradada, la detección del daño ocurre arriba, cuando el FCS de la trama no coincide. Los medios concretos se explican en [[Medios físicos (cobre, fibra e inalámbrico)]] y el cableado en [[Cableado estructurado y estándares (T568A y T568B)]]. El [[Hub]] es el ejemplo clásico de dispositivo de capa 1: repite bits sin mirar su contenido.

### Ejemplo
Un usuario de la oficina dice que "el internet está lento". Antes de revisar DNS o rutas, revisas la capa 1 en el switch:

```text
SW1# show interfaces status
Port      Name      Status       Vlan  Duplex  Speed  Type
Fa0/5     PC-Ana    connected    10    a-half  a-10   10/100BaseTX
Fa0/6     PC-Luis   notconnect   10    auto    auto   10/100BaseTX

SW1# show interfaces fa0/5
FastEthernet0/5 is up, line protocol is up
  ...
  0 input errors, 152 CRC, 0 frame
  2104 collisions, 87 late collision
```

El puerto de Ana negoció 10 Mbps half dúplex y tiene errores CRC y colisiones tardías: síntomas típicos de un cable dañado, mal ponchado o de un desajuste de dúplex. Cambias el cable y el puerto sube a `a-full a-100`. Problema resuelto sin tocar la configuración de IP.

Otros problemas de capa 1 frecuentes: cable desconectado o roto, conector con pares invertidos, cable que supera los 100 m, fibra sucia o con el transceptor equivocado (multimodo vs. monomodo), interferencia electromagnética cerca de motores o lámparas, y en Wi-Fi, mala señal o canales saturados.

### Cuándo usarlo y cuándo no
Empieza por la capa 1 cuando el síntoma es "no hay enlace", "se cae a ratos" o "va muy lento" sin un patrón lógico. Los comandos útiles en Cisco IOS son `show interfaces status`, `show interfaces` (contadores de errores), `show ip interface brief` (estado up/down) y, en algunos modelos, pruebas de cable como `test cable-diagnostics tdr interface`. Si el enlace está `up/up`, sin errores y con la velocidad esperada, sube a la capa 2.

### Errores comunes
- **Saltarse la capa 1**: se pierde una hora revisando ACL cuando el cable estaba flojo. Mira primero los LED y el estado de la interfaz.
- **Confundir `administratively down` con falla física**: ese estado significa que alguien aplicó `shutdown`; no es el cable.
- **Ignorar los contadores**: un enlace `up` con CRC creciendo sigue teniendo un problema físico. Limpia contadores con `clear counters` y observa si aumentan.
- **Forzar velocidad o dúplex en un solo extremo**: rompe la autonegociación y produce desajustes de dúplex.

### Para profundizar
- [IEEE 802.3 Ethernet Working Group](https://www.ieee802.org/3/)
- [Cisco: Troubleshoot Ethernet collisions and errors (documentación de soporte)](https://www.cisco.com/c/en/us/support/docs/interfaces-modules/port-adapters/12768-eth-collisions.html)

## Autoevaluación

Contesta primero en voz alta o en papel; luego despliega la respuesta.

> [!question]- ¿Qué hace la capa física?
> Transmite bits como señales (eléctricas, de luz o de radio) por el medio. Define conectores, voltajes, tipos de cable, velocidades y distancias.

> [!question]- Dame tres ejemplos de problemas de capa 1.
> Cable dañado o desconectado, cable demasiado largo (más de 100 m en cobre), interferencia electromagnética, conector mal ponchado, puerto apagado.

> [!question]- ¿Qué comando de IOS usarías primero para descartar capa 1?
> `show ip interface brief` (estado **up/down** de la interfaz) y luego `show interfaces` para ver errores como CRC o *input errors*.

