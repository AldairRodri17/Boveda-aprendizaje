---
proyecto: Conocimiento
tipo: laboratorio
nivel: basico
progreso: pendiente
estado: borrador
area: Fundamentos
herramienta: Python 3 (local o Google Colab) con re y zoneinfo
tags: [fundamentos, laboratorio]
contenido: completo
---
# Lab 1 - Expresiones regulares y fechas en Python

Parte de: [[02-Fundamentos/06 Datos y formatos/_Índice Datos y formatos|Datos y formatos]]

**Cubre:** [[Expresiones regulares]] · [[Fechas, zonas horarias y UTC]] · [[ISO 8601]]

> [!info] Al terminar
> Si todos los criterios quedan marcados, cambia a `dominado` el `progreso` de este laboratorio y de las notas que cubre. Guarda tus archivos de práctica en esta misma carpeta.

## Objetivo
Extraer y validar datos con expresiones regulares y convertir fechas entre UTC y la hora del centro de México sin errores de zona horaria.

## Requisitos
Python 3.9+ (trae `zoneinfo`). En Windows instala además `pip install tzdata` para tener la base de zonas. Google Colab sirve sin instalar nada.

## Datos
```python
log = '''2026-11-09T14:02:11Z INFO pedido=P-1001 cliente=ana@ejemplo.mx total=1250.50
2026-11-09T14:05:43Z ERROR pedido=P-1002 cliente=luis@ejemplo.com total=89.00 cp=4410
2026-11-10T05:59:59Z INFO pedido=P-1003 cliente=MARIA@EJEMPLO.MX total=300 cp=06700
2026-11-10T06:00:00Z WARN pedido=P-1004 cliente=pepe@@malo total=15.5 cp=64000'''
```

## Pasos
1. Con una sola regex con grupos con nombre, extrae de cada línea: fecha, nivel, pedido, cliente y total.
2. Valida los correos con una regex sencilla (sin pretender cubrir todo el RFC) e indica cuáles no pasan.
3. Valida los códigos postales: deben ser exactamente 5 dígitos. Muestra por qué `\d{5}` sin anclas no basta.
4. Normaliza los correos a minúsculas y los totales a `Decimal` con 2 decimales.
5. Convierte cada fecha UTC a `America/Mexico_City` e imprímela en ISO 8601 con desfase.
6. Agrupa los pedidos por **día local de CDMX**. ¿En qué día cae el pedido `P-1003`? ¿Y `P-1004`?
7. Calcula el rango UTC `[inicio, fin)` del día local 2026-11-09 y filtra con él.
8. Convierte `2022-07-01T12:00:00Z` y `2023-07-01T12:00:00Z` a hora de CDMX y explica la diferencia.

## Criterios de terminado
- [ ] Extraes los 5 campos de las 4 líneas con una sola regex.
- [ ] `pepe@@malo` es el único correo inválido.
- [ ] El CP `4410` se rechaza y `06700` se acepta (conservando el cero inicial como texto).
- [ ] `P-1003` cae el **9 de noviembre** local y `P-1004` el **10**.
- [ ] El rango del paso 7 es `2026-11-09T06:00:00Z` a `2026-11-10T06:00:00Z`.
- [ ] El paso 8 muestra `07:00-05:00` en 2022 y `06:00-06:00` en 2023 y explicas por qué.

## Solución

> [!success]- Pasos 1–4
> ```python
> import re
> from decimal import Decimal
> patron = re.compile(r'^(?P<fecha>\S+) (?P<nivel>[A-Z]+) pedido=(?P<pedido>P-\d+) '
>                     r'cliente=(?P<cliente>\S+) total=(?P<total>[\d.]+)')
> correo = re.compile(r'^[\w.+-]+@[\w-]+(\.[\w-]+)+$')
> cp = re.compile(r'^\d{5}$')
> for linea in log.splitlines():
>     m = patron.match(linea).groupdict()
>     m['cliente'] = m['cliente'].lower()
>     m['total'] = Decimal(m['total']).quantize(Decimal('0.01'))
>     c = re.search(r'cp=(\S+)', linea)
>     print(m, 'correo ok' if correo.match(m['cliente']) else 'correo MAL',
>           c and (c.group(1), bool(cp.match(c.group(1)))))
> ```
> `re.search(r'\d{5}', '644100')` encuentra coincidencia aunque tenga 6 dígitos; con `^...$` (o `re.fullmatch`) se exige el valor completo.

> [!success]- Pasos 5–7
> ```python
> from datetime import datetime, timedelta, timezone
> from zoneinfo import ZoneInfo
> mx = ZoneInfo('America/Mexico_City')
> for linea in log.splitlines():
>     utc = datetime.fromisoformat(linea.split()[0].replace('Z', '+00:00'))
>     local = utc.astimezone(mx)
>     print(local.isoformat(), local.date())
> inicio = datetime(2026, 11, 9, tzinfo=mx).astimezone(timezone.utc)
> fin = inicio + timedelta(days=1)
> print(inicio.isoformat(), fin.isoformat())   # 2026-11-09T06:00:00+00:00  2026-11-10T06:00:00+00:00
> ```
> `P-1003` (05:59:59Z) es 23:59:59 del día 9 en CDMX; `P-1004` (06:00:00Z) es 00:00 del día 10. Agrupar por la fecha UTC los pondría en días equivocados.

> [!success]- Paso 8: horario de verano
> En julio de 2022 México todavía aplicaba horario de verano (UTC−5): `2022-07-01T07:00:00-05:00`. Desde finales de octubre de 2022 se eliminó en casi todo el país, así que en 2023 es UTC−6: `2023-07-01T06:00:00-06:00`. Por eso nunca se resta un desfase fijo: la base de zonas conoce el historial.

## Notas personales

_Qué me costó, errores que cometí, dudas._
