---
proyecto: Conocimiento
tipo: indice
estado: en-progreso
area: Laboratorios y práctica
tags: [indice, laboratorios-practica]
---
# Laboratorios de seguridad

Parte de: [[16-Laboratorios-Practica/_Índice Laboratorios y práctica|Laboratorios y práctica]] · Avance general en [[Temario]].

> [!info] Cómo avanzar
> Cada nota tiene **Autoevaluación** (preguntas con la respuesta plegada). Márcala `visto` cuando la contestes sin abrir las respuestas. Los laboratorios llevan las notas a `dominado`.

## Temas

- [[PortSwigger Web Security Academy]]
- [[PortSwigger - ruta recomendada (Apprentice, Practitioner y Expert)]]
- [[PortSwigger - laboratorios de SQL injection]]
- [[PortSwigger - laboratorios de XSS]]
- [[OWASP Juice Shop]]
- [[DVWA]]
- [[OWASP WebGoat]]
- [[Hacker101]]
- [[TryHackMe]]
- [[Hack The Box]]
- [[PentesterLab]]
- [[picoCTF y retos CTF]]
- [[OverTheWire]]
- [[Cómo montar un laboratorio propio con Docker y red aislada]]
- [[Cómo llevar una bitácora de laboratorios resueltos]]

## Laboratorios

- [[Lab 1 - Laboratorio propio con Docker Compose y red aislada]]

## Progreso

```base
filters:
  and:
    - file.inFolder("16-Laboratorios-Practica/01 Laboratorios de seguridad")
    - file.name != "_Índice Laboratorios de seguridad"
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
