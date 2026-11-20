---
proyecto: Conocimiento
tipo: concepto
nivel: intermedio
progreso: pendiente
estado: pendiente
area: Redes
tags: [redes]
---
# Distancia administrativa y métrica

Parte de: [[Enrutamiento dinámico (conceptos)]]

> [!info] Cómo avanzar
> `progreso`: **pendiente** → **viendo** (estudiando) → **visto** (contestaste la autoevaluación sin abrir las respuestas) → **dominado** (hiciste el laboratorio o lo aplicaste de verdad).

## Contenido

### Qué es
Son los dos criterios con los que un router elige entre rutas **al mismo prefijo exacto**:
- La **distancia administrativa (AD)** mide la **confianza en la fuente** de la ruta (qué protocolo o mecanismo la aprendió). Se usa para comparar rutas de **fuentes distintas**. Menor es mejor.
- La **métrica** mide **qué tan bueno es el camino** dentro de un mismo protocolo (saltos, costo, ancho de banda y retardo). Se usa para comparar rutas del **mismo protocolo**. Menor es mejor.

Recuerda que ambas se aplican **después** de la regla de prefijo más largo: la ruta más específica gana siempre (ver [[Cómo decide un router (tabla de rutas)]]).

### Cómo funciona
**AD predeterminadas en Cisco IOS**:

| Fuente | AD |
|---|---|
| Conectada | 0 |
| Estática | 1 |
| eBGP | 20 |
| EIGRP interna | 90 |
| OSPF | 110 |
| IS-IS | 115 |
| RIP | 120 |
| EIGRP externa | 170 |
| iBGP | 200 |
| Desconocida / no confiable | 255 (nunca se instala) |

Puedes cambiar la AD de una estática (para hacerla **flotante**) o la de un protocolo (`distance` dentro de `router ospf`).

**Métricas de cada protocolo**:
- **RIP**: número de **saltos** (máximo 15; 16 es inalcanzable). No considera la velocidad de los enlaces.
- **OSPF**: **costo** = ancho de banda de referencia ÷ ancho de banda de la interfaz, sumado a lo largo del camino. Referencia por defecto: 100 Mbps.
- **EIGRP**: métrica compuesta; por defecto usa el **ancho de banda mínimo** del camino y la **suma de retardos** (constantes K1 y K3). Puede incluir carga y confiabilidad, aunque no se recomienda.
- **BGP**: no tiene métrica simple; elige con atributos (AS-PATH, local preference, MED…).

En `show ip route` cada ruta dinámica muestra `[AD/métrica]`.

### Ejemplo
R1 aprende rutas por varias fuentes:

```text
R1# show ip route 10.40.0.0
Routing entry for 10.40.0.0/16
  Known via "eigrp 100", distance 90, metric 30720
  ...
```

Casos:
1. `10.40.0.0/16` llega por **EIGRP** (AD 90) y por **OSPF** (AD 110) → se instala **EIGRP**. La de OSPF queda en la base de datos de OSPF pero no en la tabla de rutas.
2. Agregas `ip route 10.40.0.0 255.255.0.0 10.0.99.2 230` → con AD 230 es **flotante**: solo entra si desaparecen las de EIGRP y OSPF.
3. Además, OSPF anuncia `10.40.8.0/24`. Un paquete a `10.40.8.15` va por la ruta de **OSPF** /24, aunque OSPF tenga peor AD, porque el prefijo es más largo. Un paquete a `10.40.20.1` usa la /16 de EIGRP.
4. Dentro de OSPF, si hay dos caminos a la misma red con costos 3 y 5, gana el de **costo 3**. Si ambos tienen costo 3, se instalan los dos (balanceo).

Ver y modificar:

```text
R1# show ip protocols | include Distance|Routing Protocol
R1(config)# router ospf 1
R1(config-router)# distance 95          ! cambia la AD de OSPF en este router
R1(config)# interface g0/1
R1(config-if)# ip ospf cost 50          ! influye en la métrica de OSPF
```

Cambiar la AD de un protocolo es una medida delicada: afecta solo al router donde se aplica y puede crear caminos asimétricos. Úsala con documentación clara.

### Cuándo usarlo y cuándo no
Usa la AD para **respaldos** (rutas flotantes) y para controlar la preferencia durante migraciones entre protocolos. Usa la métrica (por ejemplo, el costo de OSPF) para **ingeniería de tráfico** dentro de un protocolo: preferir el enlace de fibra frente al de respaldo. No intentes comparar métricas de protocolos distintos: no son comparables (30720 de EIGRP no es "peor" que 3 de OSPF).

### Errores comunes
- **Aplicar la AD antes que el prefijo más largo**.
- **Comparar métricas entre protocolos**.
- **Ruta flotante con AD menor que la principal**.
- **Olvidar que la AD es local**: no se anuncia a los vecinos.

### Para profundizar
- [Cisco: What Is Administrative Distance?](https://www.cisco.com/c/en/us/support/docs/ip/border-gateway-protocol-bgp/15986-admin-distance.html)
- [Cisco: Route Selection in Cisco Routers](https://www.cisco.com/c/en/us/support/docs/ip/enhanced-interior-gateway-routing-protocol-eigrp/8651-21.html)

## Ejercicios

Resuélvelos en papel o en la herramienta; la solución está plegada.

> [!question]- Un router aprende 172.16.0.0/16 por OSPF y por EIGRP. ¿Cuál instala?
> **EIGRP** (AD 90 < 110).

> [!question]- Además hay una estática a 172.16.0.0/16 con AD 200. ¿Cuándo se usa?
> Solo si desaparecen las rutas de EIGRP y OSPF (es flotante).

> [!question]- OSPF anuncia 172.16.5.0/24 y existe una estática a 172.16.0.0/16. ¿A dónde va 172.16.5.9?
> Por la ruta **OSPF /24**. Son prefijos distintos, así que ambos se instalan y decide la coincidencia más larga; la AD solo compara rutas al **mismo** prefijo.

## Autoevaluación

Contesta primero en voz alta o en papel; luego despliega la respuesta.

> [!question]- ¿Qué diferencia hay entre distancia administrativa (AD) y métrica?
> **AD** compara la confiabilidad de **distintas fuentes** de rutas. **Métrica** compara rutas **del mismo protocolo**.

> [!question]- Da las AD predeterminadas de Cisco más comunes.
> Conectada 0 · Estática 1 · eBGP 20 · EIGRP 90 · OSPF 110 · RIP 120 · EIGRP externo 170 · iBGP 200.

> [!question]- ¿Qué métricas usan RIP, OSPF y EIGRP?
> RIP: **saltos**. OSPF: **costo** (basado en ancho de banda). EIGRP: compuesta, por defecto **ancho de banda y retardo**.

## Práctica

- [[Lab 1 - Rutas estáticas y OSPF de área única en Packet Tracer]]

