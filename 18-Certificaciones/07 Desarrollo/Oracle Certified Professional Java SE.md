---
proyecto: Conocimiento
tipo: certificacion
proveedor: Oracle
codigo: "1Z0-830 (Java 21) / 1Z0-831 (Java 25)"
estado_cert: pendiente
fecha_examen:
fecha_obtenida:
vence:
credencial_id:
enlace_verificacion:
costo_usd: 
vigencia: "Pendiente de confirmar"
verificado_el: 2026-11-09
area: Java
tags: [certificacion, certificaciones]
---
# Oracle Certified Professional Java SE

Parte de: [[18-Certificaciones/07 Desarrollo/_Índice Desarrollo|Desarrollo]] · Relacionado: [[Spring Professional]]

> [!info] Estado de la certificación (`estado_cert`)
> **pendiente** → **preparando** → **lista** (cumples todos los criterios de abajo) → **agendada** (pon `fecha_examen`) → **obtenida** (llena fecha, `vence`, `credencial_id`, `enlace_verificacion` y adjunta el certificado) → **vencida** si no renuevas.

## Datos del examen

| Dato | Valor |
|---|---|
| Nombre | Oracle Certified Professional: Java SE 21 Developer o Java SE 25 Developer |
| Código | 1Z0-830 (Java SE 21) · 1Z0-831 (Java SE 25) |
| Duración | Pendiente de confirmar (la página de Oracle no cargó al verificar) |
| Preguntas | Opción múltiple con fragmentos de código; número y calificación Pendiente de confirmar |
| Precio | Pendiente de confirmar (consulta education.oracle.com; varía por país) |
| Idiomas | Pendiente de confirmar |
| Requisitos | Ninguno formal |
| Dónde | Oracle University (examen supervisado en línea; Pendiente de confirmar) |
| Vigencia | Pendiente de confirmar |

> Datos revisados el 2026-11-09. Confírmalos en la página oficial antes de pagar.

Fuentes: [1](https://education.oracle.com/java-se-21-developer-professional/pexam_1Z0-830) · [2](https://education.oracle.com/java-se-25-developer-professional/pexam_1Z0-831) · [3](https://blogs.oracle.com/oracleuniversity/announcing-oracle-certified-professional-java-se-21-developer-exam-and-java-se-21-programming-complete-course)

## Dónde y cómo hacerla

1. Elige versión: Java 25 (la más nueva) o Java 21.
2. Crea tu cuenta en Oracle University y compra el examen.
3. Revisa requisitos de equipo para el examen en línea.

## ¿Cuándo estoy listo?

- [ ] Todas las notas de 15-Java en `visto` o más.
- [ ] Predigo sin compilar la salida de fragmentos con genéricos, streams, records, sealed classes, pattern matching y concurrencia.
- [ ] Hice el examen de práctica de Oracle (Practice Exam: Java SE 21 Developer Professional) con 85 % o más dos veces.
- [ ] Repasé el temario oficial punto por punto y cada punto tiene una nota mía.

Cuando todo esté marcado, cambia `estado_cert` a **lista** y pídeme una revisión: reviso tu avance en la bóveda y te digo qué reforzar.

## Mi avance en los temas

```base
filters:
  and:
    - or:
        - file.inFolder("15-Java")
    - tipo != "indice"
views:
  - type: table
    name: Lo que me falta
    filters:
      and:
        - progreso != "dominado"
    order:
      - file.name
      - progreso
      - file.folder
  - type: table
    name: Dominado
    filters:
      and:
        - progreso == "dominado"
    order:
      - file.name
      - file.folder
```

## Exámenes de práctica

| Fecha | Fuente | Resultado | Temas flojos |
|---|---|---|---|
|  |  |  |  |

## Mi certificado

> [!tip] Cuando la obtengas
> 1. Descarga el certificado (PDF) y la insignia digital.
> 2. Guárdalos en la carpeta `Certificados` junto a esta nota como `Oracle Certified Professional Java SE - certificado.pdf`.
> 3. Quita el `%%` de la línea de abajo para verlo aquí.
> 4. Llena `fecha_obtenida`, `vence`, `credencial_id` y `enlace_verificacion`.

%% ![[Oracle Certified Professional Java SE - certificado.pdf]] %%

## Renovación

- [ ] Revisa en tu perfil de Oracle si la certificación tiene vigencia o se liga a la versión de Java.
