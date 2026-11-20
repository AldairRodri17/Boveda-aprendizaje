---
proyecto: Conocimiento
tipo: certificacion
proveedor: HashiCorp
codigo: "Terraform Associate (004)"
estado_cert: pendiente
fecha_examen:
fecha_obtenida:
vence:
credencial_id:
enlace_verificacion:
costo_usd: 70.50
vigencia: "2 años"
verificado_el: 2026-11-09
area: DevOps
tags: [certificacion, certificaciones]
---
# Terraform Associate

Parte de: [[18-Certificaciones/05 DevOps, contenedores y nube en general/_Índice DevOps, contenedores y nube en general|DevOps, contenedores y nube en general]] · Relacionado: [[AZ-400 DevOps Engineer Expert]]

> [!info] Estado de la certificación (`estado_cert`)
> **pendiente** → **preparando** → **lista** (cumples todos los criterios de abajo) → **agendada** (pon `fecha_examen`) → **obtenida** (llena fecha, `vence`, `credencial_id`, `enlace_verificacion` y adjunta el certificado) → **vencida** si no renuevas.

## Datos del examen

| Dato | Valor |
|---|---|
| Nombre | HashiCorp Certified: Terraform Associate (004) |
| Código | 004 (evalúa Terraform 1.12) |
| Duración | 1 hora |
| Precio | 70.50 USD más impuestos locales (sin reintento gratis) |
| Idiomas | Inglés |
| Requisitos | Ninguno formal (terminal y nociones de nube) |
| Dónde | En línea con supervisión (opción múltiple) |
| Vigencia | 2 años |

> Datos revisados el 2026-11-09. Confírmalos en la página oficial antes de pagar.

Fuentes: [1](https://developer.hashicorp.com/certifications/terraform-associate) · [2](https://developer.hashicorp.com/terraform/tutorials/certification-004)

## Dónde y cómo hacerla

1. Crea tu cuenta en el portal de certificaciones de HashiCorp desde la página oficial.
2. Compra el examen y agenda la sesión en línea.
3. Prepara una computadora y cuarto que cumplan las reglas del supervisor en línea.

## ¿Cuándo estoy listo?

- [ ] Todas las notas de 07-Azure/10 Infraestructura como código en `visto` o más.
- [ ] Hago un módulo propio con variables, outputs y `for_each`, y uso estado remoto en Azure Storage con bloqueo.
- [ ] Explico `plan`, `apply`, `import`, `state mv`, workspaces y HCP Terraform.
- [ ] Hice las preguntas de muestra oficiales y las tutorías de preparación 004.
- [ ] 85 % o más en dos exámenes de práctica seguidos (fuentes oficiales o reconocidas; nunca *dumps*).
- [ ] Repasé el temario oficial punto por punto y cada punto tiene una nota mía.

Cuando todo esté marcado, cambia `estado_cert` a **lista** y pídeme una revisión: reviso tu avance en la bóveda y te digo qué reforzar.

## Mi avance en los temas

```base
filters:
  and:
    - or:
        - file.inFolder("07-Azure/10 Infraestructura como código")
        - file.inFolder("08-Automatizacion-DevOps")
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
> 2. Guárdalos en la carpeta `Certificados` junto a esta nota como `Terraform Associate - certificado.pdf`.
> 3. Quita el `%%` de la línea de abajo para verlo aquí.
> 4. Llena `fecha_obtenida`, `vence`, `credencial_id` y `enlace_verificacion`.

%% ![[Terraform Associate - certificado.pdf]] %%

## Renovación

- [ ] Vence a los 2 años: vuelve a presentar la versión vigente. Pon `vence`.
