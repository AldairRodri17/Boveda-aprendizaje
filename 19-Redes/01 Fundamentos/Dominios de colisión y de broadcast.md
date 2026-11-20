---
proyecto: Conocimiento
tipo: concepto
nivel: basico
progreso: pendiente
estado: pendiente
area: Redes
tags: [redes]
---
# Dominios de colisión y de broadcast

Parte de: [[19-Redes/01 Fundamentos/_Índice Fundamentos|Fundamentos]]

> [!info] Cómo avanzar
> `progreso`: **pendiente** → **viendo** (estudiando) → **visto** (contestaste la autoevaluación sin abrir las respuestas) → **dominado** (hiciste el laboratorio o lo aplicaste de verdad).

## Contenido

### Qué es
Un **dominio de colisión** es el conjunto de dispositivos que comparten el medio de forma que, si dos transmiten a la vez, sus señales chocan. Un **dominio de broadcast** es el conjunto de dispositivos que reciben una trama de broadcast (`FF:FF:FF:FF:FF:FF`) enviada por cualquiera de ellos. Saber contarlos es un ejercicio clásico del examen y, más importante, te ayuda a diseñar redes que no se saturen.

### Cómo funciona
Reglas para contar:
- Un **hub** extiende el dominio de colisión: todo lo conectado a él (incluido el puerto del equipo al que sube) es **un** dominio de colisión.
- Un **switch** crea **un dominio de colisión por puerto** en uso. Con full dúplex, en la práctica no hay colisiones, pero en el conteo cada enlace cuenta.
- Un **router** también separa colisiones: cada interfaz es su propio dominio.
- Los **broadcasts** atraviesan hubs y switches (dentro de la misma VLAN) pero **no atraviesan routers**. Cada interfaz de router delimita un dominio de broadcast.
- Las **VLAN** dividen un switch en varios dominios de broadcast; sin un router, no se comunican entre sí.

Procedimiento:
1. Dibuja la topología.
2. Para colisión: cuenta cada enlace que termina en un puerto de switch o router; cada hub (con todo lo conectado) suma uno solo.
3. Para broadcast: cuenta las interfaces de router que tienen algo conectado (o las VLAN, si no hay router entre ellas).

### Ejemplo
Topología: R1 con tres interfaces.
- G0/0 → SW1 con 5 PCs.
- G0/1 → SW2 con 2 PCs y un hub con 4 PCs.
- G0/2 → un enlace punto a punto a R2.

Colisión:
- SW1: 5 PCs + enlace a R1 = 6.
- SW2: 2 PCs + enlace a R1 + enlace al hub = 4 (el enlace al hub y sus 4 PCs son un único dominio).
- Enlace R1–R2 = 1.
- Total **11**.

Broadcast: G0/0, G0/1 y G0/2 → **3**.

¿Por qué importa limitar el tamaño del dominio de broadcast? Porque todos los equipos procesan cada broadcast (ARP, DHCP, anuncios de descubrimiento, NetBIOS, mDNS). En una red plana de 2,000 equipos, ese ruido consume CPU y ancho de banda en cada host, y un bucle de capa 2 genera una **tormenta de broadcast** que tumba todo el dominio. Por eso se segmenta con VLAN y subredes de tamaño razonable (por ejemplo, /24 con hasta 254 hosts por VLAN de usuarios).

Comprobación práctica en un switch:

```text
SW1# show interfaces counters | include Port|Fa0/1
Port            InOctets    InUcastPkts    InMcastPkts    InBcastPkts
Fa0/1           8123344     51233          4210           18654
```

Un contador de broadcasts que crece muy rápido es señal de un dominio demasiado grande o de un bucle.

### Cuándo usarlo y cuándo no
Haz este análisis al diseñar una LAN (cuántos equipos por VLAN, dónde poner routers o SVI) y al diagnosticar lentitud general. En redes modernas sin hubs y con full dúplex, las colisiones son raras; el foco práctico está en los **dominios de broadcast**. Practica en [[Cisco Packet Tracer]] con la topología de este ejemplo y verifica en modo simulación hasta dónde llega un ARP.

### Errores comunes
- **Contar cada PC del hub por separado**: el hub completo es un dominio.
- **Olvidar los enlaces entre equipos intermedios** (switch–router, switch–switch).
- **Pensar que el switch separa broadcasts**: solo con VLAN.
- **Contar una VLAN sin puertos como dominio**: solo cuenta si tiene equipos.

### Para profundizar
- [IEEE 802.3 Ethernet Working Group](https://www.ieee802.org/3/)
- [IEEE 802.1 Working Group (VLAN)](https://www.ieee802.org/1/)

## Ejercicios

Resuélvelos en papel o en la herramienta; la solución está plegada.

> [!question]- R1 tiene G0/0 hacia SW1 (3 PCs) y G0/1 hacia SW2. SW2 tiene 2 PCs y un hub con 3 PCs. ¿Cuántos dominios de colisión y de broadcast hay?
> **Colisión: 8.** SW1: 3 PCs + enlace a R1 = 4. SW2: 2 PCs + hub (todo el hub es uno) + enlace a R1 = 4.
> **Broadcast: 2** (uno por interfaz de R1).

> [!question]- Un hub con 6 PCs. ¿Cuántos dominios de cada tipo?
> **1** de colisión y **1** de broadcast.

> [!question]- Un switch con 12 PCs repartidas en VLAN 10 y VLAN 20, sin router. ¿Cuántos dominios de broadcast?
> **2**, uno por VLAN (y 12 de colisión, uno por puerto usado).

## Autoevaluación

Contesta primero en voz alta o en papel; luego despliega la respuesta.

> [!question]- ¿Qué es un dominio de colisión?
> El segmento donde dos equipos que transmiten al mismo tiempo pueden chocar. Con switches en full-duplex las colisiones prácticamente desaparecen.

> [!question]- ¿Qué es un dominio de broadcast?
> El conjunto de equipos que reciben un broadcast de capa 2. Lo limitan los routers (y las VLANs).

> [!question]- ¿Por qué conviene limitar el tamaño de un dominio de broadcast?
> Muchos broadcasts (ARP, DHCP) consumen ancho de banda y CPU de todos los equipos, y una tormenta de broadcast afecta a toda la red.

## Práctica

- [[Lab 1 - Encapsulamiento, hub y switch en modo simulación]]

