---
proyecto: Conocimiento
tipo: concepto
nivel: intermedio
progreso: pendiente
estado: pendiente
area: Redes
tags: [redes]
---
# Enrutamiento dinámico (conceptos)

Parte de: [[19-Redes/05 Routing/_Índice Routing|Routing]]

> [!info] Cómo avanzar
> `progreso`: **pendiente** → **viendo** (estudiando) → **visto** (contestaste la autoevaluación sin abrir las respuestas) → **dominado** (hiciste el laboratorio o lo aplicaste de verdad).

## Contenido

### Qué es
El **enrutamiento dinámico** es el uso de **protocolos** con los que los routers intercambian información de las redes que conocen, calculan automáticamente los mejores caminos y **se adaptan a los cambios**: si un enlace cae, recalculan y usan otro camino sin intervención humana. Es la alternativa a configurar rutas estáticas en cada router. Esta nota da el panorama; los detalles están en [[Vector distancia vs estado de enlace]] y [[Distancia administrativa y métrica]].

### Cómo funciona
Todo protocolo dinámico resuelve tres tareas:
1. **Descubrir vecinos** (normalmente con mensajes hello).
2. **Intercambiar información** de rutas o del estado de los enlaces.
3. **Calcular el mejor camino** con su métrica e instalarlo en la tabla de rutas.

Ventajas frente a las estáticas: **escalan** (agregar una red se anuncia sola), **convergen** ante fallas y permiten **balanceo** por varios caminos. Desventajas: consumen CPU, memoria y algo de ancho de banda, requieren diseño y su configuración errónea puede propagar problemas a toda la red; también deben protegerse (autenticación entre vecinos).

**Clasificación por alcance**:
- **IGP** (Interior Gateway Protocol): dentro de un sistema autónomo, es decir, de una organización. Ejemplos: **OSPF**, **EIGRP**, **IS-IS**, RIP.
- **EGP** (Exterior Gateway Protocol): entre sistemas autónomos distintos, como entre tu empresa y dos proveedores, o entre proveedores en internet. Hoy el único en uso es **BGP**.

**Clasificación por algoritmo**: vector distancia (RIP, EIGRP como "vector distancia avanzado"), estado de enlace (OSPF, IS-IS) y vector de ruta (BGP).

**Convergencia**: es el estado en que **todos los routers tienen una visión coherente** de la red tras un cambio. El tiempo de convergencia es lo que tarda en volver a ese estado después de que cae o aparece un enlace. Durante la convergencia puede haber pérdida de paquetes o bucles temporales. OSPF y EIGRP convergen en segundos (o menos con BFD); RIP podía tardar minutos.

| Protocolo | Tipo | Métrica | AD (Cisco) | Notas |
|---|---|---|---|---|
| RIPv2 | Vector distancia | Saltos (máx. 15) | 120 | Didáctico, casi en desuso |
| EIGRP | Vector distancia avanzado | Ancho de banda y retardo | 90 interna / 170 externa | Origen Cisco |
| OSPF | Estado de enlace | Costo | 110 | Estándar abierto, el más usado en empresas |
| IS-IS | Estado de enlace | Costo | 115 | Muy usado por proveedores |
| BGP | Vector de ruta | Atributos | 20 eBGP / 200 iBGP | Internet |

### Ejemplo
Tres routers en triángulo con OSPF. Si cae el enlace directo R1–R3, OSPF detecta la caída (pierde los hellos o la interfaz baja), inunda la actualización, cada router recalcula y R1 llega a la LAN de R3 por R2. Con estáticas, tendrías que haber previsto ese respaldo con rutas flotantes en cada router.

```text
R1(config)# router ospf 1
R1(config-router)# router-id 1.1.1.1
R1(config-router)# network 10.0.0.0 0.0.255.255 area 0
R1(config-router)# network 192.168.1.0 0.0.0.255 area 0
R1(config-router)# passive-interface g0/0
R1(config-router)# end
R1# show ip protocols
R1# show ip ospf neighbor
R1# show ip route ospf
```

`show ip protocols` resume qué protocolos corren, qué redes anuncian, de quién aprenden y su AD.

### Cuándo usarlo y cuándo no
Usa un IGP (OSPF es la apuesta segura y la que domina el CCNA) en cuanto tengas más de dos o tres routers o cualquier redundancia. Usa BGP cuando te conectas a varios proveedores o en la nube para intercambiar rutas con VPN y ExpressRoute. Mantén estáticas para la ruta por defecto y casos puntuales. Es común combinarlos: OSPF interno, estática por defecto hacia el proveedor y redistribución controlada.

### Errores comunes
- **Anunciar redes que no deberían** (incluidas las de internet hacia la red interna o al revés).
- **Formar vecindades por interfaces hacia usuarios**: usa `passive-interface`.
- **No autenticar los vecinos**: un router no autorizado puede inyectar rutas.
- **Mezclar protocolos sin entender la AD**: el que gana puede no ser el que esperas.

### Para profundizar
- [RFC 2328: OSPF versión 2](https://www.rfc-editor.org/rfc/rfc2328)
- [RFC 4271: BGP-4](https://www.rfc-editor.org/rfc/rfc4271)
- [RFC 7868: EIGRP (informativo)](https://www.rfc-editor.org/rfc/rfc7868)

## Subtemas

- [[Vector distancia vs estado de enlace]]
- [[Distancia administrativa y métrica]]

## Autoevaluación

Contesta primero en voz alta o en papel; luego despliega la respuesta.

> [!question]- ¿Qué ventaja tiene el enrutamiento dinámico sobre el estático?
> Los routers intercambian rutas automáticamente y se adaptan a cambios o fallas sin intervención manual; escala mejor.

> [!question]- ¿Qué diferencia hay entre IGP y EGP?
> **IGP** enruta dentro de un sistema autónomo (OSPF, EIGRP, RIP, IS-IS). **EGP** enruta entre sistemas autónomos (BGP).

> [!question]- ¿Qué es la convergencia?
> El momento en que todos los routers tienen una visión consistente de la red tras un cambio. Menor tiempo de convergencia = menos pérdida de tráfico.

## Práctica

- [[Lab 1 - Rutas estáticas y OSPF de área única en Packet Tracer]]

## Progreso de los subtemas

```base
filters:
  and:
    - file.inFolder("19-Redes/05 Routing/Enrutamiento dinámico (conceptos)")
    - file.name != "Enrutamiento dinámico (conceptos)"
views:
  - type: table
    name: Progreso
    order:
      - file.name
      - progreso
      - nivel
```
