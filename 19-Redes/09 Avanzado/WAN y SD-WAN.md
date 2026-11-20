---
proyecto: Conocimiento
tipo: concepto
nivel: avanzado
progreso: pendiente
estado: pendiente
area: Redes
tags: [redes]
---
# WAN y SD-WAN

Parte de: [[19-Redes/09 Avanzado/_Índice Avanzado|Avanzado]]

> [!info] Cómo avanzar
> `progreso`: **pendiente** → **viendo** (estudiando) → **visto** (contestaste la autoevaluación sin abrir las respuestas) → **dominado** (hiciste el laboratorio o lo aplicaste de verdad).

## Contenido

### Qué es
Una **WAN** (Wide Area Network) conecta sitios separados geográficamente: oficinas, sucursales, centros de datos y la nube. Normalmente se construye con servicios **rentados a proveedores**. **SD-WAN** (Software-Defined WAN) es una forma moderna de gestionarla: combina varios enlaces (MPLS, internet, 4G/5G) bajo un controlador central que crea túneles cifrados y elige dinámicamente el mejor camino para cada aplicación.

### Cómo funciona
**Tecnologías WAN tradicionales**:
- **Líneas dedicadas** (punto a punto): un circuito exclusivo entre dos sitios. Muy estable y caro.
- **MPLS** (Multiprotocol Label Switching): el proveedor transporta tu tráfico por su red privada con etiquetas; da VPN de capa 3 entre todas tus sedes, con calidad de servicio y SLA. Confiable pero costoso y lento de aprovisionar.
- **Metro Ethernet**: servicios Ethernet del proveedor dentro de una ciudad o región.
- **Banda ancha** (fibra hasta el hogar, cable, DSL): internet barato, sin garantías.
- **Celular 4G/5G**: rápido de instalar, útil como respaldo o para sitios temporales.
- **Satelital**: zonas remotas.
- **VPN sobre internet** (IPsec sitio a sitio): la alternativa económica a MPLS (ver [[VPN sitio a sitio]]).
- Topologías: punto a punto, hub-and-spoke, malla completa o parcial (ver [[Topologías de red]]).

**SD-WAN**:
1. Cada sitio tiene un **equipo de borde** SD-WAN conectado a uno o varios transportes (MPLS, internet, 5G).
2. Un **controlador** central (en la solución de Cisco, Catalyst SD-WAN, los roles son Manager para la gestión, Controller para el plano de control y Validator para la orquestación inicial) distribuye políticas y llaves.
3. Los equipos de borde forman **túneles IPsec** entre sí sobre todos los transportes, creando un overlay.
4. Miden continuamente **latencia, jitter y pérdida** en cada túnel.
5. Las políticas dicen, por aplicación, qué SLA necesita: "la voz por el camino con menos de 150 ms y menos de 1% de pérdida; los respaldos por el más barato". Si un camino se degrada, el tráfico cambia de inmediato.
6. Se integra con la nube y con servicios de seguridad (SASE) para que las sucursales salgan directo a SaaS sin pasar por la oficina central.

**Ventajas de SD-WAN frente a solo MPLS**:
- **Costo**: aprovechar internet de banda ancha, más barato por Mbps.
- **Resiliencia**: varios transportes activos al mismo tiempo, con conmutación automática.
- **Rendimiento por aplicación**: elige el camino según la calidad medida, no solo según la ruta.
- **Gestión centralizada** y despliegue rápido (*zero-touch provisioning*: el equipo llega a la sucursal, se conecta y descarga su configuración).
- **Acceso directo a la nube** desde cada sucursal.
- Cifrado de extremo a extremo en todos los túneles.

### Ejemplo
Una cadena con 40 sucursales en México:
- Antes: MPLS en todas las sucursales hacia el centro de datos en CDMX; internet solo por la central. Teams y Microsoft 365 se sentían lentos en las sucursales lejanas.
- Con SD-WAN: cada sucursal tiene fibra comercial más un módem 5G; la central conserva MPLS. La voz y el punto de venta van por el mejor camino medido; Microsoft 365 sale directo a internet desde cada sucursal; si cae la fibra, el 5G entra en segundos.

En un router tradicional, la idea básica de "dos caminos con respaldo" se haría con IP SLA y rutas flotantes:

```text
R1(config)# ip sla 1
R1(config-ip-sla)# icmp-echo 8.8.8.8 source-interface g0/1
R1(config-ip-sla)# frequency 10
R1(config)# ip sla schedule 1 life forever start-time now
R1(config)# track 1 ip sla 1 reachability
R1(config)# ip route 0.0.0.0 0.0.0.0 203.0.113.1 track 1
R1(config)# ip route 0.0.0.0 0.0.0.0 198.51.100.1 250
```

SD-WAN generaliza esto por aplicación, con métricas de calidad y gestión central.

### Cuándo usarlo y cuándo no
SD-WAN conviene con muchas sucursales, uso intensivo de nube y SaaS, o necesidad de bajar costos de MPLS. Para dos o tres sitios, una VPN IPsec bien configurada con respaldo puede bastar. MPLS sigue siendo útil para aplicaciones con requisitos estrictos y como uno de los transportes dentro de la SD-WAN.

### Errores comunes
- **Pensar que SD-WAN elimina la necesidad de buenos enlaces**: no crea ancho de banda.
- **Dos enlaces de internet del mismo proveedor y por la misma ruta física**: la redundancia es aparente.
- **No definir políticas por aplicación**: se desaprovecha la principal ventaja.
- **Olvidar la seguridad en la salida directa a internet** de cada sucursal.

### Para profundizar
- [Cisco Catalyst SD-WAN: documentación](https://www.cisco.com/c/en/us/support/routers/sd-wan/series.html)
- [RFC 3031: arquitectura MPLS](https://www.rfc-editor.org/rfc/rfc3031)

## Autoevaluación

Contesta primero en voz alta o en papel; luego despliega la respuesta.

> [!question]- ¿Qué tecnologías WAN tradicionales existen?
> Líneas dedicadas, **MPLS** del proveedor, Metro Ethernet, banda ancha (fibra, cable) con VPN y enlaces celulares 4G/5G.

> [!question]- ¿Qué es SD-WAN?
> Una WAN definida por software: un controlador central orquesta varios transportes (MPLS, Internet, 5G) con túneles cifrados y elige el camino por aplicación según latencia, pérdida y jitter.

> [!question]- ¿Qué ventajas tiene SD-WAN sobre MPLS solo?
> Usa enlaces de Internet más baratos, configuración centralizada (*zero-touch provisioning*), conmutación automática ante fallas y visibilidad por aplicación.

