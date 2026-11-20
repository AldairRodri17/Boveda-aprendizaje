---
proyecto: Conocimiento
tipo: concepto
nivel: basico
progreso: pendiente
estado: pendiente
area: Redes
tags: [redes]
---
# Laboratorios de Packet Tracer (bitácora)

Parte de: [[19-Redes/08 Cisco IOS y práctica/_Índice Cisco IOS y práctica|Cisco IOS y práctica]]

> [!info] Cómo avanzar
> `progreso`: **pendiente** → **viendo** (estudiando) → **visto** (contestaste la autoevaluación sin abrir las respuestas) → **dominado** (hiciste el laboratorio o lo aplicaste de verdad).

## Contenido

### Qué es
La **bitácora de laboratorios** es tu registro personal de cada práctica que haces en [[Cisco Packet Tracer]] (o en equipos reales): qué hiciste, qué configuraste, qué falló y qué aprendiste. Funciona como el historial de commits de un proyecto: con el tiempo se vuelve tu mejor material de repaso para el CCNA, tu prueba de experiencia práctica y tu guía de soluciones a errores que ya viviste.

### Cómo funciona
**Qué registrar en cada entrada**:
1. **Fecha y duración**.
2. **Objetivo**: qué querías practicar (por ejemplo, "router on a stick con tres VLAN").
3. **Topología**: lista de equipos, direccionamiento y un enlace al archivo `.pkt` (guárdalo con nombre claro: `2026-11-10_ROAS-3VLAN.pkt`).
4. **Configuración clave**: los comandos importantes (no hace falta toda la running-config).
5. **Verificación**: qué comandos usaste para comprobar y qué resultado esperabas.
6. **Errores encontrados**: el síntoma, la causa y cómo lo resolviste.
7. **Aprendizajes y dudas**: lo que entendiste y lo que quedó pendiente de investigar.
8. **Notas relacionadas**: enlaces a las notas teóricas de esta bóveda.

**Por qué guardar también los errores**: los errores son la parte más valiosa del registro. Te muestran tus puntos débiles (si tres veces olvidaste `no shutdown` en una subinterfaz física, ya sabes qué revisar primero), aceleran el diagnóstico la próxima vez porque reconoces el síntoma, y son exactamente lo que el examen pone en preguntas de troubleshooting. Además, repasar "por qué falló" fija mejor el conocimiento que repetir lo que salió bien.

**Ritmo sugerido**: un laboratorio corto por cada tema estudiado, y cada dos semanas uno integrador que combine varios (VLAN + OSPF + DHCP + NAT + ACL). Rehaz los laboratorios con errores una semana después sin mirar la solución.

### Ejemplo
Plantilla en Markdown para tu bóveda de Obsidian:

```markdown
## 2026-11-10 · Router on a stick con 3 VLAN (45 min)
**Objetivo:** enrutar VLAN 10, 20 y 99 con un solo enlace trunk.
**Archivo:** `Laboratorios/2026-11-10_ROAS-3VLAN.pkt`
**Direccionamiento:** VLAN 10 192.168.10.0/24 · VLAN 20 192.168.20.0/24 · VLAN 99 nativa 192.168.99.0/24

**Configuración clave**
- R1: g0/0.10, g0/0.20, g0/0.99 native; `no shutdown` en g0/0
- SW1: g0/1 trunk, allowed 10,20,99, native 99

**Verificación**
- `show vlans` en R1: las tres subinterfaces con paquetes
- PC10 → PC20 ping OK; tracert pasa por 192.168.10.1

**Errores**
- Síntoma: PC10 no llegaba a su gateway. Causa: g0/0 física apagada. Solución: `no shutdown` en g0/0.
- Síntoma: CDP avisaba native VLAN mismatch. Causa: nativa 1 en SW1. Solución: `switchport trunk native vlan 99`.

**Aprendí:** el número de subinterfaz es solo un nombre; lo que manda es `encapsulation dot1Q`.
**Pendiente:** probar lo mismo con switch capa 3 y comparar.
**Notas:** [[Router on a stick]] · [[VLAN nativa]]
```

Puedes llevar además una tabla resumen con Dataview o Bases en Obsidian (fecha, tema, resultado, errores) para ver tu avance.

Laboratorios guiados de esta área para registrar en la bitácora: [[Lab 1 - Encapsulamiento, hub y switch en modo simulación]], [[Lab 1 - VLANs, trunk y router on a stick en Packet Tracer]], [[Lab 2 - STP y EtherChannel en Packet Tracer]], [[Lab 1 - Subredes IPv4 en Packet Tracer]], [[Lab 2 - IPv6 con SLAAC en Packet Tracer]], [[Lab 1 - DHCP y NAT-PAT en Packet Tracer]], [[Lab 1 - Rutas estáticas y OSPF de área única en Packet Tracer]], [[Lab 1 - Wi-Fi básico en Packet Tracer]], [[Lab 1 - ACLs estándar y extendidas en Packet Tracer]], [[Lab 1 - SSH y respaldo de configuración en Packet Tracer]] y [[Lab 2 - Wireshark capturando ARP, ICMP y DNS]].

### Cuándo usarlo y cuándo no
Usa la bitácora en cada práctica, por pequeña que sea: diez minutos de registro valen más que una hora de laboratorio olvidado. No copies toda la configuración ni capturas de pantalla sin explicación; prioriza lo que te ayudará a recordar y diagnosticar.

### Errores comunes
- **Registrar solo lo que funcionó**.
- **No guardar el archivo `.pkt`** o nombrarlo `prueba1.pkt`.
- **Escribir la bitácora días después**: ya olvidaste los detalles.
- **No enlazar la teoría**: pierdes la conexión entre concepto y práctica.

### Para profundizar
- [Cisco Packet Tracer en Networking Academy](https://www.netacad.com/cisco-packet-tracer)
- [Cisco: examen 200-301 CCNA](https://www.cisco.com/site/us/en/learn/training-certifications/exams/ccna.html)

## Autoevaluación

Contesta primero en voz alta o en papel; luego despliega la respuesta.

> [!question]- ¿Qué debe registrar cada entrada de la bitácora?
> Fecha, laboratorio, objetivo, topología (archivo `.pkt`), comandos clave, problemas encontrados y cómo se resolvieron, y qué falta practicar.

> [!question]- ¿Por qué guardar también los errores?
> Son lo que más se repite en exámenes y en el trabajo; tener tu propia lista de causas y soluciones acelera el troubleshooting.

## Práctica

- [[Lab 1 - SSH y respaldo de configuración en Packet Tracer]]

