---
proyecto: Conocimiento
tipo: laboratorio
nivel: intermedio
progreso: pendiente
estado: borrador
area: Seguridad
herramienta: PortSwigger Web Security Academy, Burp Suite Community
tags: [seguridad, laboratorio]
contenido: completo
---
# Lab 6 - PortSwigger - laboratorios de XSS nivel Apprentice

Parte de: [[12-Seguridad/03 XSS/_Índice XSS|XSS]]

**Cubre:** [[XSS]] · [[XSS - reflejado]] · [[XSS - almacenado]] · [[XSS - basado en DOM]] · [[XSS - contextos de salida (HTML, atributo, JavaScript y URL)]] · [[XSS - impacto real]] · [[XSS - cómo detectarlo]]

> [!info] Al terminar
> Si todos los criterios quedan marcados, cambia a `dominado` el `progreso` de este laboratorio y de las notas que cubre. Guarda tus archivos de práctica en esta misma carpeta.

> [!warning] Ética
> Haz esto solo en tu máquina, en tu propio código o en plataformas creadas para practicar. Probar sistemas ajenos sin autorización por escrito es ilegal. Ver [[Marco legal y autorización por escrito]].

## Objetivo
Resolver los labs Apprentice del tema **Cross-site scripting** de la Web Security Academy (XSS reflejado en contexto HTML sin codificar, XSS almacenado en contexto HTML y los de XSS basado en DOM con sinks como `document.write` e `innerHTML`) y documentar para cada uno la fuente, el sink o el contexto, y la corrección.

## Pasos
1. Lee el material del tema XSS en la Academy (reflejado, almacenado, DOM y contextos).
2. Abre cada lab Apprentice del tema. Trabaja solo dentro de la instancia del laboratorio.
3. Antes de intentar nada, identifica: ¿de dónde viene el dato (fuente)? ¿dónde termina (contexto o sink)?
4. Resuelve el lab sin ver la solución.
5. En tu bitácora anota: tipo de XSS, contexto, por qué funcionó y qué línea de código (Razor, React o JS) lo habría evitado.

## Criterios de terminado
- [ ] Todos los labs Apprentice de XSS aparecen resueltos en tu perfil.
- [ ] Para cada uno identificaste fuente y sink/contexto por escrito.
- [ ] Para los de DOM, escribiste la versión corregida con `textContent` o validación de URL.
- [ ] Explicas por qué una CSP sin `'unsafe-inline'` habría bloqueado la mayoría de ellos.

## Solución

> La solución oficial de cada lab está en la Academy. Aquí está la lectura defensiva que deberías obtener.

> [!success]- Patrón común
> - **Reflejado y almacenado en HTML**: el servidor insertaba el valor en el cuerpo HTML sin codificar. En Razor, `@valor` lo habría codificado; en React, `{valor}`.
> - **DOM**: el JavaScript leía de la URL (`location.search` o similar) y escribía con `document.write` o `innerHTML`. Con `textContent` el dato se habría mostrado como texto.
> - Una CSP con `script-src 'self'` sin `'unsafe-inline'` habría bloqueado los scripts y manejadores inline, aunque no corrige la causa.

## Notas personales

_Qué me costó, errores que cometí, dudas._
