---
proyecto: Conocimiento
tipo: indice
estado: en-progreso
area: Seguridad
tags: [indice, seguridad]
---
# Inyección SQL

Parte de: [[12-Seguridad/_Índice Seguridad|Seguridad]] · Avance general en [[Temario]].

> [!info] Cómo avanzar
> Cada nota tiene **Autoevaluación** (preguntas con la respuesta plegada). Márcala `visto` cuando la contestes sin abrir las respuestas. Los laboratorios llevan las notas a `dominado`.

## Temas

- [[SQL injection]]
- [[SQL injection - qué es y por qué ocurre]]
- [[SQL injection - concatenación de cadenas, la causa raíz]]
- [[SQL injection - cómo funciona un ataque (conceptual)]]
- [[SQL injection - tipos (basada en errores, UNION, ciega y de segundo orden)]]
- [[SQL injection - impacto real]]
- [[SQL injection - consultas parametrizadas]]
- [[SQL injection - Dapper y parámetros]]
- [[SQL injection - EF Core (FromSqlRaw vs FromSqlInterpolated)]]
- [[SQL injection - stored procedures y SQL dinámico seguro (sp_executesql y QUOTENAME)]]
- [[SQL injection - listas blancas para ORDER BY y nombres de columnas]]
- [[SQL injection - mínimo privilegio en la cuenta de base de datos]]
- [[SQL injection - manejo de errores sin filtrar detalles]]
- [[SQL injection - cómo detectarla (revisión de código, SAST y DAST)]]
- [[SQL injection - WAF como capa adicional]]
- [[Inyección NoSQL]]
- [[Inyección de comandos del sistema operativo]]
- [[Otras inyecciones (LDAP y XPath)]]

## Laboratorios

- [[Lab 3 - Consulta vulnerable y su corrección con Dapper]]
- [[Lab 4 - PortSwigger - laboratorios de SQL injection nivel Apprentice]]

## Progreso

```base
filters:
  and:
    - file.inFolder("12-Seguridad/02 Inyección SQL")
    - file.name != "_Índice Inyección SQL"
views:
  - type: table
    name: Temas
    filters:
      and:
        - tipo != "laboratorio"
    order:
      - file.name
      - progreso
      - nivel
  - type: table
    name: Laboratorios
    filters:
      and:
        - tipo == "laboratorio"
    order:
      - file.name
      - progreso
```
