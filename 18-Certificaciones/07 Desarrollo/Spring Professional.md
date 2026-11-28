---
proyecto: Conocimiento
tipo: certificacion
proveedor: Broadcom (VMware)
codigo: "2V0-72.22"
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
contenido: completo
---
# Spring Professional

Parte de: [[18-Certificaciones/07 Desarrollo/_Índice Desarrollo|Desarrollo]] · Paso previo recomendado: [[Oracle Certified Professional Java SE]]

> [!info] Estado de la certificación (`estado_cert`)
> **pendiente** → **preparando** → **lista** (cumples todos los criterios de abajo) → **agendada** (pon `fecha_examen`) → **obtenida** (llena fecha, `vence`, `credencial_id`, `enlace_verificacion` y adjunta el certificado) → **vencida** si no renuevas.

## Datos del examen

| Dato | Valor |
|---|---|
| Nombre | Spring Certified Professional (examen Spring Professional Develop) |
| Código | 2V0-72.22 |
| Duración | 130 minutos |
| Preguntas | 60; aprobación 300 (escala) |
| Precio | Pendiente de confirmar |
| Idiomas | Pendiente de confirmar |
| Requisitos | Según el personal de Broadcom, para recibir la certificación se requiere también un curso oficial (Pendiente de confirmar si sigue igual) |
| Dónde | Pearson VUE |
| Vigencia | Pendiente de confirmar |
| Ojo | La guía oficial es de enero de 2024 y referencia Spring Boot 2.5; hubo reportes de problemas con el examen en 2025. Confirma en Broadcom que siga vigente antes de pagar. |

> Datos revisados el 2026-11-09. Confírmalos en la página oficial antes de pagar.

Fuentes: [1](https://docs.broadcom.com/doc/vmw-spring-professional-develop-exam-guide) · [2](https://community.broadcom.com/vci/discussion/exam-edu-1202-spring-professional)

## Dónde y cómo hacerla

1. Confirma con Broadcom (certificación de Spring / Spring Academy) que el examen siga disponible y qué curso exige.
2. Agenda en Pearson VUE.

## ¿Cuándo estoy listo?

- [ ] Todas las notas de 15-Java en `visto` o más.
- [ ] Construyo una API con Spring Boot: inyección de dependencias, JPA, transacciones, seguridad, pruebas y Actuator.
- [ ] 85 % o más en dos exámenes de práctica seguidos (fuentes oficiales o reconocidas; nunca *dumps*).
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
> 2. Guárdalos en la carpeta `Certificados` junto a esta nota como `Spring Professional - certificado.pdf`.
> 3. Quita el `%%` de la línea de abajo para verlo aquí.
> 4. Llena `fecha_obtenida`, `vence`, `credencial_id` y `enlace_verificacion`.

%% ![[Spring Professional - certificado.pdf]] %%
