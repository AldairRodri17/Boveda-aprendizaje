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
# Ataques comunes de red (spoofing, MITM, DoS)

Parte de: [[19-Redes/07 Seguridad de redes/_Índice Seguridad de redes|Seguridad de redes]]

> [!info] Cómo avanzar
> `progreso`: **pendiente** → **viendo** (estudiando) → **visto** (contestaste la autoevaluación sin abrir las respuestas) → **dominado** (hiciste el laboratorio o lo aplicaste de verdad).

## Contenido

### Qué es
Esta nota resume los ataques de red más frecuentes que debes reconocer para el CCNA y para proteger tus propios sistemas: **suplantación** (spoofing) de direcciones, **intermediario** (MITM, man-in-the-middle), **denegación de servicio** (DoS y DDoS) y los ataques típicos de **capa 2** contra switches. Para cada uno importa entender cómo funciona y qué control lo mitiga.

### Cómo funciona
**Spoofing**: falsificar una identidad de red.
- **IP spoofing**: enviar paquetes con una IP origen falsa (para esconderse o para que las respuestas lleguen a una víctima). Se mitiga con filtrado de entrada en el borde (descartar desde internet paquetes con origen privado o propio, BCP 38) y con **uRPF** (`ip verify unicast source reachable-via rx`), que descarta paquetes cuyo origen no es alcanzable por la interfaz de llegada.
- **MAC spoofing**: cambiar la MAC para hacerse pasar por otro equipo o saltar filtros por MAC. Se mitiga con port security y, sobre todo, con 802.1X.
- **ARP spoofing / envenenamiento ARP**: el atacante envía respuestas ARP falsas ("la IP del gateway está en mi MAC"). Las víctimas le mandan su tráfico y él lo reenvía al gateway real, quedando en medio (MITM). Mitigación: **Dynamic ARP Inspection** con DHCP snooping (ver [[DHCP snooping y Dynamic ARP Inspection]]) y cifrado de extremo a extremo.
- **DHCP spoofing**: un servidor DHCP falso entrega como gateway o DNS la IP del atacante. Mitigación: **DHCP snooping**.
- **DNS spoofing**: respuestas DNS falsas. Mitigación: resolvers confiables, DNSSEC, DNS cifrado.

**MITM** en general: el atacante se coloca entre dos partes para leer o modificar tráfico (ARP spoofing, APs falsos tipo "evil twin", proxies maliciosos). La mitigación de fondo es **cifrar y autenticar** (TLS con validación de certificados, SSH, VPN): aunque intercepte, no puede leer ni alterar sin ser detectado.

**Ataques de capa 2 contra switches**:
- **MAC flooding**: inundar el switch con miles de MAC falsas para llenar su tabla; entonces inunda todo como un hub y el atacante ve el tráfico. Mitigación: **port security** con máximo de MAC.
- **DHCP starvation**: agotar el pool con solicitudes falsas (preparación para un DHCP falso). Mitigación: port security y límite de tasa de DHCP snooping.
- **VLAN hopping**: por **switch spoofing** (el atacante negocia un trunk con DTP) o **doble etiquetado** (abusa de la VLAN nativa). Mitigación: puertos de usuario en `switchport mode access`, `switchport nonegotiate`, VLAN nativa sin uso y distinta de la 1, y apagar puertos sin uso en una VLAN "aparcadero".
- **Ataques a STP**: anunciar BPDU con prioridad 0 para volverse root. Mitigación: **BPDU Guard** y Root Guard.

**DoS y DDoS**: saturar un servicio o enlace para que deje de responder. **DoS** viene de una sola fuente (fácil de bloquear por IP); **DDoS** viene de miles de equipos comprometidos (botnets), a menudo con **amplificación** (pequeñas consultas DNS, NTP o memcached con origen falsificado que generan respuestas enormes hacia la víctima). Mitigación: servicios anti-DDoS del proveedor o de la nube (Azure DDoS Protection, CDN), limitación de tasa, cerrar servicios abiertos que sirvan de amplificadores y filtrado de spoofing.

### Ejemplo
Endurecimiento básico de un switch de acceso:

```text
SW1(config)# interface range fa0/1 - 22
SW1(config-if-range)# switchport mode access
SW1(config-if-range)# switchport nonegotiate
SW1(config-if-range)# switchport port-security maximum 2
SW1(config-if-range)# switchport port-security
SW1(config-if-range)# spanning-tree portfast
SW1(config-if-range)# spanning-tree bpduguard enable
SW1(config-if-range)# interface range fa0/23 - 24
SW1(config-if-range)# switchport access vlan 999
SW1(config-if-range)# shutdown
SW1(config)# ip dhcp snooping
SW1(config)# ip dhcp snooping vlan 10,20
SW1(config)# ip arp inspection vlan 10,20
```

Y no olvides el uplink como confiable para DHCP snooping y DAI. Para tus APIs en Azure, el equivalente de capa 7 es un WAF y límites de tasa en el API Management o en el propio ASP.NET Core (middleware de rate limiting).

### Cuándo usarlo y cuándo no
Aplica estas mitigaciones de forma estándar en todos los switches de acceso y en el borde; no esperes a un incidente. Para DDoS volumétricos, ninguna configuración local basta si tu enlace se satura: necesitas protección aguas arriba (proveedor o nube).

### Errores comunes
- **Confiar en la VLAN como frontera de seguridad** sin ACL ni firewall entre VLAN.
- **Dejar puertos sin uso activos en la VLAN 1**.
- **No cifrar tráfico interno** "porque es la LAN": el MITM interno es real.
- **Confundir DoS con DDoS**: la mitigación es muy distinta.

### Para profundizar
- [RFC 2827 (BCP 38): filtrado de entrada contra spoofing](https://www.rfc-editor.org/rfc/rfc2827)
- [Cisco Guide to Harden Cisco IOS Devices](https://www.cisco.com/c/en/us/support/docs/ip/access-lists/13608-21.html)
- [Microsoft Learn: Azure DDoS Protection](https://learn.microsoft.com/azure/ddos-protection/ddos-protection-overview)

## Autoevaluación

Contesta primero en voz alta o en papel; luego despliega la respuesta.

> [!question]- ¿Qué es ARP spoofing y cómo se mitiga?
> El atacante envía ARP falsos para asociar su MAC a la IP de la puerta de enlace y quedar en medio (MITM). Se mitiga con **DAI** + DHCP snooping.

> [!question]- ¿Qué ataques de capa 2 frenan port security y DHCP snooping?
> **Port security**: inundación de la tabla MAC (*MAC flooding*). **DHCP snooping**: servidores DHCP falsos (*spoofing*) y agotamiento del pool (*starvation*), junto con un límite de tasa.

> [!question]- ¿Cómo se previene VLAN hopping?
> Puertos de usuario en `switchport mode access` y `switchport nonegotiate` (sin DTP), VLAN nativa sin uso en los trunks y puertos sin uso apagados en una VLAN aislada.

> [!question]- ¿Qué diferencia hay entre DoS y DDoS?
> **DoS**: un origen satura un servicio. **DDoS**: muchos orígenes (botnet) a la vez; requiere mitigación en el proveedor o servicios de limpieza de tráfico.

