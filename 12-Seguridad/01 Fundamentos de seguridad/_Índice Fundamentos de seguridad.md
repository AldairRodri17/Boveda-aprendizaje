---
proyecto: Conocimiento
tipo: indice
estado: en-progreso
area: Seguridad
tags: [indice, seguridad]
---
# Fundamentos de seguridad

Parte de: [[12-Seguridad/_Índice Seguridad|Seguridad]] · Avance general en [[Temario]].

> [!info] Cómo avanzar
> Cada nota tiene **Autoevaluación** (preguntas con la respuesta plegada). Márcala `visto` cuando la contestes sin abrir las respuestas. Los laboratorios llevan las notas a `dominado`.

## Temas

- [[OWASP Top 10]]
- [[Mentalidad de seguridad y modelado de amenazas]]
- [[Tríada CIA (confidencialidad, integridad y disponibilidad)]]
- [[Superficie de ataque]]
- [[Defensa en profundidad]]
- [[Principio de mínimo privilegio]]
- [[Validación de entrada, codificación de salida y listas blancas]]
- [[Same-origin policy]]
- [[Cifrado simétrico y asimétrico]]
  - [[Cifrado simétrico (AES)]]
  - [[Cifrado asimétrico (RSA y ECC)]]
- [[Cifrado en reposo y en tránsito]]
- [[Firmas digitales]]
- [[Manejo de secretos]]
- [[Logs y datos sensibles]]
- [[Ciclo de desarrollo seguro (SSDLC)]]
- [[Seguridad de la cadena de suministro (SBOM y firmas)]]
- [[Privacidad y protección de datos (ley mexicana y GDPR)]]
  - [[Privacidad - ley mexicana de datos personales (LFPDPPP)]]
  - [[Privacidad - GDPR para desarrolladores]]

## Laboratorios

- [[Lab 1 - Sacar secretos del código en una API .NET]]
- [[Lab 2 - Modelado de amenazas STRIDE de una app propia]]

## Progreso

```base
filters:
  and:
    - file.inFolder("12-Seguridad/01 Fundamentos de seguridad")
    - file.name != "_Índice Fundamentos de seguridad"
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
