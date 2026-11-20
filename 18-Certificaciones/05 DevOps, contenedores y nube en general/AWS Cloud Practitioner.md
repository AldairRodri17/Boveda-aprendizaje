---
proyecto: Conocimiento
tipo: certificacion
proveedor: AWS
codigo: "CLF-C02"
estado_cert: pendiente
fecha_examen:
fecha_obtenida:
vence:
credencial_id:
enlace_verificacion:
costo_usd: 100
vigencia: "3 años"
verificado_el: 2026-11-09
area: Nube
tags: [certificacion, certificaciones]
---
# AWS Cloud Practitioner

Parte de: [[18-Certificaciones/05 DevOps, contenedores y nube en general/_Índice DevOps, contenedores y nube en general|DevOps, contenedores y nube en general]] · Siguiente: [[AWS Solutions Architect Associate]]

> [!info] Estado de la certificación (`estado_cert`)
> **pendiente** → **preparando** → **lista** (cumples todos los criterios de abajo) → **agendada** (pon `fecha_examen`) → **obtenida** (llena fecha, `vence`, `credencial_id`, `enlace_verificacion` y adjunta el certificado) → **vencida** si no renuevas.

## Datos del examen

| Dato | Valor |
|---|---|
| Nombre | AWS Certified Cloud Practitioner |
| Código | CLF-C02 |
| Duración | 90 minutos |
| Preguntas | 65 (opción múltiple o respuesta múltiple) |
| Precio | 100 USD más cargos posibles (tipo de cambio, impuestos) |
| Idiomas | 12 idiomas, incluido español (Latinoamérica) |
| Requisitos | Ninguno |
| Dónde | Pearson VUE (centro u en línea) |
| Vigencia | 3 años |

> Datos revisados el 2026-11-09. Confírmalos en la página oficial antes de pagar.

Fuentes: [1](https://aws.amazon.com/certification/certified-cloud-practitioner/) · [2](https://docs.aws.amazon.com/aws-certification/latest/examguides/cloud-practitioner-02.html)

## Dónde y cómo hacerla

1. Crea tu cuenta en AWS Certification (aws.training) y entra a Pearson VUE desde ahí.
2. Elige idioma español (Latinoamérica), centro o en línea.
3. Paga o usa un voucher/beneficio de AWS.

## ¿Cuándo estoy listo?

- [ ] Explico modelo de responsabilidad compartida, regiones/zonas, IAM, EC2, S3, VPC, RDS, Lambda y modelos de precio de AWS.
- [ ] Hice el examen de práctica oficial de AWS Skill Builder.
- [ ] 85 % o más en dos exámenes de práctica seguidos (fuentes oficiales o reconocidas; nunca *dumps*).
- [ ] Repasé el temario oficial punto por punto y cada punto tiene una nota mía.

Cuando todo esté marcado, cambia `estado_cert` a **lista** y pídeme una revisión: reviso tu avance en la bóveda y te digo qué reforzar.

## Mi avance en los temas

```base
filters:
  and:
    - or:
        - file.inFolder("07-Azure/01 Fundamentos")
        - file.inFolder("02-Fundamentos/01 Internet y web")
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
> 2. Guárdalos en la carpeta `Certificados` junto a esta nota como `AWS Cloud Practitioner - certificado.pdf`.
> 3. Quita el `%%` de la línea de abajo para verlo aquí.
> 4. Llena `fecha_obtenida`, `vence`, `credencial_id` y `enlace_verificacion`.

%% ![[AWS Cloud Practitioner - certificado.pdf]] %%

## Renovación

- [ ] Vence a los 3 años: renueva con el curso Cloud Quest de recertificación, con la versión vigente del examen o con un examen Associate/Professional.
