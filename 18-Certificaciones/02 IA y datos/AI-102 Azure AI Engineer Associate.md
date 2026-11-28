---
proyecto: Conocimiento
tipo: certificacion
proveedor: Microsoft
codigo: "AI-102 (retirado) / AI-103"
estado_cert: pendiente
fecha_examen:
fecha_obtenida:
vence:
credencial_id:
enlace_verificacion:
costo_usd: 
vigencia: "1 año (renovación gratuita en línea)"
verificado_el: 2026-11-09
area: IA
tags: [certificacion, certificaciones]
contenido: completo
---
# AI-102 Azure AI Engineer Associate

Parte de: [[18-Certificaciones/02 IA y datos/_Índice IA y datos|IA y datos]] · Paso previo opcional: [[AI-900 Azure AI Fundamentals]]

> [!info] Estado de la certificación (`estado_cert`)
> **pendiente** → **preparando** → **lista** (cumples todos los criterios de abajo) → **agendada** (pon `fecha_examen`) → **obtenida** (llena fecha, `vence`, `credencial_id`, `enlace_verificacion` y adjunta el certificado) → **vencida** si no renuevas.

## Datos del examen

| Dato | Valor |
|---|---|
| Nombre | Microsoft Certified: Azure AI Engineer Associate (retirada) → Azure AI Apps and Agents Developer Associate |
| Código | AI-102 (retirado) / AI-103 |
| Duración | AI-103: 120 minutos |
| Precio | Varía por país o región (más impuestos); el precio exacto aparece al agendar en Pearson VUE |
| Idiomas | AI-103: 10 idiomas, incluido español |
| Requisitos | Ninguno formal (AI-103 recomienda Python y conocer IA generativa y servicios de Azure) |
| Dónde | Pearson VUE: centro de examen o en línea (OnVUE) |
| Vigencia | 1 año (renovación gratuita en línea) |
| Estado AI-102 | **AI-102 se retiró el 30 de junio de 2026** (lista oficial de exámenes retirados). Ya no se puede presentar. |
| Qué presentar hoy | **AI-103 – Developing AI Apps and Agents on Azure** (certificación *Azure AI Apps and Agents Developer Associate*). La página oficial no lo llama sucesor de AI-102. |
| Temario AI-103 | Planear y administrar una solución de IA; IA generativa y agentes; visión; análisis de texto; extracción de información |

> Datos revisados el 2026-11-09. Confírmalos en la página oficial antes de pagar.

Fuentes: [1](https://learn.microsoft.com/en-us/credentials/certifications/azure-ai-apps-and-agents-developer-associate/) · [2](https://learn.microsoft.com/en-us/credentials/support/retired-certification-exams) · [3](https://learn.microsoft.com/en-us/credentials/certifications/resources/study-guides/ai-103) · [4](https://learn.microsoft.com/en-us/credentials/certifications/renew-your-microsoft-certification-faq)

## Dónde y cómo hacerla

1. Inicia sesión en Microsoft Learn con una cuenta personal (MSA), no la del trabajo: si dejas la empresa pierdes el historial.
2. En la página de la certificación elige **Programar examen** → Pearson VUE (centro de examen o en línea con OnVUE).
3. Revisa el precio que te muestra para México, paga con tarjeta o voucher y elige idioma (si no está en tu idioma puedes pedir 30 min extra).
4. Si repruebas puedes reintentar a las 24 h; los siguientes intentos tienen esperas mayores.

## ¿Cuándo estoy listo?

- [ ] Todas las notas de 21-IA-Machine-Learning (Modelos de lenguaje, Agentes de IA, IA en .NET y Azure) en `visto` o más.
- [ ] Construyo en Python un agente con herramientas y una solución RAG en Microsoft Foundry.
- [ ] Uso servicios de visión, análisis de texto y extracción de información (Document Intelligence) desde código.
- [ ] 85 % o más en dos evaluaciones de práctica oficiales de Microsoft seguidas.
- [ ] Repasé el temario oficial (study guide) punto por punto y cada punto tiene una nota mía.

Cuando todo esté marcado, cambia `estado_cert` a **lista** y pídeme una revisión: reviso tu avance en la bóveda y te digo qué reforzar.

## Mi avance en los temas

```base
filters:
  and:
    - or:
        - file.inFolder("21-IA-Machine-Learning/05 Modelos de lenguaje")
        - file.inFolder("21-IA-Machine-Learning/06 Agentes de IA")
        - file.inFolder("21-IA-Machine-Learning/07 IA en .NET y Azure")
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
> 2. Guárdalos en la carpeta `Certificados` junto a esta nota como `AI-102 Azure AI Engineer Associate - certificado.pdf`.
> 3. Quita el `%%` de la línea de abajo para verlo aquí.
> 4. Llena `fecha_obtenida`, `vence`, `credencial_id` y `enlace_verificacion`.

%% ![[AI-102 Azure AI Engineer Associate - certificado.pdf]] %%

## Renovación

Las certificaciones associate, expert y specialty de Microsoft vencen al año. La evaluación de renovación es gratis, en línea en Microsoft Learn, se abre 6 meses antes del vencimiento y se puede repetir (después del segundo intento, espera de 24 h).

- [ ] Cuando la obtengas, pon `vence` (1 año después) y un recordatorio 6 meses antes.
