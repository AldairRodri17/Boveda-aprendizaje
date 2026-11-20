---
proyecto: Conocimiento
tipo: concepto
nivel: intermedio
progreso: pendiente
estado: pendiente
area: Redes
tags: [redes]
---
# EIGRP

Parte de: [[19-Redes/05 Routing/_Índice Routing|Routing]]

> [!info] Cómo avanzar
> `progreso`: **pendiente** → **viendo** (estudiando) → **visto** (contestaste la autoevaluación sin abrir las respuestas) → **dominado** (hiciste el laboratorio o lo aplicaste de verdad).

## Contenido

### Qué es
**EIGRP** (Enhanced Interior Gateway Routing Protocol) es un protocolo de enrutamiento interior creado por Cisco, de tipo **vector distancia avanzado**. En 2016 Cisco publicó su funcionamiento básico como RFC informativo (RFC 7868), pero en la práctica sigue siendo casi exclusivo de equipos Cisco. Destaca por su convergencia muy rápida gracias al algoritmo **DUAL**, su métrica que considera ancho de banda y retardo, y por permitir **balanceo de carga por caminos de costo desigual**. No es tema central del examen CCNA actual, pero lo encontrarás en redes Cisco existentes.

### Cómo funciona
1. **Vecinos**: envía hellos a `224.0.0.10`. Para ser vecinos deben coincidir el **número de sistema autónomo** (`router eigrp 100` en ambos), la subred, los valores K de la métrica y la autenticación.
2. **Actualizaciones**: no envía la tabla periódicamente; solo envía cambios, de forma confiable (con su protocolo RTP de transporte).
3. **Tablas**: tabla de vecinos, **tabla de topología** (todas las rutas aprendidas de todos los vecinos) y tabla de rutas.
4. **Métrica**: por defecto combina el **ancho de banda mínimo** del camino y la **suma de los retardos** de las interfaces.
5. **DUAL** elige, para cada destino:
   - **Distancia factible (FD)**: la mejor métrica total hacia el destino desde este router.
   - **Distancia reportada (RD o AD)**: la métrica que anuncia el vecino desde él hasta el destino.
   - **Sucesor**: el vecino con la mejor ruta (la FD). Es el que se instala en la tabla de rutas.
   - **Sucesor factible**: un vecino de respaldo que cumple la **condición de factibilidad**: su distancia reportada es **menor que la FD** actual. Eso garantiza que no pasa a través de este router (no hay bucle). Si el sucesor falla y hay sucesor factible, el cambio es **inmediato**, sin preguntar a nadie.
   - Si no hay sucesor factible, la ruta pasa a estado **activo** y el router consulta a sus vecinos (queries) para encontrar una alternativa.

**Lo que EIGRP permite y OSPF no de forma nativa**: balanceo por **costo desigual** con el comando `variance`. Con `variance 2`, se instalan también caminos cuya métrica sea hasta el doble de la mejor (siempre que sean sucesores factibles), y el tráfico se reparte en proporción. OSPF solo balancea entre caminos de **igual** costo.

AD: 90 para rutas internas y 170 para externas (redistribuidas). Los equipos modernos usan la configuración en **modo nombrado** (`router eigrp NOMBRE`), con más opciones y métricas de 64 bits.

### Ejemplo
EIGRP clásico en dos routers con AS 100:

```text
R1(config)# router eigrp 100
R1(config-router)# eigrp router-id 1.1.1.1
R1(config-router)# network 10.0.12.0 0.0.0.3
R1(config-router)# network 192.168.10.0 0.0.0.255
R1(config-router)# passive-interface g0/0
R1(config-router)# no auto-summary
R1(config-router)# variance 2
R1(config-router)# end
R1# show ip eigrp neighbors
R1# show ip eigrp topology
P 192.168.30.0/24, 1 successors, FD is 28416
        via 10.0.12.2 (28416/2816), GigabitEthernet0/1
        via 10.0.13.2 (30976/2816), GigabitEthernet0/2
R1# show ip route eigrp
```

En `(28416/2816)` el primer número es la métrica total (FD por ese camino) y el segundo la distancia reportada por el vecino. El segundo camino tiene RD 2816 < FD 28416: cumple la condición de factibilidad y es **sucesor factible**. Con `variance 2` (28416 × 2 = 56832 > 30976) también se instala y balancea.

### Cuándo usarlo y cuándo no
Úsalo si heredas una red 100% Cisco que ya lo usa y funciona bien, o donde el balanceo por costo desigual sea valioso. Para redes nuevas, multifabricante o para alinearte con el examen y la industria, prefiere **OSPF**. Nunca mezcles EIGRP y OSPF sin un plan de redistribución.

### Errores comunes
- **Número de AS distinto** entre routers: no forman vecindad (en OSPF el ID de proceso no importa; en EIGRP sí).
- **Olvidar `no auto-summary`** en versiones antiguas de IOS: resume por clase y causa problemas con redes discontinuas.
- **Esperar `variance` sobre un camino que no es sucesor factible**: no se instala.
- **Clasificarlo como estado de enlace**.

### Para profundizar
- [RFC 7868: Cisco's Enhanced Interior Gateway Routing Protocol (EIGRP)](https://www.rfc-editor.org/rfc/rfc7868)
- [Cisco: Enhanced Interior Gateway Routing Protocol (white paper técnico)](https://www.cisco.com/c/en/us/support/docs/ip/enhanced-interior-gateway-routing-protocol-eigrp/16406-eigrp-toc.html)

## Autoevaluación

Contesta primero en voz alta o en papel; luego despliega la respuesta.

> [!question]- ¿Qué es EIGRP?
> Un protocolo de vector distancia avanzado creado por Cisco (publicado como RFC 7868 informativo). Usa el algoritmo DUAL, converge rápido y su AD es 90 (externas 170).

> [!question]- ¿Qué son sucesor y sucesor factible?
> **Sucesor**: el mejor camino (en la tabla de rutas). **Sucesor factible**: respaldo sin bucles, listo para usarse al instante. Cumple la condición de factibilidad: su distancia reportada < distancia factible del sucesor.

> [!question]- ¿Qué permite EIGRP que OSPF no hace de forma nativa?
> Balanceo de carga entre caminos de **distinto costo** con `variance`.

