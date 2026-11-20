---
proyecto: Conocimiento
tipo: concepto
nivel: basico
progreso: pendiente
estado: pendiente
area: Redes
tags: [redes]
---
# Formato y abreviación de IPv6

Parte de: [[IPv6 - direccionamiento]]

> [!info] Cómo avanzar
> `progreso`: **pendiente** → **viendo** (estudiando) → **visto** (contestaste la autoevaluación sin abrir las respuestas) → **dominado** (hiciste el laboratorio o lo aplicaste de verdad).

## Contenido

### Qué es
Una dirección IPv6 completa tiene **32 dígitos hexadecimales** en ocho grupos de cuatro (hextetos), separados por dos puntos. Escribirla entera es largo y propenso a errores, así que existen **dos reglas de abreviación** estandarizadas (RFC 5952 define además la forma "canónica" recomendada). Saber abreviar y expandir rápido es imprescindible para configurar equipos y para el examen.

### Cómo funciona
Cada dígito hexadecimal representa 4 bits (0 a 9 y a a f); cada hexteto, 16 bits.

**Regla 1: omitir ceros a la izquierda** dentro de cada hexteto.
- `0db8` → `db8`, `0042` → `42`, `0000` → `0`.
- Solo los de la **izquierda**: `ff00` no se puede escribir `ff`.

**Regla 2: sustituir una secuencia continua de hextetos en cero por `::`**.
- Solo **una vez** por dirección. Si apareciera dos veces, no sabrías cuántos ceros hay en cada hueco: `2001::1::5` es ambigua.
- RFC 5952 recomienda: reemplazar la secuencia **más larga**; si hay dos iguales, la **primera**; no usar `::` para un solo hexteto en cero; y escribir en **minúsculas**.

**Expandir** es el proceso inverso: cuenta los hextetos que hay, y el `::` vale tantos grupos `0000` como falten para llegar a ocho; luego rellena cada hexteto con ceros a la izquierda hasta 4 dígitos.

### Ejemplo
**Abreviar** `2001:0db8:0000:0042:0000:0000:0000:0007`:
1. Ceros a la izquierda: `2001:db8:0:42:0:0:0:7`
2. La secuencia más larga de ceros son los tres hextetos centrales: `2001:db8:0:42::7`

**Abreviar** `2001:0db8:0000:0000:0001:0000:0000:0001`:
- Hay dos secuencias de dos ceros; se reemplaza la primera: `2001:db8::1:0:0:1`.

**Expandir** `2001:db8:cafe::10`:
- Hay 4 hextetos escritos → faltan 4 → `2001:0db8:cafe:0000:0000:0000:0000:0010`.

**Expandir** `fe80::1` → `fe80:0000:0000:0000:0000:0000:0000:0001`.

Comprueba con Python (útil para practicar):

```python
import ipaddress
for d in ["2001:0db8:0000:0042:0000:0000:0000:0007",
          "2001:0db8:0000:0000:0001:0000:0000:0001"]:
    print(ipaddress.ip_address(d))            # forma comprimida canónica
print(ipaddress.ip_address("2001:db8:cafe::10").exploded)
# 2001:db8:0:42::7 · 2001:db8::1:0:0:1 · 2001:0db8:cafe:0000:0000:0000:0000:0010
```

En Cisco IOS puedes escribir la dirección abreviada; IOS la muestra en mayúsculas:

```text
R1(config-if)# ipv6 address 2001:db8:cafe:1::1/64
R1# show ipv6 interface brief
GigabitEthernet0/1   [up/up]
    FE80::...
    2001:DB8:CAFE:1::1
```

**Prefijos**: igual que en CIDR, `/64` indica los bits de red. `2001:db8:cafe:1::/64` es la red; los hosts varían en los últimos 64 bits. Los límites "bonitos" son múltiplos de 4 bits (un dígito hex): /48, /56, /64 se leen fácil porque coinciden con hextetos o medios hextetos.

**En URLs** la dirección va entre corchetes para que los dos puntos no se confundan con el puerto: `https://[2001:db8:cafe:1::10]:8443/api`.

### Cuándo usarlo y cuándo no
Escribe siempre la forma abreviada canónica en documentación y configuraciones; facilita comparar y buscar. Usa la forma expandida cuando necesites calcular a mano (subredes, identificadores EUI-64) o al explicar. Evita formas "mixtas" o con mayúsculas en documentos: dificultan buscar con grep.

### Errores comunes
- **Usar `::` dos veces**.
- **Quitar ceros a la derecha** (`ff00` → `ff`): cambia el valor.
- **Reemplazar con `::` una secuencia más corta** cuando hay una más larga.
- **Olvidar los corchetes** en URLs con IPv6.

### Para profundizar
- [RFC 4291: arquitectura de direccionamiento IPv6 (sección de representación de texto)](https://www.rfc-editor.org/rfc/rfc4291)
- [RFC 5952: recomendación para la representación de direcciones IPv6](https://www.rfc-editor.org/rfc/rfc5952)
- [Documentación de Python: módulo ipaddress](https://docs.python.org/es/3/library/ipaddress.html)

## Autoevaluación

Contesta primero en voz alta o en papel; luego despliega la respuesta.

> [!question]- ¿Cuáles son las dos reglas para abreviar?
> 1) Quitar los ceros a la izquierda de cada hexteto. 2) Reemplazar **una sola** secuencia continua de hextetos en cero por `::`.

> [!question]- Abrevia 2001:0db8:0000:0000:0000:ff00:0042:8329.
> `2001:db8::ff00:42:8329`

> [!question]- Expande 2001:db8:a::1.
> `2001:0db8:000a:0000:0000:0000:0000:0001`

> [!question]- ¿Por qué `::` solo puede aparecer una vez?
> Porque si hubiera dos, no se sabría cuántos ceros representa cada uno.

## Práctica

- [[Lab 2 - IPv6 con SLAAC en Packet Tracer]]

