---
proyecto: Conocimiento
tipo: indice
estado: en-progreso
area: Java
tags: [indice, java]
contenido: completo
---
# Java

Avance general en [[Temario]] · Mapa mental: [[Mapa mental - Java.canvas|Mapa mental]].

> [!info] Cómo avanzar
> Cada nota tiene **Autoevaluación** (preguntas con la respuesta plegada). Márcala `visto` cuando la contestes sin abrir las respuestas. Los laboratorios llevan las notas a `dominado`.

## Temas

- [[Java - JVM, JDK y JRE]]
- [[Java - sintaxis básica]]
- [[Java - clases, interfaces y records]]
- [[Java - colecciones y streams]]
  - [[Colecciones en Java (List, Set y Map)]]
  - [[Streams en Java (map, filter y collect)]]
- [[Java - Maven y Gradle]]
- [[Spring Boot]]
  - [[Spring Boot - inyección de dependencias y beans]]
  - [[Spring Boot - controladores REST]]
  - [[Spring Boot - configuración y perfiles]]
- [[JPA e Hibernate]]
- [[Java vs CSharp (comparativa)]]

## Laboratorios

- [[Lab 1 - Primer programa Java con javac, jar y jshell]]
- [[Lab 2 - API REST con Spring Boot, JPA y H2]]
- [[Lab 3 - Colecciones y streams de Java comparados con LINQ]]

## Progreso

```base
filters:
  and:
    - file.inFolder("15-Java")
    - file.name != "_Índice Java"
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

## Progreso de toda el área

```base
filters:
  and:
    - file.inFolder("15-Java")
    - tipo != "indice"
views:
  - type: table
    name: Pendientes
    filters:
      and:
        - progreso != "visto"
        - progreso != "dominado"
    order:
      - file.name
      - file.folder
      - progreso
      - nivel
  - type: table
    name: Vistos
    filters:
      and:
        - progreso == "visto"
    order:
      - file.name
      - file.folder
  - type: table
    name: Dominados
    filters:
      and:
        - progreso == "dominado"
    order:
      - file.name
      - file.folder
```
