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
# IPv4 - estructura y clases

Parte de: [[_Índice Direccionamiento IP]]

> [!info] Cómo avanzar
> `progreso`: **pendiente** → **viendo** (estudiando) → **visto** (contestaste la autoevaluación sin abrir las respuestas) → **dominado** (hiciste el laboratorio o lo aplicaste de verdad).

## Contenido

### Qué es
Una dirección **IPv4** es un número de **32 bits** que identifica una interfaz en una red IP. Se escribe en **notación decimal con puntos**: cuatro octetos de 8 bits, cada uno entre 0 y 255 (por ejemplo, `192.168.1.10`). Toda dirección tiene una **parte de red** y una **parte de host**; la máscara de subred indica dónde termina una y empieza la otra. Las **clases** (A, B, C, D, E) fueron el primer esquema para dividir ese espacio; hoy están obsoletas para el enrutamiento, pero se siguen usando como vocabulario.

### Cómo funciona
**Binario**: cada octeto representa 8 bits con valores posicionales 128, 64, 32, 16, 8, 4, 2, 1. Para convertir a binario restas de izquierda a derecha los valores que caben. Ejemplo con `10.20.30.77`:
- 10 = 8 + 2 → `00001010`
- 20 = 16 + 4 → `00010100`
- 30 = 16 + 8 + 4 + 2 → `00011110`
- 77 = 64 + 8 + 4 + 1 → `01001101`

Resultado: `00001010.00010100.00011110.01001101`. Dominar esta conversión es imprescindible para el [[Subnetting paso a paso]].

**Clases** (según el primer octeto, es decir, según los primeros bits):

| Clase | Primeros bits | Primer octeto | Máscara por defecto | Uso |
|---|---|---|---|---|
| A | 0 | 1 – 126 | /8 (255.0.0.0) | Redes muy grandes |
| B | 10 | 128 – 191 | /16 (255.255.0.0) | Redes medianas |
| C | 110 | 192 – 223 | /24 (255.255.255.0) | Redes pequeñas |
| D | 1110 | 224 – 239 | No aplica | Multicast |
| E | 1111 | 240 – 255 | No aplica | Experimental/reservada |

El 0 y el 127 quedan fuera de la clase A útil: `0.0.0.0/8` significa "esta red" y `127.0.0.0/8` es loopback (ver [[Loopback y APIPA]]).

**Por qué ya casi no se habla de clases**: el esquema era muy rígido. Una empresa con 2,000 equipos no cabía en una C (254 hosts) y recibía una B (65,534 hosts), desperdiciando el 97% del bloque. En 1993 se adoptó **CIDR** (Classless Inter-Domain Routing), que permite máscaras de cualquier longitud (/21, /27…) y agregar rutas. Los routers modernos ignoran la clase y usan el prefijo (ver [[Máscara de subred y CIDR]]). Las clases sobreviven en algunos comportamientos (por ejemplo, la sumarización automática de protocolos antiguos como RIPv1) y en preguntas de examen.

Además del esquema de clases, hay bloques especiales: privados ([[IP privada (RFC 1918)]]), CGNAT `100.64.0.0/10`, link-local `169.254.0.0/16`, documentación `192.0.2.0/24`, `198.51.100.0/24` y `203.0.113.0/24` (por eso los ejemplos de esta bóveda usan esas redes como "públicas").

### Ejemplo
Practica la conversión con Python (útil para verificar tus ejercicios en papel):

```python
import ipaddress

ip = ipaddress.ip_address("172.20.100.37")
print(".".join(f"{int(o):08b}" for o in str(ip).split(".")))
# 10101100.00010100.01100100.00100101
print(int(ip))               # la dirección como entero de 32 bits
print(ip.is_private)         # True: 172.16.0.0/12 es privado
```

Y en un router Cisco verás la IP y máscara de cada interfaz:

```text
R1# show ip interface g0/0 | include Internet address
  Internet address is 192.168.1.1/24
```

### Cuándo usarlo y cuándo no
Las clases sirven para entender la historia, leer documentación vieja y contestar el examen. Para diseñar redes, calcular subredes o leer tablas de rutas usa **siempre el prefijo** (`/n`). Si alguien te dice "es una red clase C", traduce mentalmente a "/24" y pregunta por la máscara real.

### Errores comunes
- **Deducir la máscara a partir de la clase**: `10.1.1.0` puede ser /24 aunque sea "clase A".
- **Olvidar que 127 no es clase A útil**.
- **Errores al convertir**: verifica siempre que la suma de los bits dé el octeto.
- **Confundir la clase D con una red normal**: es multicast, no se asigna a hosts.

### Para profundizar
- [RFC 791: Internet Protocol](https://www.rfc-editor.org/rfc/rfc791)
- [RFC 4632: CIDR](https://www.rfc-editor.org/rfc/rfc4632)
- [RFC 6890: registros de direcciones de propósito especial](https://www.rfc-editor.org/rfc/rfc6890)

## Autoevaluación

Contesta primero en voz alta o en papel; luego despliega la respuesta.

> [!question]- ¿Cuántos bits tiene una dirección IPv4 y cómo se escribe?
> 32 bits, escritos como 4 octetos en decimal (0 a 255) separados por puntos. Ejemplo: `192.168.1.10`.

> [!question]- Convierte 192.168.1.10 a binario.
> `11000000.10101000.00000001.00001010`

> [!question]- ¿Qué rango del primer octeto corresponde a cada clase?
> A: 1–126 (127 se reserva para loopback) · B: 128–191 · C: 192–223 · D: 224–239 (multicast) · E: 240–255 (experimental).

> [!question]- ¿Por qué ya casi no se habla de clases?
> Desde 1993 se usa **CIDR**: la máscara define dónde termina la red, sin importar la clase. Las clases desperdiciaban direcciones (una empresa con 300 hosts recibía una clase B de 65,534).

## Práctica

- [[Lab 1 - Subredes IPv4 en Packet Tracer]]

