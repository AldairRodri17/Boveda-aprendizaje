---
proyecto: Conocimiento
tipo: indice
estado: en-progreso
area: Fundamentos
tags: [indice, fundamentos]
---
# Computación general

Parte de: [[02-Fundamentos/_Índice Fundamentos|Fundamentos]] · Avance general en [[Temario]].

> [!info] Cómo avanzar
> Cada nota tiene **Autoevaluación** (preguntas con la respuesta plegada). Márcala `visto` cuando la contestes sin abrir las respuestas. Los laboratorios llevan las notas a `dominado`.

## Temas

- [[Terminal (bash y PowerShell)]]
- [[Linux para desarrolladores (SSH, permisos, procesos y systemd)]]
  - [[Linux - SSH y llaves]]
  - [[Linux - permisos de archivos]]
  - [[Linux - procesos y señales]]
  - [[Linux - servicios con systemd]]
- [[Variables de entorno]]
- [[Procesos e hilos]]
- [[Concurrencia vs paralelismo]]
- [[Síncrono vs asíncrono]]
- [[Estructuras de datos básicas]]
  - [[Arreglos y listas enlazadas]]
  - [[Pilas y colas (estructuras de datos)]]
  - [[Tablas hash]]
  - [[Árboles y árboles binarios de búsqueda]]
  - [[Grafos (estructuras de datos)]]
- [[Complejidad algorítmica (Big O)]]
- [[Algoritmos básicos (búsqueda, ordenamiento, recursión)]]
  - [[Búsqueda lineal y binaria]]
  - [[Algoritmos de ordenamiento]]
  - [[Recursión (algoritmos)]]
- [[Principios SOLID]]
  - [[SOLID - Responsabilidad única (SRP)]]
  - [[SOLID - Abierto-cerrado (OCP)]]
  - [[SOLID - Sustitución de Liskov (LSP)]]
  - [[SOLID - Segregación de interfaces (ISP)]]
  - [[SOLID - Inversión de dependencias (DIP)]]
- [[DRY, KISS y YAGNI]]
- [[Acoplamiento y cohesión]]
- [[Code smells y refactorización]]
- [[Clean code]]
- [[Markdown]]

## Laboratorios

- [[Lab 1 - Terminal, variables de entorno y procesos]]

## Progreso

```base
filters:
  and:
    - file.inFolder("02-Fundamentos/08 Computación general")
    - file.name != "_Índice Computación general"
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
