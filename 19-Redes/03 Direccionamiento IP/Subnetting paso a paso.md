---
proyecto: Conocimiento
tipo: concepto
nivel: basico
progreso: pendiente
estado: pendiente
area: Redes
tags: [redes]
---
# Subnetting paso a paso

Parte de: [[_Índice Direccionamiento IP]]

> [!info] Cómo avanzar
> `progreso`: **pendiente** → **viendo** (estudiando) → **visto** (contestaste la autoevaluación sin abrir las respuestas) → **dominado** (hiciste el laboratorio o lo aplicaste de verdad).

## Contenido

### Qué es
**Subnetting** es dividir una red en redes más pequeñas (subredes) tomando bits de la parte de host y pasándolos a la parte de red. Sirve para ajustar el tamaño de cada red al número real de equipos, separar departamentos o VLAN, reducir dominios de broadcast y aprovechar mejor el espacio de direcciones. Es la habilidad de cálculo más importante del CCNA: tienes que hacerlo rápido y sin calculadora.

### Cómo funciona
**Método del número mágico** (el más rápido a mano):
1. Identifica el **octeto interesante**: el primero donde la máscara no es 255.
2. **Número mágico** = 256 − valor de la máscara en ese octeto. Es el tamaño del bloque.
3. Las subredes empiezan en **múltiplos** del número mágico en ese octeto.
4. La **red** de una IP es el múltiplo menor o igual al valor del octeto; el **broadcast** es la siguiente red menos uno; los hosts van de red + 1 a broadcast − 1.

**Fórmulas**:
- Subredes al tomar n bits: **2ⁿ**.
- Hosts por subred con h bits de host: **2ʰ − 2**.
- Para necesitar S subredes: el menor n con 2ⁿ ≥ S.
- Para necesitar H hosts: el menor h con 2ʰ − 2 ≥ H.

Si te dan una IP y una máscara, lo primero es la operación AND entre ambas (o, en la práctica, ubicar el múltiplo del número mágico).

### Ejemplo
**Caso 1: ¿a qué red pertenece 172.20.100.37/21?**
- /21 → máscara `255.255.248.0`; octeto interesante: el tercero (248).
- Número mágico: 256 − 248 = **8**. Múltiplos de 8: …, 88, 96, 104…
- 100 cae entre 96 y 103 → red **172.20.96.0**, broadcast **172.20.103.255**.
- Hosts: `172.20.96.1` a `172.20.103.254` → 2¹¹ − 2 = **2,046**.

**Caso 2: divide 192.168.50.0/24 en 8 subredes iguales.**
- 2³ = 8 → tomas 3 bits → **/27**, número mágico 32, 30 hosts cada una.
- Subredes: `.0`, `.32`, `.64`, `.96`, `.128`, `.160`, `.192`, `.224` (todas /27).
- La tercera, `192.168.50.64/27`, tiene hosts `.65` a `.94` y broadcast `.95`.

**Caso 3: ¿cuántos hosts necesitas y qué prefijo?** Para 60 hosts: 2⁶ − 2 = 62 ≥ 60 → 6 bits de host → **/26**.

Verifica siempre con Python mientras practicas:

```python
import ipaddress

print(ipaddress.ip_interface("172.20.100.37/21").network)
# 172.20.96.0/21
for s in ipaddress.ip_network("192.168.50.0/24").subnets(new_prefix=27):
    hosts = list(s.hosts())
    print(s, hosts[0], hosts[-1], s.broadcast_address)
```

Y en el router, IOS rechazará una IP que sea red o broadcast de su máscara:

```text
R1(config-if)# ip address 192.168.50.95 255.255.255.224
Bad mask /27 for address 192.168.50.95
```

Practica a mano con el [[Lab 1 - Subredes IPv4 en Packet Tracer]], donde asignas las subredes calculadas a interfaces reales y compruebas con `ping`.

### Cuándo usarlo y cuándo no
Úsalo cada vez que diseñes direccionamiento: VLAN de una oficina, enlaces entre routers, subredes de una VNet de Azure o redes de Docker. Si todas las subredes son del mismo tamaño, basta subnetting de máscara fija. Si los tamaños varían mucho (una VLAN de 200 usuarios y enlaces de 2 hosts), usa [[VLSM]] para no desperdiciar direcciones.

### Errores comunes
- **Tomar el múltiplo mayor** en lugar del menor o igual al calcular la red.
- **Olvidar que el octeto interesante puede ser el segundo o el tercero**: en /21 los saltos son en el tercer octeto.
- **Asignar la dirección de red o broadcast a un host**: en prefijos menores a /24, una dirección terminada en .0 o .255 puede ser válida o no; calcula siempre.
- **Contar mal los bits**: 2ⁿ subredes, 2ʰ − 2 hosts; no al revés.

### Para profundizar
- [RFC 950: procedimiento de subnetting](https://www.rfc-editor.org/rfc/rfc950)
- [RFC 4632: CIDR](https://www.rfc-editor.org/rfc/rfc4632)
- [Documentación de Python: módulo ipaddress](https://docs.python.org/es/3/library/ipaddress.html)

## Ejercicios

Resuélvelos en papel; la solución está plegada.

> [!question]- 1. Divide 192.168.1.0/24 en 4 subredes iguales.
> Se toman 2 bits → **/26** (62 hosts cada una):
> `192.168.1.0/26` · `192.168.1.64/26` · `192.168.1.128/26` · `192.168.1.192/26`

> [!question]- 2. Red, broadcast y rango de hosts de 172.16.45.200/20.
> Red `172.16.32.0` · Broadcast `172.16.47.255` · Hosts `172.16.32.1` – `172.16.47.254` (**4,094 hosts**).
> Truco: en /20 el tercer octeto salta de 16 en 16 → 45 cae en el bloque 32–47.

> [!question]- 3. ¿En qué subred está 192.168.5.130/27?
> /27 salta de 32 en 32 → `192.168.5.128/27`, broadcast `192.168.5.159`, hosts `.129` – `.158`.

> [!question]- 4. Tienes 10.0.0.0/8 y necesitas 500 subredes. ¿Qué prefijo usas y cuántos hosts quedan por subred?
> 2⁹ = 512 ≥ 500 → se toman 9 bits → **/17**. Hosts por subred: 2¹⁵ − 2 = **32,766**.

> [!question]- 5. ¿Qué prefijo mínimo necesitas para 300 hosts?
> 2⁸ − 2 = 254 no alcanza; 2⁹ − 2 = 510 sí → **/23**.

> [!question]- 6. ¿Puedes asignar 10.1.1.255/23 a un equipo?
> **No.** La red es `10.1.0.0/23` y su broadcast es justo `10.1.1.255`. (En una /23, `10.1.0.255` sí es un host válido.)

## Autoevaluación

Contesta primero en voz alta o en papel; luego despliega la respuesta.

> [!question]- ¿Cuál es el "número mágico" y para qué sirve?
> 256 menos el valor del octeto interesante de la máscara (ej. /26 → 256 − 192 = 64). Es el tamaño del bloque: las subredes empiezan en múltiplos de ese número.

> [!question]- ¿Cómo calculas cuántas subredes obtienes al tomar n bits?
> 2ⁿ subredes.

> [!question]- ¿Qué haces primero si te dan una IP y una máscara y te piden la red?
> AND binario entre la IP y la máscara (o, rápido: ubicar el múltiplo del número mágico inmediatamente menor o igual).

## Práctica

- [[Lab 1 - Subredes IPv4 en Packet Tracer]]

