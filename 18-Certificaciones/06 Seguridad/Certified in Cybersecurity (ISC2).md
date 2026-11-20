---
proyecto: Conocimiento
tipo: certificacion
proveedor: ISC2
codigo: "CC"
estado_cert: pendiente
fecha_examen:
fecha_obtenida:
vence:
credencial_id:
enlace_verificacion:
costo_usd: 199
vigencia: "3 años (CPE y cuota anual)"
verificado_el: 2026-11-09
area: Seguridad
tags: [certificacion, certificaciones]
---
# Certified in Cybersecurity (ISC2)

Parte de: [[18-Certificaciones/06 Seguridad/_Índice Seguridad|Seguridad]] · Siguiente: [[CompTIA Security+]]

> [!info] Estado de la certificación (`estado_cert`)
> **pendiente** → **preparando** → **lista** (cumples todos los criterios de abajo) → **agendada** (pon `fecha_examen`) → **obtenida** (llena fecha, `vence`, `credencial_id`, `enlace_verificacion` y adjunta el certificado) → **vencida** si no renuevas.

## Datos del examen

| Dato | Valor |
|---|---|
| Nombre | ISC2 Certified in Cybersecurity (CC) |
| Código | CC (temario vigente desde el 1 de septiembre de 2026) |
| Duración | Pendiente de confirmar |
| Precio | 199 USD en América (paquete con curso, voucher y 2 intentos); el programa gratuito “One Million Certified in Cybersecurity” está concluyendo (Pendiente de confirmar si sigue disponible) |
| Idiomas | Incluye español |
| Requisitos | Ninguno (nivel de entrada, sin experiencia laboral) |
| Dónde | Pearson VUE |
| Vigencia | Cuota anual de mantenimiento de 50 USD; ciclo y CPE: Pendiente de confirmar |

> Datos revisados el 2026-11-09. Confírmalos en la página oficial antes de pagar.

Fuentes: [1](https://www.isc2.org/certifications/cc) · [2](https://www.isc2.org/register-for-exam/isc2-exam-pricing) · [3](https://www.isc2.org/training/online-self-paced/spanish) · [4](https://www.isc2.org/certifications/cc/cc-certification-exam-outline)

## Dónde y cómo hacerla

1. Crea tu cuenta en ISC2 y revisa si aún aplica el programa gratuito.
2. Toma el curso oficial a tu ritmo (hay versión en español).
3. Agenda en Pearson VUE.

## ¿Cuándo estoy listo?

- [ ] Todas las notas de 12-Seguridad/01 Fundamentos de seguridad en `visto` o más.
- [ ] Explico principios (CIA), gobierno, control de acceso, redes y nube, operaciones y respuesta a incidentes.
- [ ] 85 % o más en dos exámenes de práctica seguidos (fuentes oficiales o reconocidas; nunca *dumps*).
- [ ] Repasé el temario oficial punto por punto y cada punto tiene una nota mía.

Cuando todo esté marcado, cambia `estado_cert` a **lista** y pídeme una revisión: reviso tu avance en la bóveda y te digo qué reforzar.

## Mi avance en los temas

```base
filters:
  and:
    - or:
        - file.inFolder("12-Seguridad/01 Fundamentos de seguridad")
        - file.inFolder("19-Redes/07 Seguridad de redes")
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
> 2. Guárdalos en la carpeta `Certificados` junto a esta nota como `Certified in Cybersecurity (ISC2) - certificado.pdf`.
> 3. Quita el `%%` de la línea de abajo para verlo aquí.
> 4. Llena `fecha_obtenida`, `vence`, `credencial_id` y `enlace_verificacion`.

%% ![[Certified in Cybersecurity (ISC2) - certificado.pdf]] %%

## Renovación

- [ ] Paga la cuota anual y junta CPE. Pon `vence`.
