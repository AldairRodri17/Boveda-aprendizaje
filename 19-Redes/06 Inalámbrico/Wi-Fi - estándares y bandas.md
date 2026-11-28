---
proyecto: Conocimiento
tipo: concepto
nivel: intermedio
progreso: pendiente
estado: pendiente
area: Redes
tags: [redes]
contenido: completo
---
# Wi-Fi - estándares y bandas

Parte de: [[19-Redes/06 Inalámbrico/_Índice Inalámbrico|Inalámbrico]]

> [!info] Cómo avanzar
> `progreso`: **pendiente** → **viendo** (estudiando) → **visto** (contestaste la autoevaluación sin abrir las respuestas) → **dominado** (hiciste el laboratorio o lo aplicaste de verdad).

## Contenido

### Qué es
**Wi-Fi** es el nombre comercial de las redes locales inalámbricas basadas en los estándares **IEEE 802.11**. Cada generación del estándar define modulación, anchos de canal, número de flujos espaciales y bandas de frecuencia. La **Wi-Fi Alliance** certifica la interoperabilidad y, desde 2018, usa nombres comerciales numerados (Wi-Fi 4, 5, 6…) para que sea más fácil saber qué soporta un equipo.

### Cómo funciona
**Generaciones** principales:

| Nombre comercial | Estándar | Bandas | Ideas clave |
|---|---|---|---|
| (sin nombre) | 802.11b / a / g | 2.4 / 5 / 2.4 GHz | Históricos; 11 y 54 Mbps nominales |
| Wi-Fi 4 | 802.11n | 2.4 y 5 GHz | MIMO, canales de 40 MHz |
| Wi-Fi 5 | 802.11ac | 5 GHz | Canales de 80/160 MHz, MU-MIMO de bajada |
| Wi-Fi 6 | 802.11ax | 2.4 y 5 GHz | OFDMA, eficiencia con muchos clientes, BSS coloring |
| Wi-Fi 6E | 802.11ax | Agrega 6 GHz | Mucho más espectro limpio |
| Wi-Fi 7 | 802.11be | 2.4, 5 y 6 GHz | Canales de 320 MHz, MLO (varias bandas a la vez) |

Las velocidades del empaque son **máximos teóricos** combinando todos los flujos y el canal más ancho; el rendimiento real de un cliente es bastante menor.

**2.4 GHz frente a 5 GHz (y 6 GHz)**:
- **2.4 GHz**: mayor alcance y mejor penetración de paredes, pero solo 3 canales que no se traslapan, mucha interferencia (microondas, Bluetooth, vecinos) y menor velocidad.
- **5 GHz**: muchos más canales, más velocidad, menos interferencia; menor alcance. Algunos canales requieren **DFS** (el AP debe cambiar de canal si detecta radar).
- **6 GHz**: aún más canales anchos y sin dispositivos antiguos; alcance todavía menor. Solo la usan equipos Wi-Fi 6E/7 y su disponibilidad depende de la regulación de cada país (en México, verifica la normativa vigente del regulador antes de planear; Pendiente de confirmar).

**Canales en 2.4 GHz**: los canales están separados 5 MHz pero cada uno ocupa unos 20 MHz, así que se traslapan. Los únicos tres que no se traslapan en América son **1, 6 y 11**. Usa solo esos y alterna entre APs vecinos.

### Ejemplo
Tu iPhone 15 Pro soporta Wi-Fi 6E. Para aprovecharlo con tu laptop y subir rápido videos de la a6700 o la Osmo Pocket al NAS:
- Router o AP con 6 GHz: conecta los equipos compatibles ahí, cerca del AP.
- En 5 GHz usa canales de 80 MHz si no hay muchos vecinos; en un edificio con muchas redes, 40 MHz puede dar mejor resultado.
- Deja 2.4 GHz con canales de **20 MHz** en 1, 6 u 11 para dispositivos IoT y para cobertura lejana.

Revisa en Windows qué estándar y canal usas:

```bash
netsh wlan show interfaces
# Tipo de radio : 802.11ax   Canal : 36   Banda : 5 GHz   Señal : 88%
```

En un controlador o AP Cisco puedes ver el canal y la potencia de cada radio (en Packet Tracer, en la pestaña Config del AP verás el canal y el SSID).

**Subir la potencia al máximo no siempre ayuda**: el AP puede "gritar" lejos, pero el cliente (un teléfono con mucha menos potencia de transmisión) no le responde con la misma fuerza, y la comunicación es asimétrica. Además, un AP con mucha potencia invade la celda de sus vecinos en el mismo canal y aumenta la interferencia cocanal. En redes con varios APs se prefiere potencia moderada y más APs bien distribuidos.

### Cuándo usarlo y cuándo no
Elige 5 o 6 GHz para rendimiento y para la mayoría de los clientes modernos; 2.4 GHz para alcance e IoT. Para equipos que exigen estabilidad (servidores, estación de edición, consola de videojuegos competitiva), el cable sigue siendo superior. Planea con un estudio de sitio (site survey) en oficinas: la cobertura depende de paredes, muebles y personas.

### Errores comunes
- **Usar canales 2, 3, 4… en 2.4 GHz**: se traslapan con todos los demás.
- **Canales de 40 MHz en 2.4 GHz**: ocupan casi toda la banda.
- **Fiarse de la velocidad nominal** del empaque.
- **Subir potencia en lugar de agregar APs**.

### Para profundizar
- [IEEE 802.11 Working Group](https://www.ieee802.org/11/)
- [Wi-Fi Alliance](https://www.wi-fi.org/)
- [Cisco: Campus LAN and Wireless LAN Solution Design Guide](https://www.cisco.com/c/en/us/td/docs/solutions/CVD/Campus/cisco-campus-lan-wlan-design-guide.html)

## Autoevaluación

Contesta primero en voz alta o en papel; luego despliega la respuesta.

> [!question]- Relaciona los nombres comerciales con los estándares.
> Wi-Fi 4 = 802.11n · Wi-Fi 5 = 802.11ac · Wi-Fi 6/6E = 802.11ax · Wi-Fi 7 = 802.11be. Los anteriores: 802.11b/g (2.4 GHz) y 802.11a (5 GHz).

> [!question]- ¿Qué diferencia hay entre 2.4 GHz y 5 GHz?
> **2.4 GHz**: más alcance y atraviesa mejor paredes, pero pocos canales y mucha interferencia. **5 GHz**: más canales y velocidad, menos alcance. (6 GHz: aún más canales limpios, alcance menor).

> [!question]- ¿Cuáles son los canales que no se traslapan en 2.4 GHz?
> **1, 6 y 11** (en América).

> [!question]- Error común: subir la potencia del AP al máximo para mejorar la cobertura. ¿Por qué no siempre ayuda?
> El cliente (celular) transmite con menos potencia y el AP no lo escucha; además aumenta la interferencia con APs vecinos del mismo canal.

## Práctica

- [[Lab 1 - Wi-Fi básico en Packet Tracer]]

