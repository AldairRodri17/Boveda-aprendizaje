---
proyecto: Conocimiento
tipo: laboratorio
nivel: intermedio
progreso: pendiente
estado: borrador
area: Redes
herramienta: Cisco Packet Tracer
tags: [redes, laboratorio]
contenido: completo
---
# Lab 1 - Rutas estáticas y OSPF de área única en Packet Tracer

Parte de: [[19-Redes/05 Routing/_Índice Routing|Routing]]

**Cubre:** [[Cómo decide un router (tabla de rutas)]] · [[Rutas estáticas y ruta por defecto]] · [[Distancia administrativa y métrica]] · [[OSPF]] · [[OSPF de área única]] · [[Enrutamiento dinámico (conceptos)]]

> [!info] Al terminar
> Si todos los criterios quedan marcados, cambia a `dominado` el `progreso` de este laboratorio y de las notas que cubre. Guarda tus archivos de práctica en esta misma carpeta.

## Objetivo

Conectar tres routers primero con rutas estáticas y después con OSPF de área única; comparar distancia administrativa y costo.

## Topología

```
LAN1 ── G0/0 R1 G0/1 ─── G0/0 R2 G0/1 ─── G0/1 R3 G0/0 ── LAN3
               G0/2 ───────────────────── G0/2   (reto)
```

| Enlace / LAN | Red | Direcciones |
|---|---|---|
| LAN1 | 192.168.1.0/24 | R1 .1, PC1 .10 |
| R1–R2 | 10.0.12.0/30 | R1 .1, R2 .2 |
| R2–R3 | 10.0.23.0/30 | R2 .1, R3 .2 |
| LAN3 | 192.168.3.0/24 | R3 .1, PC3 .10 |
| R1–R3 (reto) | 10.0.13.0/30 | R1 .1, R3 .2 |

## Pasos

1. Arma la topología con 3 routers 2911, 2 switches y 2 PCs. Configura las IPs (sin el enlace del reto).
2. **Parte A – estáticas:** configura las rutas mínimas para que PC1 y PC3 se comuniquen. En R3 usa una ruta por defecto.
3. Verifica con `show ip route` y ping. Anota los códigos `S` y `S*`.
4. **Parte B – OSPF:** borra las estáticas. Configura OSPF proceso 1, área 0, router ID `1.1.1.1`, `2.2.2.2` y `3.3.3.3`. Haz pasivas las interfaces de LAN.
5. Verifica vecinos, rutas `O` y la métrica `[110/x]`.
6. Vuelve a agregar en R1 la estática hacia LAN3 y observa cuál ruta queda en la tabla. Luego quítala.
7. **Reto:** conecta el enlace R1–R3, agrégalo a OSPF y calcula qué camino elige R1 hacia LAN3. Después cambia el costo para que prefiera pasar por R2.

## Criterios de terminado

- [ ] Parte A: PC1 hace ping a PC3 solo con estáticas; R3 muestra `S* 0.0.0.0/0`.
- [ ] `show ip ospf neighbor` muestra vecinos en estado **FULL** (R2 con dos vecinos).
- [ ] `show ip route ospf` en R1 muestra `O 192.168.3.0/24 [110/3]`.
- [ ] Con la estática agregada, R1 usa la ruta `S` (AD 1) y puedo explicar por qué.
- [ ] `show ip protocols` muestra las interfaces de LAN como pasivas.
- [ ] Reto: con el enlace directo, el costo a LAN3 es 2; después de cambiar el costo, `tracert` pasa por R2.

## Solución

> [!success]- Parte A: estáticas
> ```
> ! R1
> ip route 192.168.3.0 255.255.255.0 10.0.12.2
> ! R2
> ip route 192.168.1.0 255.255.255.0 10.0.12.1
> ip route 192.168.3.0 255.255.255.0 10.0.23.2
> ! R3
> ip route 0.0.0.0 0.0.0.0 10.0.23.1
> ```
> Para borrarlas, repite cada línea con `no` delante.

> [!success]- Parte B: OSPF
> ```
> ! R1
> router ospf 1
>  router-id 1.1.1.1
>  network 192.168.1.0 0.0.0.255 area 0
>  network 10.0.12.0 0.0.0.3 area 0
>  passive-interface g0/0
> ! R2
> router ospf 1
>  router-id 2.2.2.2
>  network 10.0.12.0 0.0.0.3 area 0
>  network 10.0.23.0 0.0.0.3 area 0
> ! R3
> router ospf 1
>  router-id 3.3.3.3
>  network 192.168.3.0 0.0.0.255 area 0
>  network 10.0.23.0 0.0.0.3 area 0
>  passive-interface g0/0
> ```
> Costo de R1 a LAN3: G0/1 de R1 (1) + G0/1 de R2 (1) + G0/0 de R3 (1) = **3**.

> [!success]- Estática frente a OSPF
> La estática tiene AD **1** y OSPF **110**: para el mismo prefijo gana la estática y la ruta OSPF no se instala. Si la estática fuera `ip route 192.168.3.0 255.255.255.0 10.0.12.2 120`, quedaría como respaldo flotante.

> [!success]- Reto: enlace directo y costo
> ```
> ! R1
> interface g0/2
>  ip address 10.0.13.1 255.255.255.252
>  no shutdown
> router ospf 1
>  network 10.0.13.0 0.0.0.3 area 0
> ! R3
> interface g0/2
>  ip address 10.0.13.2 255.255.255.252
>  no shutdown
> router ospf 1
>  network 10.0.13.0 0.0.0.3 area 0
> ```
> Camino directo: 1 + 1 = **2** (< 3). Para preferir R2: en R1 `interface g0/2` → `ip ospf cost 10`.

## Notas personales

_Qué me costó, errores que cometí, dudas._
