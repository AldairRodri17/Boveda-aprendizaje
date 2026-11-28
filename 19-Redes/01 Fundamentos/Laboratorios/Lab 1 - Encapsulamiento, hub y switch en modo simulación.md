---
proyecto: Conocimiento
tipo: laboratorio
nivel: basico
progreso: pendiente
estado: borrador
area: Redes
herramienta: Cisco Packet Tracer
tags: [redes, laboratorio]
contenido: completo
---
# Lab 1 - Encapsulamiento, hub y switch en modo simulación

Parte de: [[19-Redes/01 Fundamentos/_Índice Fundamentos|Fundamentos]]

**Cubre:** [[Encapsulamiento y PDUs]] · [[Modelo OSI]] · [[Hub vs switch vs router]] · [[Hub]] · [[Switch]] · [[Dominios de colisión y de broadcast]] · [[Ethernet y tramas]] · [[Direcciones MAC]]

> [!info] Al terminar
> Si todos los criterios quedan marcados, cambia a `dominado` el `progreso` de este laboratorio y de las notas que cubre. Guarda tus archivos de práctica en esta misma carpeta.

## Objetivo

Ver en el modo simulación de Packet Tracer cómo se encapsula un ping, y comparar cómo reenvían las tramas un hub y un switch.

## Topología

```
PC1 ─┐               ┌─ PC4
PC2 ─┼─ HUB0   SW0 ──┼─ PC5
PC3 ─┘               └─ PC6
```

Dos redes independientes (no se conectan entre sí). Equipos: 1 hub, 1 switch 2960 y 6 PCs.

## Pasos

1. Arma la topología. Direcciones: PC1–PC3 `192.168.1.1–3/24`, PC4–PC6 `192.168.2.4–6/24` (sin puerta de enlace).
2. Anota la MAC de cada PC (pasa el cursor sobre la PC o usa `ipconfig /all` en su Command Prompt).
3. Pasa al modo **Simulación**. En *Edit Filters* deja solo **ARP** e **ICMP**.
4. Desde PC1 haz `ping 192.168.1.3`. Avanza con **Capture/Forward** y observa por dónde sale cada trama del hub.
5. Abre un sobre (PDU) de ICMP y revisa la pestaña **OSI Model** y **Outbound PDU Details**: identifica MAC origen y destino, EtherType, IP origen y destino, TTL y tipo ICMP.
6. Repite el ping de PC4 a PC6 y observa el switch. Luego repítelo otra vez (ya con la tabla MAC llena).
7. En el switch (CLI) ejecuta `show mac address-table`.
8. Cuenta los dominios de colisión y de broadcast de cada red.

## Criterios de terminado

- [ ] Vi que el hub reenvía el ICMP por **todos** sus puertos, incluso a PC2.
- [ ] Vi que el switch inunda el ARP request (broadcast) pero envía el ICMP solo por el puerto de PC6.
- [ ] `show mac address-table` en SW0 muestra las MAC de PC4 y PC6 con su puerto.
- [ ] Identifiqué en el PDU: trama Ethernet (MAC, EtherType `0x0800`), paquete IP (TTL) e ICMP tipo 8 (echo request) y tipo 0 (echo reply).
- [ ] Mi conteo de dominios coincide con la solución.

## Solución

> [!success]- Comportamiento esperado
> - **Hub**: cada trama (ARP y luego ICMP) sale por todos los puertos excepto el de entrada; PC2 la recibe y la descarta porque la MAC destino no es la suya.
> - **Switch**: el ARP request es broadcast (`FFFF.FFFF.FFFF`) y se inunda; la respuesta ARP y los ICMP van solo entre PC4 y PC6, porque el switch ya aprendió sus MAC por la MAC origen.
> - En el segundo ping no hay ARP: ambas PCs ya tienen la MAC en su caché (`arp -a`).

> [!success]- Encabezados del ICMP de PC1 a PC3
> | Capa | Campo | Valor |
> |---|---|---|
> | 2 | MAC destino / origen | MAC de PC3 / MAC de PC1 |
> | 2 | EtherType | `0x0800` (IPv4) |
> | 3 | IP origen / destino | 192.168.1.1 / 192.168.1.3 |
> | 3 | TTL | 128 (valor típico de las PCs de Packet Tracer) |
> | 3-4 | ICMP | tipo 8 en la solicitud, tipo 0 en la respuesta |

> [!success]- Conteo de dominios
> - Red del hub: **1** dominio de colisión y **1** de broadcast.
> - Red del switch: **3** dominios de colisión (uno por puerto usado) y **1** de broadcast.

## Notas personales

_Qué me costó, errores que cometí, dudas._
