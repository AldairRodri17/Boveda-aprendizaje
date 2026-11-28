---
proyecto: Conocimiento
tipo: indice
estado: en-progreso
area: Seguridad
tags: [indice, seguridad]
contenido: completo
---
# Otras vulnerabilidades web

Parte de: [[12-Seguridad/_Índice Seguridad|Seguridad]] · Avance general en [[Temario]].

> [!info] Cómo avanzar
> Cada nota tiene **Autoevaluación** (preguntas con la respuesta plegada). Márcala `visto` cuando la contestes sin abrir las respuestas. Los laboratorios llevan las notas a `dominado`.

## Temas

- [[CSRF]]
- [[CORS]]
- [[Control de acceso roto e IDOR]]
- [[Autenticación rota]]
- [[Ataques a JWT y cómo evitarlos]]
- [[SSRF]]
- [[Path traversal]]
- [[Subida de archivos insegura]]
- [[Deserialización insegura]]
- [[XXE]]
- [[Clickjacking]]
- [[postMessage seguro (validar origen)]]
- [[Open redirect]]
- [[Exposición de datos sensibles]]
- [[Componentes y dependencias vulnerables]]
- [[Configuración insegura]]
- [[Secretos en repositorios]]
- [[Condiciones de carrera]]
- [[Seguridad de APIs]]

## Laboratorios

- [[Lab 7 - Revisión de un JWT mal validado en tu propio código]]
- [[Lab 8 - Control de acceso roto en OWASP Juice Shop local]]

## Progreso

```base
filters:
  and:
    - file.inFolder("12-Seguridad/04 Otras vulnerabilidades web")
    - file.name != "_Índice Otras vulnerabilidades web"
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
