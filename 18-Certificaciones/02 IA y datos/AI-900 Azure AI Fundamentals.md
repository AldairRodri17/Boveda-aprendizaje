---
proyecto: Conocimiento
tipo: certificacion
proveedor: Microsoft
codigo: "AI-901 (antes AI-900)"
estado_cert: pendiente
fecha_examen:
fecha_obtenida:
vence:
credencial_id:
enlace_verificacion:
costo_usd: 
vigencia: "No vence (Fundamentals)"
verificado_el: 2026-11-09
area: IA
tags: [certificacion, certificaciones]
contenido: completo
---
# AI-900 Azure AI Fundamentals

Parte de: [[18-Certificaciones/02 IA y datos/_Índice IA y datos|IA y datos]] · Siguiente: [[AI-102 Azure AI Engineer Associate]] (hoy AI-103) o [[AI-200 Azure AI Cloud Developer Associate (sucesor de AZ-204)]]

> [!info] Estado de la certificación (`estado_cert`)
> **pendiente** → **preparando** → **lista** (cumples todos los criterios de abajo) → **agendada** (pon `fecha_examen`) → **obtenida** (llena fecha, `vence`, `credencial_id`, `enlace_verificacion` y adjunta el certificado) → **vencida** si no renuevas.

## Datos del examen

| Dato | Valor |
|---|---|
| Nombre | Microsoft Certified: Azure AI Fundamentals |
| Código | AI-901 (antes AI-900) |
| Duración | 45 minutos (Fundamentals) |
| Precio | Varía por país o región (más impuestos); el precio exacto aparece al agendar en Pearson VUE |
| Idiomas | 13 idiomas, incluido español |
| Requisitos | Ninguno (recomendado: algo de Python y conocer recursos de Azure) |
| Dónde | Pearson VUE: centro de examen o en línea (OnVUE) |
| Vigencia | No vence (Fundamentals) |
| Cambio de examen | **El examen AI-900 se retiró el 30 de junio de 2026.** La certificación sigue con el mismo nombre, pero ahora se obtiene con **AI-901** (temario actualizado a Microsoft Foundry). |
| Temario AI-901 | Conceptos y capacidades de IA; implementar soluciones de IA con Microsoft Foundry |

> Datos revisados el 2026-11-09. Confírmalos en la página oficial antes de pagar.

Fuentes: [1](https://learn.microsoft.com/en-us/credentials/certifications/azure-ai-fundamentals/) · [2](https://learn.microsoft.com/en-us/credentials/support/retired-certification-exams) · [3](https://learn.microsoft.com/en-us/credentials/support/exam-duration-exam-experience) · [4](https://learn.microsoft.com/en-us/credentials/certifications/renew-your-microsoft-certification-faq)

## Dónde y cómo hacerla

1. Inicia sesión en Microsoft Learn con una cuenta personal (MSA), no la del trabajo: si dejas la empresa pierdes el historial.
2. En la página de la certificación elige **Programar examen** → Pearson VUE (centro de examen o en línea con OnVUE).
3. Revisa el precio que te muestra para México, paga con tarjeta o voucher y elige idioma (si no está en tu idioma puedes pedir 30 min extra).
4. Si repruebas puedes reintentar a las 24 h; los siguientes intentos tienen esperas mayores.

## ¿Cuándo estoy listo?

- [ ] Todas las notas de 21-IA-Machine-Learning (ML clásico, Modelos de lenguaje, IA en .NET y Azure) en `visto` o más.
- [ ] Creo un proyecto en Microsoft Foundry y pruebo un modelo de chat y un servicio de visión o de lenguaje.
- [ ] 85 % o más en dos evaluaciones de práctica oficiales de Microsoft seguidas.
- [ ] Repasé el temario oficial (study guide) punto por punto y cada punto tiene una nota mía.

Cuando todo esté marcado, cambia `estado_cert` a **lista** y pídeme una revisión: reviso tu avance en la bóveda y te digo qué reforzar.

## Mi avance en los temas

```base
filters:
  and:
    - or:
        - file.inFolder("21-IA-Machine-Learning/03 Machine learning clásico")
        - file.inFolder("21-IA-Machine-Learning/05 Modelos de lenguaje")
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
> 2. Guárdalos en la carpeta `Certificados` junto a esta nota como `AI-900 Azure AI Fundamentals - certificado.pdf`.
> 3. Quita el `%%` de la línea de abajo para verlo aquí.
> 4. Llena `fecha_obtenida`, `vence`, `credencial_id` y `enlace_verificacion`.

%% ![[AI-900 Azure AI Fundamentals - certificado.pdf]] %%

## Renovación

Las certificaciones Fundamentals de Microsoft no vencen.
