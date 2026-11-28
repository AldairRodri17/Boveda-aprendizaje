---
proyecto: Conocimiento
tipo: certificacion
proveedor: OffSec
codigo: "OSCP+ (PEN-200)"
estado_cert: pendiente
fecha_examen:
fecha_obtenida:
vence:
credencial_id:
enlace_verificacion:
costo_usd: 1699
vigencia: "OSCP no vence; el “+” vence a los 3 años"
verificado_el: 2026-11-09
area: Seguridad
tags: [certificacion, certificaciones]
contenido: completo
---
# OSCP

Parte de: [[18-Certificaciones/06 Seguridad/_Índice Seguridad|Seguridad]] · Paso previo recomendado: [[eJPT]]

> [!info] Estado de la certificación (`estado_cert`)
> **pendiente** → **preparando** → **lista** (cumples todos los criterios de abajo) → **agendada** (pon `fecha_examen`) → **obtenida** (llena fecha, `vence`, `credencial_id`, `enlace_verificacion` y adjunta el certificado) → **vencida** si no renuevas.

## Datos del examen

| Dato | Valor |
|---|---|
| Nombre | OffSec Certified Professional (OSCP / OSCP+) |
| Código | PEN-200 |
| Duración | Examen supervisado de unas 24 horas (más el reporte; Pendiente de confirmar el plazo exacto del reporte) |
| Formato | 3 máquinas independientes + 1 conjunto de Active Directory de 3 máquinas |
| Precio | Curso + examen 1,749 USD (90 días) · Learn One 2,199 USD/año (2 intentos) · Examen solo 1,699 USD |
| Idiomas | Inglés |
| Requisitos | Ninguno formal (recomendado: TCP/IP, administración Windows/Linux, Bash o Python) |
| Dónde | En línea por VPN, supervisado por personal de OffSec |
| Vigencia | OSCP sin vencimiento; OSCP+ 3 años (renueva con examen o CPE) |

> Datos revisados el 2026-11-09. Confírmalos en la página oficial antes de pagar.

Fuentes: [1](https://www.offsec.com/courses/pen-200/) · [2](https://help.offsec.com/hc/en-us/articles/24828221457940-OffSec-Certification-Exam-FAQ)

## Dónde y cómo hacerla

1. Compra PEN-200 (paquete o Learn One) y termina los laboratorios.
2. Agenda el examen con anticipación desde el portal de OffSec.
3. Prepara plantilla de reporte y entorno (VPN, capturas) antes del día.

## ¿Cuándo estoy listo?

- [ ] Todas las notas de 12-Seguridad/06 Hacking ético en `dominado`.
- [ ] Comprometí sin ayuda los laboratorios de PEN-200, incluido un conjunto de Active Directory completo.
- [ ] Hice un simulacro de 24 h con reporte profesional.
- [ ] Repasé el temario oficial punto por punto y cada punto tiene una nota mía.

Cuando todo esté marcado, cambia `estado_cert` a **lista** y pídeme una revisión: reviso tu avance en la bóveda y te digo qué reforzar.

## Mi avance en los temas

```base
filters:
  and:
    - or:
        - file.inFolder("12-Seguridad/06 Hacking ético")
        - file.inFolder("19-Redes")
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
> 2. Guárdalos en la carpeta `Certificados` junto a esta nota como `OSCP - certificado.pdf`.
> 3. Quita el `%%` de la línea de abajo para verlo aquí.
> 4. Llena `fecha_obtenida`, `vence`, `credencial_id` y `enlace_verificacion`.

%% ![[OSCP - certificado.pdf]] %%

## Renovación

- [ ] El “+” vence a los 3 años: renueva con examen de recertificación, otro examen de OffSec o CPE.
