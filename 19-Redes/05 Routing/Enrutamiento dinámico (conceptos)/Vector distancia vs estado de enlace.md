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
# Vector distancia vs estado de enlace

Parte de: [[Enrutamiento dinámico (conceptos)]]

> [!info] Cómo avanzar
> `progreso`: **pendiente** → **viendo** (estudiando) → **visto** (contestaste la autoevaluación sin abrir las respuestas) → **dominado** (hiciste el laboratorio o lo aplicaste de verdad).

## Contenido

### Qué es
Son las dos grandes familias de algoritmos de los protocolos de enrutamiento interior:
- **Vector distancia** (distance vector): cada router conoce solo **la distancia y la dirección** (el vecino) para llegar a cada red, y lo aprende de lo que le cuentan sus vecinos. "Enrutamiento por rumores". Ejemplo: **RIP**; **EIGRP** es un vector distancia avanzado.
- **Estado de enlace** (link state): cada router conoce el **mapa completo** de su área (todos los routers y enlaces con sus costos) y calcula por sí mismo el árbol de caminos más cortos con el algoritmo de **Dijkstra (SPF)**. Ejemplos: **OSPF** e **IS-IS**.

### Cómo funciona
**Vector distancia**:
1. Cada router comparte con sus vecinos su tabla de rutas (o los cambios), con la métrica a cada red.
2. Quien recibe suma el costo del enlace y, si la ruta es mejor que la que tenía, la adopta con ese vecino como siguiente salto.
3. RIP envía la tabla completa cada 30 segundos; EIGRP solo envía actualizaciones cuando hay cambios.
4. Problema clásico: **conteo al infinito**. Si una red cae, un router puede creer el rumor viejo de un vecino que, a su vez, la aprendió de él, y la métrica sube poco a poco formando un bucle.

**Mecanismos para evitar bucles en vector distancia**:
- **Métrica máxima**: en RIP, 16 saltos significa inalcanzable; corta el conteo al infinito.
- **Split horizon**: no anunciar una ruta por la misma interfaz por donde se aprendió.
- **Route poisoning**: cuando una red cae, anunciarla con métrica infinita en lugar de solo dejar de anunciarla.
- **Poison reverse**: devolver por la interfaz de origen la ruta envenenada para confirmar.
- **Temporizadores de hold-down**: durante un tiempo, ignorar rutas peores hacia una red que acaba de caer.
- **Actualizaciones desencadenadas** (triggered updates): avisar de inmediato en lugar de esperar al siguiente ciclo.
- EIGRP usa además el algoritmo **DUAL**, que garantiza caminos sin bucles con la condición de factibilidad.

**Estado de enlace**:
1. Cada router descubre a sus vecinos con hellos.
2. Describe sus enlaces en **LSA** (Link State Advertisements) y los **inunda** a todo el área.
3. Todos guardan las LSA en una base de datos idéntica (LSDB).
4. Cada uno ejecuta **SPF** poniéndose como raíz y obtiene los caminos más cortos.
5. Ante un cambio solo se inunda la LSA afectada y se recalcula: convergencia rápida y sin rumores.

| Característica | Vector distancia | Estado de enlace |
|---|---|---|
| Visión | Solo vecinos y distancias | Mapa completo del área |
| Actualizaciones | Periódicas o de la tabla (RIP); por cambios (EIGRP) | Por cambios (LSA) más refresco periódico largo |
| Recursos | Menos memoria y CPU | Más memoria y CPU (LSDB y SPF) |
| Convergencia | Lenta (RIP) a rápida (EIGRP) | Rápida |
| Escalabilidad | Limitada (RIP) | Alta con áreas |

### Ejemplo
Cadena R1 — R2 — R3 con la red `192.168.3.0/24` detrás de R3, con RIP. Si cae esa LAN y no hubiera split horizon, R2 podría volver a aprenderla de R1 ("yo la alcanzo a 2 saltos") y anunciarla a R3, y el valor iría subiendo en un bucle hasta 16. Con split horizon, R1 nunca le anuncia a R2 una ruta que aprendió de R2.

En OSPF puedes ver la base de datos que todos comparten:

```text
R1# show ip ospf database
R1# show ip ospf database router 3.3.3.3
R1# debug ip routing
```

Y en RIP (para comparar en un laboratorio de [[Cisco Packet Tracer]]):

```text
R1(config)# router rip
R1(config-router)# version 2
R1(config-router)# no auto-summary
R1(config-router)# network 10.0.0.0
R1# show ip rip database
```

### Cuándo usarlo y cuándo no
En redes empresariales actuales lo normal es un protocolo de **estado de enlace** (OSPF) por ser estándar, rápido y escalable. EIGRP sigue presente en redes Cisco existentes. RIP se usa casi solo para aprender o en equipos muy limitados.

### Errores comunes
- **Clasificar EIGRP como estado de enlace**: es vector distancia avanzado (a veces lo llaman "híbrido").
- **Pensar que estado de enlace no envía nada periódico**: hay hellos y un refresco de LSA cada 30 minutos.
- **Desactivar split horizon** sin una razón (en ciertos diseños hub-and-spoke es necesario, pero con cuidado).
- **Creer que todos los routers OSPF de la empresa tienen el mismo mapa**: solo dentro de cada área.

### Para profundizar
- [RFC 2453: RIP versión 2](https://www.rfc-editor.org/rfc/rfc2453)
- [RFC 2328: OSPF versión 2](https://www.rfc-editor.org/rfc/rfc2328)
- [RFC 7868: EIGRP (informativo)](https://www.rfc-editor.org/rfc/rfc7868)

## Autoevaluación

Contesta primero en voz alta o en papel; luego despliega la respuesta.

> [!question]- ¿Cómo funciona un protocolo de vector distancia?
> Cada router conoce solo lo que le dicen sus vecinos ("a la red X estoy a N saltos"). Ejemplo: RIP. EIGRP es vector distancia avanzado.

> [!question]- ¿Cómo funciona uno de estado de enlace?
> Cada router conoce el mapa completo del área (LSAs) y calcula el mejor camino con el algoritmo de Dijkstra (SPF). Ejemplos: OSPF, IS-IS.

> [!question]- ¿Qué mecanismos evitan bucles en vector distancia?
> Horizonte dividido (*split horizon*), envenenamiento de ruta (*route poisoning*), temporizadores de espera (*hold-down*) y máximo de saltos (16 = inalcanzable en RIP).

