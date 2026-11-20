---
proyecto: Conocimiento
tipo: concepto
nivel: intermedio
progreso: pendiente
estado: pendiente
area: Redes
tags: [redes]
---
# OSPF de área única

Parte de: [[OSPF]]

> [!info] Cómo avanzar
> `progreso`: **pendiente** → **viendo** (estudiando) → **visto** (contestaste la autoevaluación sin abrir las respuestas) → **dominado** (hiciste el laboratorio o lo aplicaste de verdad).

## Contenido

### Qué es
**OSPF de área única** es la configuración más común para empezar: todos los routers y enlaces pertenecen al **área 0** (backbone). Es el alcance que exige el CCNA (OSPFv2 de área única para IPv4 y OSPFv3 para IPv6). En redes pequeñas y medianas, una sola área funciona perfectamente; cuando crece mucho se pasa a [[OSPF multiárea]].

### Cómo funciona
**Pasos de configuración**:
1. Crear el proceso: `router ospf <id-proceso>` (número local).
2. Fijar el **router ID**: `router-id x.x.x.x`.
3. Activar OSPF en las interfaces, de una de dos formas:
   - `network <dirección> <wildcard> area 0`: activa OSPF en toda interfaz cuya IP coincida con la dirección y la wildcard.
   - `ip ospf <id-proceso> area 0` directamente en cada interfaz (más claro y preciso).
4. Marcar como **pasivas** las interfaces hacia usuarios: `passive-interface <interfaz>`. Se sigue anunciando la red, pero no se envían hellos; nadie puede formar vecindad por ahí (seguridad y menos ruido). También `passive-interface default` y luego `no passive-interface` en los enlaces entre routers.
5. Si un router tiene la salida a internet, anunciar la ruta por defecto a los demás: `default-information originate` (requiere que ese router tenga una ruta por defecto en su tabla, salvo que uses `always`).

**Costo**: costo = ancho de banda de referencia ÷ ancho de banda de la interfaz (mínimo 1). Con la referencia por defecto de **100 Mbps**: Ethernet 10 Mbps = 10, FastEthernet = 1, Gigabit = 1, 10 Gigabit = 1. Como todo lo de 100 Mbps o más vale 1, OSPF no distingue entre FastEthernet y 10G. Por eso se sube la referencia en **todos** los routers con `auto-cost reference-bandwidth <Mbps>`. Con referencia de 1000 Mbps: FastEthernet = 10, Gigabit = 1, 10 Gigabit = 1. Con 100000 (100 Gbps): FastEthernet = 1000, Gigabit = 100, 10 Gigabit = 10. También se puede fijar el costo a mano con `ip ospf cost`.

El costo de un camino es la **suma de los costos de las interfaces de salida** a lo largo del camino.

**Wildcard del comando network**: es la inversa de la máscara. Para `192.168.10.0/24` → `0.0.0.255`; para `10.0.12.4/30` → `0.0.0.3`; para `10.50.8.0/21` → `0.0.7.255`. También vale `network 10.0.12.5 0.0.0.0 area 0` para activar solo la interfaz con esa IP exacta.

### Ejemplo
R1 con LAN `192.168.10.0/24` (G0/0), enlace a R2 `10.0.12.4/30` (G0/1) y salida a internet:

```text
R1(config)# router ospf 1
R1(config-router)# router-id 1.1.1.1
R1(config-router)# auto-cost reference-bandwidth 1000
R1(config-router)# network 192.168.10.0 0.0.0.255 area 0
R1(config-router)# network 10.0.12.4 0.0.0.3 area 0
R1(config-router)# passive-interface g0/0
R1(config-router)# default-information originate
R1(config-router)# exit
R1(config)# ip route 0.0.0.0 0.0.0.0 203.0.113.1
R1(config)# end
```

Verificación:

```text
R1# show ip ospf neighbor              ! vecinos en FULL (o 2-WAY entre DROTHER)
R1# show ip ospf interface brief       ! interfaces, área, costo, estado DR/BDR/P2P
R1# show ip route ospf                 ! rutas O y O*E2 (la por defecto en los demás)
R1# show ip protocols                  ! redes, pasivas, router ID
```

