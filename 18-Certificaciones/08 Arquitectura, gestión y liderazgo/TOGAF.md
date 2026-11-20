---
proyecto: Conocimiento
tipo: certificacion
proveedor: The Open Group
codigo: "OGEA-101 / OGEA-102 / OGEA-103"
estado_cert: pendiente
fecha_examen:
fecha_obtenida:
vence:
credencial_id:
enlace_verificacion:
costo_usd: 405
vigencia: "Pendiente de confirmar"
verificado_el: 2026-11-09
area: Arquitectura
tags: [certificacion, certificaciones]
---
# TOGAF

Parte de: [[18-Certificaciones/08 Arquitectura, gestión y liderazgo/_Índice Arquitectura, gestión y liderazgo|Arquitectura, gestión y liderazgo]] · Relacionado: [[AZ-305 Azure Solutions Architect Expert]]

> [!info] Estado de la certificación (`estado_cert`)
> **pendiente** → **preparando** → **lista** (cumples todos los criterios de abajo) → **agendada** (pon `fecha_examen`) → **obtenida** (llena fecha, `vence`, `credencial_id`, `enlace_verificacion` y adjunta el certificado) → **vencida** si no renuevas.

## Datos del examen

| Dato | Valor |
|---|---|
| Nombre | TOGAF Enterprise Architecture Foundation (Parte 1) y Practitioner (Parte 2) |
| Código | OGEA-101 (Foundation) · OGEA-102 (Practitioner) · OGEA-103 (combinado) |
| Duración | Foundation 60 min (40 preguntas, libro cerrado) · Practitioner 90 min (8 escenarios, libro abierto) · Combinado 150 min |
| Aprobación | 60 % en cada parte |
| Precio | Desde el 1 de febrero de 2026: 405 USD cada parte reservando directo en Pearson; combinado 610 USD (más impuestos; algunos países pagan en moneda local) |
| Idiomas | Inglés, francés y chino simplificado (combinado: inglés y chino) |
| Requisitos | Foundation: ninguno · Practitioner: tener Foundation |
| Dónde | Pearson VUE: centro u OnVUE en línea |
| Vigencia | Pendiente de confirmar |

> Datos revisados el 2026-11-09. Confírmalos en la página oficial antes de pagar.

Fuentes: [1](https://certification.opengroup.org/docs/datasheets/togaf-exams.pdf) · [2](https://certification.opengroup.org/docs/exam-fee-schedule-retail.pdf)

## Dónde y cómo hacerla

1. Descarga el estándar TOGAF 10 y la guía de estudio desde The Open Group.
2. Agenda en Pearson VUE la Parte 1, la 2 o el combinado.

## ¿Cuándo estoy listo?

- [ ] Todas las notas de 09-Arquitectura-Patrones/06 Documentar decisiones en `visto` o más.
- [ ] Explico el ciclo ADM fase por fase, el repositorio de arquitectura y la gobernanza.
- [ ] 85 % o más en dos exámenes de práctica seguidos (fuentes oficiales o reconocidas; nunca *dumps*).
- [ ] Repasé el temario oficial punto por punto y cada punto tiene una nota mía.

Cuando todo esté marcado, cambia `estado_cert` a **lista** y pídeme una revisión: reviso tu avance en la bóveda y te digo qué reforzar.

## Mi avance en los temas

```base
filters:
  and:
    - or:
        - file.inFolder("09-Arquitectura-Patrones")
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
> 2. Guárdalos en la carpeta `Certificados` junto a esta nota como `TOGAF - certificado.pdf`.
> 3. Quita el `%%` de la línea de abajo para verlo aquí.
> 4. Llena `fecha_obtenida`, `vence`, `credencial_id` y `enlace_verificacion`.

%% ![[TOGAF - certificado.pdf]] %%
