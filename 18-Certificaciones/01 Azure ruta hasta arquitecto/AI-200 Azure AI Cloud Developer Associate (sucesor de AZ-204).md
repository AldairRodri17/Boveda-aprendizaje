---
proyecto: Conocimiento
tipo: certificacion
proveedor: Microsoft
codigo: "AI-200"
estado_cert: pendiente
fecha_examen:
fecha_obtenida:
vence:
credencial_id:
enlace_verificacion:
costo_usd: 
vigencia: "1 año (renovación gratuita en línea)"
verificado_el: 2026-11-09
area: Azure
tags: [certificacion, certificaciones]
contenido: completo
---
# AI-200 Azure AI Cloud Developer Associate (sucesor de AZ-204)

Parte de: [[18-Certificaciones/01 Azure ruta hasta arquitecto/_Índice Azure ruta hasta arquitecto|Azure ruta hasta arquitecto]] · Reemplaza en la práctica a: [[AZ-204 Azure Developer Associate (retirado el 31 de julio de 2026)]] · Siguiente: [[AZ-305 Azure Solutions Architect Expert]]

> [!info] Estado de la certificación (`estado_cert`)
> **pendiente** → **preparando** → **lista** (cumples todos los criterios de abajo) → **agendada** (pon `fecha_examen`) → **obtenida** (llena fecha, `vence`, `credencial_id`, `enlace_verificacion` y adjunta el certificado) → **vencida** si no renuevas.

## Datos del examen

| Dato | Valor |
|---|---|
| Nombre | Microsoft Certified: Azure AI Cloud Developer Associate |
| Código | AI-200 |
| Duración | 120 minutos (puede incluir elementos interactivos) |
| Precio | Varía por país o región (más impuestos); el precio exacto aparece al agendar en Pearson VUE |
| Idiomas | 13 idiomas, incluido español |
| Requisitos | Ninguno formal |
| Dónde | Pearson VUE: centro de examen o en línea (OnVUE) |
| Vigencia | 1 año (renovación gratuita en línea) |
| Temario | Contenedores (20–25 %), soluciones de IA con servicios de datos: Cosmos DB, PostgreSQL/pgvector, Managed Redis (25–30 %), consumir servicios: Service Bus, Event Grid, Functions (20–25 %), seguridad y monitoreo: Key Vault, App Configuration, OpenTelemetry/KQL (20–25 %) |
| Calificación | 700 o más |
| ¿Sucesor de AZ-204? | La página oficial de la certificación **no** lo dice. Microsoft Q&A y la comunidad lo tratan como el reemplazo (cubre terreno parecido con más IA). Pendiente de confirmar si cuenta como requisito de AZ-400. |

> Datos revisados el 2026-11-09. Confírmalos en la página oficial antes de pagar.

Fuentes: [1](https://learn.microsoft.com/en-us/credentials/certifications/resources/study-guides/ai-200) · [2](https://learn.microsoft.com/nl-nl/credentials/certifications/azure-ai-cloud-developer-associate/) · [3](https://learn.microsoft.com/en-us/credentials/certifications/renew-your-microsoft-certification-faq)

## Dónde y cómo hacerla

1. Inicia sesión en Microsoft Learn con una cuenta personal (MSA), no la del trabajo: si dejas la empresa pierdes el historial.
2. En la página de la certificación elige **Programar examen** → Pearson VUE (centro de examen o en línea con OnVUE).
3. Revisa el precio que te muestra para México, paga con tarjeta o voucher y elige idioma (si no está en tu idioma puedes pedir 30 min extra).
4. Si repruebas puedes reintentar a las 24 h; los siguientes intentos tienen esperas mayores.

## ¿Cuándo estoy listo?

- [ ] Todas las notas de 07-Azure (Cómputo, Datos, Integración, Seguridad, Observabilidad) en `visto` o más.
- [ ] Todas las notas de 21-IA-Machine-Learning (Modelos de lenguaje, IA en .NET y Azure) en `visto` o más.
- [ ] Despliego una app en contenedor a Azure Container Apps desde Azure Container Registry, con escalado KEDA.
- [ ] Hago una búsqueda vectorial (RAG) con Cosmos DB for NoSQL o PostgreSQL con pgvector.
- [ ] Conecto una Azure Function con Service Bus o Event Grid y leo secretos desde Key Vault con identidad administrada.
- [ ] Consulto trazas OpenTelemetry con KQL en Application Insights.
- [ ] 85 % o más en dos evaluaciones de práctica oficiales de Microsoft seguidas.
- [ ] Repasé el temario oficial (study guide) punto por punto y cada punto tiene una nota mía.

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
        - file.inFolder("21-IA-Machine-Learning/05 Modelos de lenguaje")
        - file.inFolder("21-IA-Machine-Learning/07 IA en .NET y Azure")
        - file.inFolder("03-CSharp-DotNet/05 ASP.NET Core")
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
> 2. Guárdalos en la carpeta `Certificados` junto a esta nota como `AI-200 Azure AI Cloud Developer Associate (sucesor de AZ-204) - certificado.pdf`.
> 3. Quita el `%%` de la línea de abajo para verlo aquí.
> 4. Llena `fecha_obtenida`, `vence`, `credencial_id` y `enlace_verificacion`.

%% ![[AI-200 Azure AI Cloud Developer Associate (sucesor de AZ-204) - certificado.pdf]] %%

## Renovación

Las certificaciones associate, expert y specialty de Microsoft vencen al año. La evaluación de renovación es gratis, en línea en Microsoft Learn, se abre 6 meses antes del vencimiento y se puede repetir (después del segundo intento, espera de 24 h).

- [ ] Cuando la obtengas, pon `vence` (1 año después) y un recordatorio 6 meses antes.
