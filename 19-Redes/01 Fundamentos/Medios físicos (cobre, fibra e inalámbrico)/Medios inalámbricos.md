---
proyecto: Conocimiento
tipo: concepto
nivel: basico
progreso: pendiente
estado: pendiente
area: Redes
tags: [redes]
---
# Medios inalámbricos

Parte de: [[Medios físicos (cobre, fibra e inalámbrico)]]

> [!info] Cómo avanzar
> `progreso`: **pendiente** → **viendo** (estudiando) → **visto** (contestaste la autoevaluación sin abrir las respuestas) → **dominado** (hiciste el laboratorio o lo aplicaste de verdad).

## Contenido

### Qué es
Los **medios inalámbricos** transmiten datos usando ondas electromagnéticas (normalmente radiofrecuencia) a través del aire, sin cables. Dan movilidad y llegan a lugares donde cablear es caro o imposible, a cambio de compartir un medio con interferencia, alcance limitado y retos de seguridad. En redes de datos los más importantes son **Wi-Fi (IEEE 802.11)**, **Bluetooth (originalmente IEEE 802.15.1)**, las redes **celulares** (4G/LTE y 5G) y los enlaces **satelitales** y de **microondas punto a punto**.

### Cómo funciona
- **Wi-Fi (802.11)**: WLAN en las bandas de 2.4, 5 y 6 GHz. Los clientes se asocian a un [[Access point]]. Los estándares y bandas se detallan en [[Wi-Fi - estándares y bandas]].
- **Bluetooth**: PAN de corto alcance (audífonos, relojes, micrófonos inalámbricos como el DJI Mic Mini 2 con su receptor).
- **Celular**: WAN de proveedor; tu iPhone usa 5G cuando sale del Wi-Fi.
- **Satelital**: cobertura donde no hay otra opción; tradicionalmente con alta latencia en órbita geoestacionaria, menor en órbita baja.
- **Microondas punto a punto**: enlaces con antenas direccionales entre edificios con línea de vista.

**Acceso al medio**: el aire es compartido y un equipo no puede detectar bien una colisión mientras transmite. Por eso Wi-Fi usa **CSMA/CA** (Collision Avoidance): escucha si el canal está libre, espera un tiempo aleatorio y transmite; el receptor confirma cada trama con un ACK. Opcionalmente se usa RTS/CTS para reservar el canal. Es inherentemente **half dúplex**: en un canal, solo un equipo transmite a la vez.

**Qué afecta la cobertura y el rendimiento**:
- **Distancia** y **frecuencia**: 2.4 GHz llega más lejos y atraviesa mejor paredes; 5 y 6 GHz dan más velocidad pero menos alcance.
- **Obstáculos**: concreto, metal, vidrio con película, agua (incluidas las personas), elevadores.
- **Interferencia**: otros APs en el mismo canal, microondas, Bluetooth, cámaras inalámbricas.
- **Potencia y antenas** del AP y del cliente.
- **Número de clientes**: todos comparten el tiempo de aire.

### Ejemplo
Tu dron DJI Mini 4K, la Osmo Pocket y el iPhone conviven en la misma banda de 2.4/5 GHz con el Wi-Fi de casa. Si al grabar con la Osmo conectada a la app notas cortes, revisa:
1. ¿El router de casa usa un canal de 2.4 GHz saturado? Cambia a 1, 6 u 11 o pasa el iPhone a 5 GHz.
2. ¿Hay muchas redes vecinas? Una app de análisis Wi-Fi te muestra los canales ocupados.
3. Para transferir archivos grandes de la a6700 a la PC, mejor cable o 5/6 GHz cerca del AP.

```bash
# Windows: ver la red, canal, banda y señal actuales
netsh wlan show interfaces
# Ver redes visibles con su canal
netsh wlan show networks mode=bssid
```

### Cuándo usarlo y cuándo no
Usa inalámbrico para movilidad (laptops, móviles), invitados, IoT y lugares sin cableado. Evítalo como único enlace para servidores, equipos de edición con archivos pesados o aplicaciones sensibles a la latencia constante. Si debes unir dos edificios sin fibra, un enlace de microondas punto a punto es una alternativa válida.

### Errores comunes
- **Esperar la velocidad nominal**: el rendimiento real suele ser bastante menor por la sobrecarga y el medio compartido.
- **Subir la potencia al máximo**: el AP te "escucha" lejos, pero tu cliente no le responde con la misma potencia, y causas interferencia a otros APs.
- **Ignorar la seguridad**: el medio llega a cualquiera dentro del alcance; usa WPA2 o WPA3 (ver [[Seguridad Wi-Fi (WPA2, WPA3 y 802.1X)]]).
- **Colocar el AP dentro de un gabinete metálico** o detrás de la TV.

### Para profundizar
- [IEEE 802.11 Working Group](https://www.ieee802.org/11/)
- [Wi-Fi Alliance](https://www.wi-fi.org/)
- [Bluetooth SIG: especificaciones](https://www.bluetooth.com/specifications/)

## Autoevaluación

Contesta primero en voz alta o en papel; luego despliega la respuesta.

> [!question]- ¿Qué estándares inalámbricos se usan en redes de datos?
> Wi-Fi (IEEE 802.11) para WLAN, Bluetooth (802.15.1) para PAN, y redes celulares (4G/5G) para WAN.

> [!question]- ¿Qué método de acceso al medio usa Wi-Fi?
> **CSMA/CA** (evitar colisiones): escucha antes de transmitir y espera confirmación (ACK), porque en radio no puede detectar colisiones como en el cable.

> [!question]- ¿Qué afecta la cobertura de una red inalámbrica?
> Distancia, paredes y materiales (concreto, metal, agua), interferencia de otras redes o aparatos y la banda usada (5 GHz alcanza menos que 2.4 GHz).

