---
proyecto: Conocimiento
tipo: certificacion
proveedor: Microsoft
codigo: "SC-200"
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
# SC-200 Security Operations Analyst

Parte de: [[18-Certificaciones/06 Seguridad/_Índice Seguridad|Seguridad]] · Paso previo opcional: [[SC-900 Security, Compliance and Identity Fundamentals]] · Siguiente: [[SC-100 Cybersecurity Architect Expert]]

> [!info] Estado de la certificación (`estado_cert`)
> **pendiente** → **preparando** → **lista** (cumples todos los criterios de abajo) → **agendada** (pon `fecha_examen`) → **obtenida** (llena fecha, `vence`, `credencial_id`, `enlace_verificacion` y adjunta el certificado) → **vencida** si no renuevas.

## Datos del examen

| Dato | Valor |
|---|---|
| Nombre | Microsoft Certified: Security Operations Analyst Associate |
| Código | SC-200 |
| Duración | 100 minutos |
| Precio | Varía por país o región (más impuestos); el precio exacto aparece al agendar en Pearson VUE |
| Idiomas | 10 idiomas, incluido español |
| Requisitos | Ninguno formal |
| Dónde | Pearson VUE: centro de examen o en línea (OnVUE) |
| Vigencia | 1 año (renovación gratuita en línea) |
| Temario | Administrar el entorno de operaciones de seguridad; responder a incidentes; cacería de amenazas |

> Datos revisados el 2026-11-09. Confírmalos en la página oficial antes de pagar.

Fuentes: [1](https://learn.microsoft.com/en-us/credentials/certifications/security-operations-analyst/) · [2](https://learn.microsoft.com/en-us/credentials/certifications/renew-your-microsoft-certification-faq)

## Dónde y cómo hacerla

1. Inicia sesión en Microsoft Learn con una cuenta personal (MSA), no la del trabajo: si dejas la empresa pierdes el historial.
2. En la página de la certificación elige **Programar examen** → Pearson VUE (centro de examen o en línea con OnVUE).
3. Revisa el precio que te muestra para México, paga con tarjeta o voucher y elige idioma (si no está en tu idioma puedes pedir 30 min extra).
4. Si repruebas puedes reintentar a las 24 h; los siguientes intentos tienen esperas mayores.

## ¿Cuándo estoy listo?

- [ ] Todas las notas de 12-Seguridad/05 Herramientas defensivas en `visto` o más.
- [ ] Todas las notas de 07-Azure/06 Seguridad en `visto` o más.
- [ ] Escribo consultas KQL de cacería (logins fallidos, procesos raros) y explico el flujo de un incidente en Defender XDR y Sentinel.
- [ ] 85 % o más en dos evaluaciones de práctica oficiales de Microsoft seguidas.
- [ ] Repasé el temario oficial (study guide) punto por punto y cada punto tiene una nota mía.

Cuando todo esté marcado, cambia `estado_cert` a **lista** y pídeme una revisión: reviso tu avance en la bóveda y te digo qué reforzar.

## Mi avance en los temas

```base
filters:
  and:
    - or:
        - file.inFolder("12-Seguridad/05 Herramientas defensivas")
        - file.inFolder("07-Azure/06 Seguridad")
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
> 2. Guárdalos en la carpeta `Certificados` junto a esta nota como `SC-200 Security Operations Analyst - certificado.pdf`.
> 3. Quita el `%%` de la línea de abajo para verlo aquí.
> 4. Llena `fecha_obtenida`, `vence`, `credencial_id` y `enlace_verificacion`.

%% ![[SC-200 Security Operations Analyst - certificado.pdf]] %%

## Renovación

Las certificaciones associate, expert y specialty de Microsoft vencen al año. La evaluación de renovación es gratis, en línea en Microsoft Learn, se abre 6 meses antes del vencimiento y se puede repetir (después del segundo intento, espera de 24 h).

- [ ] Cuando la obtengas, pon `vence` (1 año después) y un recordatorio 6 meses antes.
