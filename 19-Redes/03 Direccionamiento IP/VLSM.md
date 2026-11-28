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
# VLSM

Parte de: [[_Índice Direccionamiento IP]]

> [!info] Cómo avanzar
> `progreso`: **pendiente** → **viendo** (estudiando) → **visto** (contestaste la autoevaluación sin abrir las respuestas) → **dominado** (hiciste el laboratorio o lo aplicaste de verdad).

## Contenido

### Qué es
**VLSM** (Variable Length Subnet Masking) es dividir un bloque de direcciones en subredes de **distinto tamaño**, cada una con la máscara que necesita. Con subnetting de máscara fija todas las subredes miden lo mismo y se desperdician direcciones: un enlace entre dos routers no necesita una /24. VLSM es posible gracias a CIDR y a protocolos de enrutamiento sin clase (OSPF, EIGRP, BGP, RIPv2), que anuncian la máscara junto con cada red.

### Cómo funciona
Procedimiento:
1. **Lista los requisitos** de hosts por red, incluido el crecimiento.
2. **Ordénalos de mayor a menor**.
3. Para cada uno calcula el prefijo mínimo: el menor h con 2ʰ − 2 ≥ hosts.
4. **Asigna empezando por la más grande** al inicio del bloque libre; la siguiente empieza justo después del broadcast de la anterior.
5. Los enlaces punto a punto van al final con **/30** (2 hosts) o **/31** si los equipos lo soportan.
6. Documenta la tabla y deja el espacio sobrante contiguo para crecer.

**¿Por qué primero las grandes?** Porque una subred debe empezar en un múltiplo de su tamaño. Si colocas primero las pequeñas, dejas huecos que impiden alinear una subred grande y terminas desperdiciando o sin espacio. Al asignar de mayor a menor, cada bloque queda alineado automáticamente.

### Ejemplo
Bloque `10.10.0.0/22` (1,024 direcciones) para una oficina:
- Usuarios: 300 hosts
- Desarrollo: 120 hosts
- Servidores: 60 hosts
- 2 enlaces punto a punto entre routers

| Red | Hosts pedidos | Prefijo | Subred | Broadcast | Hosts útiles |
|---|---|---|---|---|---|
| Usuarios | 300 | /23 | 10.10.0.0/23 | 10.10.1.255 | 510 |
| Desarrollo | 120 | /25 | 10.10.2.0/25 | 10.10.2.127 | 126 |
| Servidores | 60 | /26 | 10.10.2.128/26 | 10.10.2.191 | 62 |
| Enlace R1–R2 | 2 | /30 | 10.10.2.192/30 | 10.10.2.195 | 2 |
| Enlace R2–R3 | 2 | /30 | 10.10.2.196/30 | 10.10.2.199 | 2 |

Queda libre desde `10.10.2.200` hasta `10.10.3.255` para crecer. Con máscara fija de /23 para todo, el bloque /22 solo alcanzaría para dos redes.

Script para verificar (y para generar tus propios ejercicios):

```python
import ipaddress, math

def vlsm(bloque, requisitos):
    actual = int(ipaddress.ip_network(bloque).network_address)
    for nombre, hosts in sorted(requisitos, key=lambda r: -r[1]):
        prefijo = 32 - math.ceil(math.log2(hosts + 2))
        red = ipaddress.ip_network((actual, prefijo))
        actual += red.num_addresses
        print(f"{nombre:12} {red}  bc {red.broadcast_address}  útiles {red.num_addresses - 2}")

vlsm("10.10.0.0/22", [("Usuarios", 300), ("Desarrollo", 120), ("Servidores", 60),
                      ("R1-R2", 2), ("R2-R3", 2)])
```

En el router, cada interfaz recibe su máscara particular:

```text
R1(config)# interface g0/0
R1(config-if)# ip address 10.10.0.1 255.255.254.0
R1(config)# interface g0/1
R1(config-if)# ip address 10.10.2.193 255.255.255.252
```

### Cuándo usarlo y cuándo no
Úsalo siempre que tus redes tengan tamaños muy distintos y el espacio sea limitado: direcciones públicas, VNets de Azure que deben convivir con redes on-premises sin traslaparse, o bloques que te asignó el área de seguridad. En redes privadas con espacio de sobra (todo 10.0.0.0/8), algunos prefieren máscara fija /24 por simplicidad operativa; es válido mientras no haya traslapes.

### Errores comunes
- **Asignar primero las pequeñas** y desalinear las grandes.
- **Traslapar subredes**: la siguiente empieza después del broadcast, no en el siguiente múltiplo "cualquiera".
- **No dejar margen de crecimiento**: ajustar al número exacto de hoy obliga a renumerar mañana.
- **Usar un protocolo con clase** (RIPv1) que no anuncia máscaras.

### Para profundizar
- [RFC 1878: tabla de subredes de longitud variable](https://www.rfc-editor.org/rfc/rfc1878)
- [RFC 3021: prefijos /31 en enlaces punto a punto](https://www.rfc-editor.org/rfc/rfc3021)
- [RFC 4632: CIDR](https://www.rfc-editor.org/rfc/rfc4632)

## Ejercicio de diseño

> [!question]- Con 192.168.20.0/24 diseña: Ventas 100 hosts, TI 50, RRHH 25 y 2 enlaces punto a punto entre routers.
> Se ordena de mayor a menor y se asigna en orden:
>
> | Red | Prefijo | Red | Hosts | Broadcast |
> |---|---|---|---|---|
> | Ventas | /25 | 192.168.20.0 | .1 – .126 | .127 |
> | TI | /26 | 192.168.20.128 | .129 – .190 | .191 |
> | RRHH | /27 | 192.168.20.192 | .193 – .222 | .223 |
> | Enlace 1 | /30 | 192.168.20.224 | .225 – .226 | .227 |
> | Enlace 2 | /30 | 192.168.20.228 | .229 – .230 | .231 |
>
> Queda libre desde `192.168.20.232`.

## Autoevaluación

Contesta primero en voz alta o en papel; luego despliega la respuesta.

> [!question]- ¿Qué diferencia hay entre VLSM y subnetting de máscara fija?
> Con máscara fija todas las subredes tienen el mismo tamaño; con **VLSM** cada subred usa el prefijo que necesita, y se desperdician menos direcciones.

> [!question]- ¿Por qué se asignan primero las subredes más grandes?
> Para que queden alineadas a su tamaño de bloque y no se encimen; si empiezas por las chicas, las grandes ya no caben en un límite válido.

> [!question]- ¿Qué prefijo usas en un enlace punto a punto entre dos routers?
> /30 (2 hosts). También /31 (RFC 3021), si el equipo lo soporta.

## Práctica

- [[Lab 1 - Subredes IPv4 en Packet Tracer]]

