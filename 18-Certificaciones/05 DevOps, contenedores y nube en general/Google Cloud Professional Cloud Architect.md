---
proyecto: Conocimiento
tipo: certificacion
proveedor: Google Cloud
codigo: "Professional Cloud Architect"
estado_cert: pendiente
fecha_examen:
fecha_obtenida:
vence:
credencial_id:
enlace_verificacion:
costo_usd: 200
vigencia: "2 años"
verificado_el: 2026-11-09
area: Nube
tags: [certificacion, certificaciones]
contenido: completo
---
# Google Cloud Professional Cloud Architect

Parte de: [[18-Certificaciones/05 DevOps, contenedores y nube en general/_Índice DevOps, contenedores y nube en general|DevOps, contenedores y nube en general]] · Relacionado: [[AZ-305 Azure Solutions Architect Expert]]

> [!info] Estado de la certificación (`estado_cert`)
> **pendiente** → **preparando** → **lista** (cumples todos los criterios de abajo) → **agendada** (pon `fecha_examen`) → **obtenida** (llena fecha, `vence`, `credencial_id`, `enlace_verificacion` y adjunta el certificado) → **vencida** si no renuevas.

## Datos del examen

| Dato | Valor |
|---|---|
| Nombre | Google Cloud Certified – Professional Cloud Architect |
| Código | Professional Cloud Architect |
| Duración | 2 horas |
| Preguntas | 50–60 de opción múltiple/selección múltiple; 2 casos de estudio (20–30 % de las preguntas) |
| Precio | 200 USD más impuestos |
| Idiomas | Inglés y japonés |
| Requisitos | Ninguno (recomendado: 3+ años de industria y 1+ en Google Cloud) |
| Dónde | En línea con supervisión o en centro Pearson VUE |
| Vigencia | 2 años; hay examen de renovación de 1 h (100 USD) centrado en IA generativa |

> Datos revisados el 2026-11-09. Confírmalos en la página oficial antes de pagar.

Fuentes: [1](https://cloud.google.com/learn/certification/cloud-architect)

## Dónde y cómo hacerla

1. Crea tu cuenta en el portal de certificación de Google Cloud.
2. Agenda en línea o en centro Pearson VUE (en inglés).
3. Lee antes los casos de estudio publicados en la guía del examen.

## ¿Cuándo estoy listo?

- [ ] Leí y resolví los casos de estudio de la guía oficial (diseño, migración, seguridad, costos).
- [ ] Explico VPC, IAM, GKE, Cloud Run, Cloud SQL/Spanner, BigQuery y patrones de alta disponibilidad en Google Cloud.
- [ ] 85 % o más en dos exámenes de práctica seguidos (fuentes oficiales o reconocidas; nunca *dumps*).
- [ ] Repasé el temario oficial punto por punto y cada punto tiene una nota mía.

Cuando todo esté marcado, cambia `estado_cert` a **lista** y pídeme una revisión: reviso tu avance en la bóveda y te digo qué reforzar.

## Mi avance en los temas

```base
filters:
  and:
    - or:
        - file.inFolder("09-Arquitectura-Patrones")
        - file.inFolder("08-Automatizacion-DevOps/02 Operación en producción")
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
> 2. Guárdalos en la carpeta `Certificados` junto a esta nota como `Google Cloud Professional Cloud Architect - certificado.pdf`.
> 3. Quita el `%%` de la línea de abajo para verlo aquí.
> 4. Llena `fecha_obtenida`, `vence`, `credencial_id` y `enlace_verificacion`.

%% ![[Google Cloud Professional Cloud Architect - certificado.pdf]] %%

## Renovación

- [ ] Vence a los 2 años: renueva con el examen estándar, el examen corto de renovación o la ruta de renovación de Google Skills.
