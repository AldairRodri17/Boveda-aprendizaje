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
# Lab 4 - PortSwigger - laboratorios de SQL injection nivel Apprentice

Parte de: [[12-Seguridad/02 Inyección SQL/_Índice Inyección SQL|Inyección SQL]]

**Cubre:** [[SQL injection]] · [[SQL injection - cómo funciona un ataque (conceptual)]] · [[SQL injection - tipos (basada en errores, UNION, ciega y de segundo orden)]] · [[SQL injection - impacto real]] · [[SQL injection - cómo detectarla (revisión de código, SAST y DAST)]]

> [!info] Al terminar
> Si todos los criterios quedan marcados, cambia a `dominado` el `progreso` de este laboratorio y de las notas que cubre. Guarda tus archivos de práctica en esta misma carpeta.

> [!warning] Ética
> Haz esto solo en tu máquina, en tu propio código o en plataformas creadas para practicar. Probar sistemas ajenos sin autorización por escrito es ilegal. Ver [[Marco legal y autorización por escrito]].

## Objetivo
Resolver los laboratorios de nivel Apprentice del tema **SQL injection** de la Web Security Academy (los que tratan de recuperar datos ocultos con la cláusula WHERE y de saltarse el inicio de sesión) y, para cada uno, escribir con tus palabras la causa raíz y la corrección en C#.

## Pasos
1. Crea tu cuenta gratuita en la Academy y lee el material del tema SQL injection antes de abrir los labs.
2. Instala Burp Suite Community y configura el navegador integrado de Burp.
3. Abre cada laboratorio Apprentice del tema. Cada uno levanta una instancia temporal hecha para ser atacada; no pruebes nada fuera de esa instancia.
4. Intenta resolverlo sin ver la solución oficial. Usa Repeater para observar cómo cambia la respuesta.
5. Cuando lo resuelvas, escribe en tus notas: qué parámetro era vulnerable, qué tipo de inyección era, cómo lo notaste y cómo se vería la consulta corregida con parámetros.
6. Opcional: continúa con los de nivel Practitioner sobre ataques UNION y ciegos.

## Criterios de terminado
- [ ] Todos los labs Apprentice de SQL injection aparecen como resueltos en tu perfil.
- [ ] Para cada lab tienes una entrada en tu bitácora (ver [[Cómo llevar una bitácora de laboratorios resueltos]]) con causa raíz y corrección.
- [ ] Puedes explicar la diferencia entre la inyección que cambió el filtro y la que se saltó el login.
- [ ] Escribiste el código C# con Dapper que habría evitado cada caso.

## Solución

> La solución paso a paso está en la propia Academy (botón de solución de cada lab). Úsala solo si te atoras más de 30 minutos; aquí va lo que deberías haber aprendido.

> [!success]- Lo que deberías concluir
> - En ambos casos el servidor concatenaba la entrada en el texto de la consulta: la comilla del usuario cerraba el literal y lo que seguía se interpretaba como SQL.
> - En el de datos ocultos, la condición que limitaba los resultados se anulaba; en el de login, la verificación de contraseña dejaba de evaluarse.
> - La corrección en ambos es la misma: parámetros (`@categoria`, `@usuario`) y, en el login, comparar el hash de la contraseña en código, no en una condición SQL armada con texto.

> [!success]- Cómo lo habría detectado una herramienta
> - SAST: un flujo de datos de `Request.Query` a un `string` que llega a `QueryAsync` sin parámetros.
> - DAST: diferencias en la respuesta (número de productos, error 500) al enviar caracteres especiales.

## Notas personales

_Qué me costó, errores que cometí, dudas._
