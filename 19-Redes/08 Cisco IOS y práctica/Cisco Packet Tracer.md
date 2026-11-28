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
# Cisco Packet Tracer

Parte de: [[19-Redes/08 Cisco IOS y práctica/_Índice Cisco IOS y práctica|Cisco IOS y práctica]]

> [!info] Cómo avanzar
> `progreso`: **pendiente** → **viendo** (estudiando) → **visto** (contestaste la autoevaluación sin abrir las respuestas) → **dominado** (hiciste el laboratorio o lo aplicaste de verdad).

## Contenido

### Qué es
**Cisco Packet Tracer** es un simulador de redes **gratuito** de Cisco Networking Academy. Permite armar topologías con routers, switches, access points, PCs, servidores e incluso dispositivos IoT, configurarlos con una CLI muy parecida a IOS y ver cómo viajan los paquetes. Es la herramienta oficial con la que se practican los cursos de CCNA y la forma más barata de ganar práctica real antes del examen. Se descarga con una cuenta gratuita de Networking Academy (por ejemplo, inscribiéndote al curso introductorio de Packet Tracer).

### Cómo funciona
**Áreas de trabajo**:
- **Vista lógica**: el diagrama de la red, donde arrastras equipos y los conectas con el tipo de cable correcto (directo, cruzado, consola, serial, fibra) o con el conector automático.
- **Vista física**: edificios, racks y distancias (útil para Wi-Fi y cableado).
- **Pestañas de cada equipo**: *Physical* (agregar módulos, encender), *Config* (configuración por formularios), *CLI* (la consola de IOS) y, en PCs, *Desktop* (configuración IP, símbolo del sistema, navegador, cliente de correo, terminal).

**Modo tiempo real frente a modo simulación**:
- **Tiempo real**: la red funciona normalmente; los protocolos convergen y los pings responden como en equipos reales (en tiempo acelerado).
- **Simulación**: el tiempo se detiene y avanzas paso a paso. Cada PDU aparece como un sobre que puedes abrir para ver sus encabezados **por capa OSI** (entrante y saliente) y por qué el equipo tomó cada decisión. Con los filtros de eventos ves solo ARP, ICMP, DHCP, OSPF, STP, etc. Es perfecto para entender [[Encapsulamiento y PDUs]], ARP o el proceso DORA de DHCP.

**Limitaciones frente a equipos reales**:
- Simula un **subconjunto** de comandos de IOS; algunos no existen o se comportan distinto (opciones avanzadas de QoS, ciertas funciones de seguridad, `debug` limitados).
- No refleja rendimiento real ni fallas físicas sutiles.
- Los modelos de equipos disponibles son limitados.
Para práctica más fiel se usan emuladores con imágenes reales como **Cisco Modeling Labs (CML)** (de pago, con una edición gratuita limitada; Pendiente de confirmar las condiciones vigentes) o equipos físicos usados.

**Actividades**: los archivos `.pka` incluyen instrucciones y verificación automática del porcentaje completado; los `.pkt` son topologías libres. Guarda tus propias prácticas en `.pkt` dentro de una carpeta con nombre por tema.

### Ejemplo
Primer laboratorio en 15 minutos:
1. Arrastra un router 2911 (o 4331), un switch 2960 y dos PCs.
2. Conecta las PCs al switch y el switch al router con cables directos.
3. En el router (pestaña CLI):

```text
Router> enable
Router# configure terminal
Router(config)# hostname R1
R1(config)# no ip domain-lookup
R1(config)# interface g0/0
R1(config-if)# ip address 192.168.1.1 255.255.255.0
R1(config-if)# no shutdown
R1(config-if)# end
R1# copy running-config startup-config
```

4. En cada PC (Desktop → IP Configuration): `192.168.1.10/24` y `192.168.1.11/24`, gateway `192.168.1.1`.
5. Cambia a **Simulación**, filtra solo ARP e ICMP y haz `ping` de una PC a otra desde Desktop → Command Prompt. Abre cada sobre y observa la MAC destino del primer ARP (`FFFF.FFFF.FFFF`).

**Para el examen CCNA**: según Cisco, el examen 200-301 dura **120 minutos** y cuesta **US$300**. La versión actual, v1.1, se puede presentar hasta el **2 de febrero de 2027**; la **v2.0** se aplica desde el **3 de febrero de 2027**, con dominios Network Infrastructure and Connectivity (25%), Switching and Network Access (25%), IP Routing (20%), Network Services and Security (20%) y AI, and Network Operations and Management (10%). Verifica en cisco.com antes de agendar, porque los temarios pueden cambiar.

### Cuándo usarlo y cuándo no
Úsalo para aprender y repasar todo el temario del CCNA: VLAN, STP, OSPF, ACL, NAT, DHCP, IPv6, Wi-Fi básico. Es ligero y corre bien en tu PC con Windows. Cuando necesites probar funciones que Packet Tracer no soporta, pasa a CML o a equipos reales.

### Errores comunes
- **Usar el tipo de cable incorrecto** (en Packet Tracer los equipos antiguos no tienen Auto-MDIX).
- **Olvidar encender un equipo** tras agregar un módulo en la vista física.
- **Asumir que un comando no existe en IOS** porque Packet Tracer no lo reconoce.
- **No guardar la configuración** de los equipos antes de cerrar el archivo: el `.pkt` guarda la running-config, pero practicar `copy run start` es un buen hábito.

### Para profundizar
- [Cisco Packet Tracer en Networking Academy](https://www.netacad.com/cisco-packet-tracer)
- [Curso Getting Started with Cisco Packet Tracer](https://www.netacad.com/courses/getting-started-cisco-packet-tracer)
- [Cisco: examen 200-301 CCNA](https://www.cisco.com/site/us/en/learn/training-certifications/exams/ccna.html)
- [Cisco blog: fechas de la actualización CCNA v2.0](https://blogs.cisco.com/learning/stay-on-track-get-certified-before-the-ccna-refresh)

## Autoevaluación

Contesta primero en voz alta o en papel; luego despliega la respuesta.

> [!question]- ¿Qué es Packet Tracer y cómo se obtiene?
> Un simulador de redes de Cisco para practicar configuración de routers, switches y servicios. Se descarga gratis con una cuenta de Cisco Networking Academy.

> [!question]- ¿Qué diferencia hay entre el modo tiempo real y el modo simulación?
> **Tiempo real**: la red funciona normalmente. **Simulación**: avanza paso a paso y muestra cada PDU con sus encabezados por capa OSI; ideal para ver ARP, DHCP o el proceso de encapsulamiento.

> [!question]- ¿Qué limitación tiene frente a equipos reales?
> Simula un subconjunto de comandos y funciones de IOS; algunos comandos avanzados no existen o se comportan distinto.

## Práctica

- [[Lab 1 - SSH y respaldo de configuración en Packet Tracer]]

