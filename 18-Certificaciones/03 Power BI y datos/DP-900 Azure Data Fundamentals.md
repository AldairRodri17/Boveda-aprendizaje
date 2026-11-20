---
proyecto: Conocimiento
tipo: certificacion
proveedor: Microsoft
codigo: "DP-900"
estado_cert: pendiente
fecha_examen:
fecha_obtenida:
vence:
credencial_id:
enlace_verificacion:
costo_usd: 
vigencia: "No vence (Fundamentals)"
verificado_el: 2026-11-09
area: Datos
tags: [certificacion, certificaciones]
---
# DP-900 Azure Data Fundamentals

Parte de: [[18-Certificaciones/03 Power BI y datos/_Índice Power BI y datos|Power BI y datos]] · Siguiente: [[DP-300 Azure Database Administrator]] o [[PL-300 Power BI Data Analyst Associate]]

> [!info] Estado de la certificación (`estado_cert`)
> **pendiente** → **preparando** → **lista** (cumples todos los criterios de abajo) → **agendada** (pon `fecha_examen`) → **obtenida** (llena fecha, `vence`, `credencial_id`, `enlace_verificacion` y adjunta el certificado) → **vencida** si no renuevas.

## Datos del examen

| Dato | Valor |
|---|---|
| Nombre | Microsoft Certified: Azure Data Fundamentals |
| Código | DP-900 |
| Duración | 45 minutos |
| Precio | Varía por país o región (más impuestos); el precio exacto aparece al agendar en Pearson VUE |
| Idiomas | 13 idiomas, incluido español |
| Requisitos | Ninguno |
| Dónde | Pearson VUE: centro de examen o en línea (OnVUE) |
| Vigencia | No vence (Fundamentals) |

> Datos revisados el 2026-11-09. Confírmalos en la página oficial antes de pagar.

Fuentes: [1](https://learn.microsoft.com/en-us/credentials/certifications/azure-data-fundamentals/) · [2](https://learn.microsoft.com/en-us/credentials/support/exam-duration-exam-experience) · [3](https://learn.microsoft.com/en-us/credentials/certifications/renew-your-microsoft-certification-faq)

## Dónde y cómo hacerla

1. Inicia sesión en Microsoft Learn con una cuenta personal (MSA), no la del trabajo: si dejas la empresa pierdes el historial.
2. En la página de la certificación elige **Programar examen** → Pearson VUE (centro de examen o en línea con OnVUE).
3. Revisa el precio que te muestra para México, paga con tarjeta o voucher y elige idioma (si no está en tu idioma puedes pedir 30 min extra).
4. Si repruebas puedes reintentar a las 24 h; los siguientes intentos tienen esperas mayores.

## ¿Cuándo estoy listo?

- [ ] Todas las notas de 06-SQL-Server/01 Básico en `visto` o más.
- [ ] Todas las notas de 07-Azure/04 Datos en `visto` o más.
- [ ] Explico diferencias entre datos relacionales y no relacionales, OLTP y OLAP, y cuándo usar Azure SQL, Cosmos DB o Fabric.
- [ ] 85 % o más en dos evaluaciones de práctica oficiales de Microsoft seguidas.
- [ ] Repasé el temario oficial (study guide) punto por punto y cada punto tiene una nota mía.

Cuando todo esté marcado, cambia `estado_cert` a **lista** y pídeme una revisión: reviso tu avance en la bóveda y te digo qué reforzar.

## Mi avance en los temas

```base
filters:
  and:
    - or:
        - file.inFolder("06-SQL-Server/01 Básico")
        - file.inFolder("07-Azure/04 Datos")
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
> 2. Guárdalos en la carpeta `Certificados` junto a esta nota como `DP-900 Azure Data Fundamentals - certificado.pdf`.
> 3. Quita el `%%` de la línea de abajo para verlo aquí.
> 4. Llena `fecha_obtenida`, `vence`, `credencial_id` y `enlace_verificacion`.

%% ![[DP-900 Azure Data Fundamentals - certificado.pdf]] %%

## Renovación

Las certificaciones Fundamentals de Microsoft no vencen.