En R2 la ruta por defecto aparecerá como `O*E2 0.0.0.0/0`.

Calcular costos con Python para comparar caminos:

```python
ref = 1000  # Mbps (auto-cost reference-bandwidth 1000)
def costo(mbps): return max(1, ref // mbps)
print(costo(100), costo(1000), costo(10000))      # 10 1 1
print("vía FE directo:", costo(100), " vía 2 Gigabit:", costo(1000) * 2)  # 10 vs 2
```

Con referencia 1000, un camino de dos saltos Gigabit (2) gana a uno directo por FastEthernet (10). Practica en el [[Lab 1 - Rutas estáticas y OSPF de área única en Packet Tracer]].

### Cuándo usarlo y cuándo no
Área única basta para la mayoría de redes de hasta algunas decenas de routers. Usa siempre router ID explícito, `passive-interface` en LAN y la misma referencia de ancho de banda en todos los routers.

### Errores comunes
- **Cambiar la referencia en un solo router**: costos inconsistentes.
- **Wildcard incorrecta** en `network`: la interfaz no entra a OSPF o entra una que no debía.
- **Olvidar `passive-interface`** en LAN de usuarios.
- **`default-information originate` sin ruta por defecto** en el router que la anuncia.

### Para profundizar
- [RFC 2328: OSPF versión 2](https://www.rfc-editor.org/rfc/rfc2328)
- [Cisco: OSPF Design Guide](https://www.cisco.com/c/en/us/support/docs/ip/open-shortest-path-first-ospf/7039-1.html)

## Ejercicios

Resuélvelos en papel o en la herramienta; la solución está plegada.

> [!question]- Con ancho de banda de referencia de 100 Mbps, ¿qué costo tienen FastEthernet, Ethernet 10 Mbps, GigabitEthernet y un serial de 1544 kbps?
> FastEthernet **1** · Ethernet **10** · GigabitEthernet **1** (el mínimo es 1) · Serial T1 **64** (100 000 / 1544 ≈ 64).

> [!question]- Configuras `auto-cost reference-bandwidth 10000` (10 Gbps). ¿Nuevos costos de FastEthernet, Gigabit y 10 Gigabit?
> FastEthernet **100** · Gigabit **10** · 10 Gigabit **1**. Configúralo igual en todos los routers.

> [!question]- PC1 (LAN de R1) a PC3 (LAN de R3), todo Gigabit con referencia 100. Camino A: R1→R3 directo. Camino B: R1→R2→R3. ¿Costo de cada uno?
> Se suman las interfaces de **salida** del camino hasta la red destino. A: enlace R1-R3 (1) + LAN de R3 (1) = **2**. B: 1 + 1 + 1 = **3**. OSPF elige A.

> [!question]- Escribe la wildcard del `network` para 10.0.12.0/30 y para 172.16.0.0/20.
> /30 → `0.0.0.3`. /20 → `0.0.15.255`.

## Autoevaluación

Contesta primero en voz alta o en papel; luego despliega la respuesta.

> [!question]- ¿Qué comandos configuran OSPF en área 0 para 192.168.1.0/24 y 10.0.12.0/30?
> ```
> router ospf 1
>  router-id 1.1.1.1
>  network 192.168.1.0 0.0.0.255 area 0
>  network 10.0.12.0 0.0.0.3 area 0
>  passive-interface g0/0
> ```
> Alternativa por interfaz: `ip ospf 1 area 0`.

> [!question]- ¿Para qué sirve `passive-interface`?
> Anuncia la red de esa interfaz pero **no envía hellos** por ella; se usa en interfaces hacia hosts para no formar vecinos ni exponer OSPF.

> [!question]- ¿Qué estado deben tener los vecinos y cómo lo verificas?
> **FULL** (o 2-WAY entre routers que no son DR/BDR). Se ve con `show ip ospf neighbor`; las rutas con `show ip route ospf`.

> [!question]- ¿Cómo se propaga la ruta por defecto en OSPF?
> En el router de borde con una default configurada: `default-information originate`. Los demás la ven como `O*E2`.

## Práctica

- [[Lab 1 - Rutas estáticas y OSPF de área única en Packet Tracer]]

