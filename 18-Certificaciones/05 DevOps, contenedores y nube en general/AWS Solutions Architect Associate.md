---
proyecto: Conocimiento
tipo: certificacion
proveedor: AWS
codigo: "SAA-C03"
estado_cert: pendiente
fecha_examen:
fecha_obtenida:
vence:
credencial_id:
enlace_verificacion:
costo_usd: 150
vigencia: "3 años"
verificado_el: 2026-11-09
area: Nube
tags: [certificacion, certificaciones]
---
# AWS Solutions Architect Associate

Parte de: [[18-Certificaciones/05 DevOps, contenedores y nube en general/_Índice DevOps, contenedores y nube en general|DevOps, contenedores y nube en general]] · Paso previo opcional: [[AWS Cloud Practitioner]]

> [!info] Estado de la certificación (`estado_cert`)
> **pendiente** → **preparando** → **lista** (cumples todos los criterios de abajo) → **agendada** (pon `fecha_examen`) → **obtenida** (llena fecha, `vence`, `credencial_id`, `enlace_verificacion` y adjunta el certificado) → **vencida** si no renuevas.

## Datos del examen

| Dato | Valor |
|---|---|
| Nombre | AWS Certified Solutions Architect – Associate |
| Código | SAA-C03 |
| Duración | 130 minutos |
| Preguntas | 65 (opción múltiple o respuesta múltiple) |
| Precio | 150 USD más cargos posibles |
| Idiomas | 10 idiomas, incluido español (Latinoamérica) |
| Requisitos | Ninguno formal (recomendado: 1 año diseñando en AWS) |
| Dónde | Pearson VUE (centro u en línea) |
| Vigencia | 3 años |

> Datos revisados el 2026-11-09. Confírmalos en la página oficial antes de pagar.

Fuentes: [1](https://aws.amazon.com/certification/certified-solutions-architect-associate/) · [2](https://docs.aws.amazon.com/aws-certification/latest/solutions-architect-associate-03/solutions-architect-associate-03.html)

## Dónde y cómo hacerla

1. Crea tu cuenta en AWS Certification y agenda en Pearson VUE.
2. Elige idioma y modalidad.
3. Paga o usa un voucher/beneficio de AWS.

## ¿Cuándo estoy listo?

- [ ] Diseño una arquitectura de 3 capas en AWS con VPC pública/privada, ALB, Auto Scaling, RDS Multi-AZ y S3, y la justifico por costo, seguridad y resiliencia.
- [ ] Todas las notas de 09-Arquitectura-Patrones/03 Patrones de nube y sistemas distribuidos en `visto` o más.
- [ ] 85 % o más en dos exámenes de práctica seguidos (fuentes oficiales o reconocidas; nunca *dumps*).
- [ ] Repasé el temario oficial punto por punto y cada punto tiene una nota mía.

Cuando todo esté marcado, cambia `estado_cert` a **lista** y pídeme una revisión: reviso tu avance en la bóveda y te digo qué reforzar.

## Mi avance en los temas

```base
filters:
  and:
    - or:
        - file.inFolder("09-Arquitectura-Patrones/03 Patrones de nube y sistemas distribuidos")
        - file.inFolder("19-Redes/03 Direccionamiento IP")
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
> 2. Guárdalos en la carpeta `Certificados` junto a esta nota como `AWS Solutions Architect Associate - certificado.pdf`.
> 3. Quita el `%%` de la línea de abajo para verlo aquí.
> 4. Llena `fecha_obtenida`, `vence`, `credencial_id` y `enlace_verificacion`.

%% ![[AWS Solutions Architect Associate - certificado.pdf]] %%

## Renovación

- [ ] Vence a los 3 años: renueva con la versión vigente o con Solutions Architect – Professional.
