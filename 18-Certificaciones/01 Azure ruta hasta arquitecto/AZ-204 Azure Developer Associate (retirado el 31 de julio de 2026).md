---
proyecto: Conocimiento
tipo: certificacion
proveedor: Microsoft
codigo: "AZ-204"
estado_cert: pendiente
fecha_examen:
fecha_obtenida:
vence:
credencial_id:
enlace_verificacion:
costo_usd: 
vigencia: "1 año para quien ya la tiene; ya no se puede obtener"
verificado_el: 2026-11-09
area: Azure
tags: [certificacion, certificaciones]
---
# AZ-204 Azure Developer Associate (retirado el 31 de julio de 2026)

Parte de: [[18-Certificaciones/01 Azure ruta hasta arquitecto/_Índice Azure ruta hasta arquitecto|Azure ruta hasta arquitecto]] · Reemplazo práctico: [[AI-200 Azure AI Cloud Developer Associate (sucesor de AZ-204)]]

> [!info] Estado de la certificación (`estado_cert`)
> **pendiente** → **preparando** → **lista** (cumples todos los criterios de abajo) → **agendada** (pon `fecha_examen`) → **obtenida** (llena fecha, `vence`, `credencial_id`, `enlace_verificacion` y adjunta el certificado) → **vencida** si no renuevas.

## Datos del examen

| Dato | Valor |
|---|---|
| Nombre | Microsoft Certified: Azure Developer Associate |
| Código | AZ-204 |
| Duración | Ya no se puede presentar |
| Precio | Varía por país o región (más impuestos); el precio exacto aparece al agendar en Pearson VUE |
| Idiomas | — |
| Requisitos | — |
| Dónde | Pearson VUE: centro de examen o en línea (OnVUE) |
| Vigencia | 1 año para quien ya la tiene; ya no se puede obtener |
| Estado | **Retirado el 31 de julio de 2026** (confirmado en la lista oficial de exámenes retirados de Microsoft) |
| Ojo con AZ-400 | La página de DevOps Engineer Expert todavía acepta *Azure Developer Associate* como requisito; no menciona AI-200 (Pendiente de confirmar si Microsoft la agregará). |

> Datos revisados el 2026-11-09. Confírmalos en la página oficial antes de pagar.

Fuentes: [1](https://learn.microsoft.com/en-us/credentials/support/retired-certification-exams) · [2](https://learn.microsoft.com/en-us/credentials/certifications/devops-engineer/) · [3](https://learn.microsoft.com/en-us/answers/questions/5955907/will-ai-200-replace-az-204-after-its-retirement)

## Dónde y cómo hacerla

1. Inicia sesión en Microsoft Learn con una cuenta personal (MSA), no la del trabajo: si dejas la empresa pierdes el historial.
2. En la página de la certificación elige **Programar examen** → Pearson VUE (centro de examen o en línea con OnVUE).
3. Revisa el precio que te muestra para México, paga con tarjeta o voucher y elige idioma (si no está en tu idioma puedes pedir 30 min extra).
4. Si repruebas puedes reintentar a las 24 h; los siguientes intentos tienen esperas mayores.

## ¿Cuándo estoy listo?

- [ ] No la agendes: el examen AZ-204 se retiró el 31 de julio de 2026. Prepara [[AI-200 Azure AI Cloud Developer Associate (sucesor de AZ-204)]] en su lugar.
- [ ] Si ya la tienes: renuévala antes de su vencimiento si Microsoft aún te ofrece la evaluación; si no, pasará a certificaciones históricas.

Cuando todo esté marcado, cambia `estado_cert` a **lista** y pídeme una revisión: reviso tu avance en la bóveda y te digo qué reforzar.

## Mi avance en los temas

```base
filters:
  and:
    - or:
        - file.inFolder("07-Azure/03 Cómputo")
        - file.inFolder("07-Azure/04 Datos")
        - file.inFolder("07-Azure/05 Integración")
        - file.inFolder("07-Azure/06 Seguridad")
        - file.inFolder("07-Azure/08 Observabilidad")
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
> 2. Guárdalos en la carpeta `Certificados` junto a esta nota como `AZ-204 Azure Developer Associate (retirado el 31 de julio de 2026) - certificado.pdf`.
> 3. Quita el `%%` de la línea de abajo para verlo aquí.
> 4. Llena `fecha_obtenida`, `vence`, `credencial_id` y `enlace_verificacion`.

%% ![[AZ-204 Azure Developer Associate (retirado el 31 de julio de 2026) - certificado.pdf]] %%

## Renovación

Según una respuesta en Microsoft Q&A (no es página oficial), al retirarse el examen la certificación ya no se puede renovar y pasa a *Historical certifications* al vencer. Pendiente de confirmar en tu perfil de Learn.
