---
proyecto: Conocimiento
tipo: certificacion
proveedor: Microsoft
codigo: "AZ-900"
estado_cert: pendiente
fecha_examen:
fecha_obtenida:
vence:
credencial_id:
enlace_verificacion:
costo_usd: 
vigencia: "No vence (las Fundamentals no requieren renovación)"
verificado_el: 2026-11-09
area: Azure
tags: [certificacion, certificaciones]
---
# AZ-900 Azure Fundamentals (opcional)

Parte de: [[18-Certificaciones/01 Azure ruta hasta arquitecto/_Índice Azure ruta hasta arquitecto|Azure ruta hasta arquitecto]] · Siguiente: [[AZ-104 Azure Administrator Associate]]

> [!info] Estado de la certificación (`estado_cert`)
> **pendiente** → **preparando** → **lista** (cumples todos los criterios de abajo) → **agendada** (pon `fecha_examen`) → **obtenida** (llena fecha, `vence`, `credencial_id`, `enlace_verificacion` y adjunta el certificado) → **vencida** si no renuevas.

## Datos del examen

| Dato | Valor |
|---|---|
| Nombre | Microsoft Certified: Azure Fundamentals |
| Código | AZ-900 |
| Duración | 45 minutos (65 de tiempo en sala) |
| Precio | Varía por país o región (más impuestos); el precio exacto aparece al agendar en Pearson VUE |
| Idiomas | 13 idiomas, incluido español |
| Requisitos | Ninguno |
| Dónde | Pearson VUE: centro de examen o en línea (OnVUE) |
| Vigencia | No vence (las Fundamentals no requieren renovación) |

> Datos revisados el 2026-11-09. Confírmalos en la página oficial antes de pagar.

Fuentes: [1](https://learn.microsoft.com/en-us/credentials/certifications/azure-fundamentals/) · [2](https://learn.microsoft.com/en-us/credentials/support/exam-duration-exam-experience) · [3](https://learn.microsoft.com/en-us/credentials/certifications/renew-your-microsoft-certification-faq)

## Dónde y cómo hacerla

1. Inicia sesión en Microsoft Learn con una cuenta personal (MSA), no la del trabajo: si dejas la empresa pierdes el historial.
2. En la página de la certificación elige **Programar examen** → Pearson VUE (centro de examen o en línea con OnVUE).
3. Revisa el precio que te muestra para México, paga con tarjeta o voucher y elige idioma (si no está en tu idioma puedes pedir 30 min extra).
4. Si repruebas puedes reintentar a las 24 h; los siguientes intentos tienen esperas mayores.

## ¿Cuándo estoy listo?

- [ ] Todas las notas de [[07-Azure/_Índice Azure|07-Azure]] sección 01 Fundamentos en `visto` o más.
- [ ] Explico sin apuntes modelos de servicio (IaaS, PaaS, SaaS), regiones y zonas, y la jerarquía grupo de administración → suscripción → grupo de recursos.
- [ ] 85 % o más en dos evaluaciones de práctica oficiales de Microsoft seguidas.
- [ ] Repasé el temario oficial (study guide) punto por punto y cada punto tiene una nota mía.

Cuando todo esté marcado, cambia `estado_cert` a **lista** y pídeme una revisión: reviso tu avance en la bóveda y te digo qué reforzar.

## Mi avance en los temas

```base
filters:
  and:
    - or:
        - file.inFolder("07-Azure/01 Fundamentos")
        - file.inFolder("07-Azure/02 Dónde probar Azure sin pagar")
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
> 2. Guárdalos en la carpeta `Certificados` junto a esta nota como `AZ-900 Azure Fundamentals (opcional) - certificado.pdf`.
> 3. Quita el `%%` de la línea de abajo para verlo aquí.
> 4. Llena `fecha_obtenida`, `vence`, `credencial_id` y `enlace_verificacion`.

%% ![[AZ-900 Azure Fundamentals (opcional) - certificado.pdf]] %%

## Renovación

Las certificaciones Fundamentals de Microsoft no vencen: no hay que renovar.
