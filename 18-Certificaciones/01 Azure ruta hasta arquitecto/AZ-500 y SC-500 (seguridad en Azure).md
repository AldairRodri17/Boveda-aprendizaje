---
proyecto: Conocimiento
tipo: certificacion
proveedor: Microsoft
codigo: "SC-500"
estado_cert: pendiente
fecha_examen:
fecha_obtenida:
vence:
credencial_id:
enlace_verificacion:
costo_usd: 
vigencia: "1 año (renovación gratuita en línea)"
verificado_el: 2026-11-09
area: Seguridad
tags: [certificacion, certificaciones]
contenido: completo
---
# AZ-500 y SC-500 (seguridad en Azure)

Parte de: [[18-Certificaciones/01 Azure ruta hasta arquitecto/_Índice Azure ruta hasta arquitecto|Azure ruta hasta arquitecto]] · Siguiente: [[SC-100 Cybersecurity Architect Expert]] (SC-500 es uno de sus requisitos)

> [!info] Estado de la certificación (`estado_cert`)
> **pendiente** → **preparando** → **lista** (cumples todos los criterios de abajo) → **agendada** (pon `fecha_examen`) → **obtenida** (llena fecha, `vence`, `credencial_id`, `enlace_verificacion` y adjunta el certificado) → **vencida** si no renuevas.

## Datos del examen

| Dato | Valor |
|---|---|
| Nombre | Microsoft Certified: Cloud and AI Security Engineer Associate |
| Código | SC-500 |
| Duración | 120 minutos |
| Precio | Varía por país o región (más impuestos); el precio exacto aparece al agendar en Pearson VUE |
| Idiomas | 10 idiomas, incluido español |
| Requisitos | Ninguno formal (recomendado: administrar Azure e híbrido, Entra ID y Microsoft 365) |
| Dónde | Pearson VUE: centro de examen o en línea (OnVUE) |
| Vigencia | 1 año (renovación gratuita en línea) |
| AZ-500 | **AZ-500 (Azure Security Engineer Associate) se retiró el 31 de agosto de 2026.** SC-500 es la certificación de seguridad de nube vigente; la página oficial no dice explícitamente que la reemplaza. |
| Temario | Identidad, acceso y gobierno; almacenamiento, bases de datos y redes; cómputo; postura de seguridad y monitoreo |

> Datos revisados el 2026-11-09. Confírmalos en la página oficial antes de pagar.

Fuentes: [1](https://learn.microsoft.com/en-us/credentials/certifications/cloud-and-ai-security-engineer-associate/) · [2](https://learn.microsoft.com/en-us/credentials/support/retired-certification-exams) · [3](https://learn.microsoft.com/en-us/credentials/certifications/resources/study-guides/sc-500) · [4](https://learn.microsoft.com/en-us/credentials/certifications/renew-your-microsoft-certification-faq)

## Dónde y cómo hacerla

1. Inicia sesión en Microsoft Learn con una cuenta personal (MSA), no la del trabajo: si dejas la empresa pierdes el historial.
2. En la página de la certificación elige **Programar examen** → Pearson VUE (centro de examen o en línea con OnVUE).
3. Revisa el precio que te muestra para México, paga con tarjeta o voucher y elige idioma (si no está en tu idioma puedes pedir 30 min extra).
4. Si repruebas puedes reintentar a las 24 h; los siguientes intentos tienen esperas mayores.

## ¿Cuándo estoy listo?

- [ ] Todas las notas de 07-Azure/06 Seguridad en `visto` o más.
- [ ] Todas las notas de 07-Azure/07 Redes en Azure en `visto` o más.
- [ ] Configuro en mi propia suscripción (los sandboxes de Microsoft Learn ya se retiraron; usa un grupo de recursos desechable con presupuesto): acceso condicional/PIM (conceptual), NSG y Private Endpoint, Key Vault con RBAC y Defender for Cloud.
- [ ] La evaluación de práctica oficial de SC-500 aún no está disponible (Microsoft la publica unas 8 semanas después de salir de beta): mientras, 85 % o más en dos simulacros de fuente confiable.
- [ ] Repasé el temario oficial (study guide) punto por punto y cada punto tiene una nota mía.

Cuando todo esté marcado, cambia `estado_cert` a **lista** y pídeme una revisión: reviso tu avance en la bóveda y te digo qué reforzar.

## Mi avance en los temas

```base
filters:
  and:
    - or:
        - file.inFolder("07-Azure/06 Seguridad")
        - file.inFolder("07-Azure/07 Redes en Azure")
        - file.inFolder("12-Seguridad/01 Fundamentos de seguridad")
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
> 2. Guárdalos en la carpeta `Certificados` junto a esta nota como `AZ-500 y SC-500 (seguridad en Azure) - certificado.pdf`.
> 3. Quita el `%%` de la línea de abajo para verlo aquí.
> 4. Llena `fecha_obtenida`, `vence`, `credencial_id` y `enlace_verificacion`.

%% ![[AZ-500 y SC-500 (seguridad en Azure) - certificado.pdf]] %%

## Renovación

Las certificaciones associate, expert y specialty de Microsoft vencen al año. La evaluación de renovación es gratis, en línea en Microsoft Learn, se abre 6 meses antes del vencimiento y se puede repetir (después del segundo intento, espera de 24 h).

- [ ] Cuando la obtengas, pon `vence` (1 año después) y un recordatorio 6 meses antes.
