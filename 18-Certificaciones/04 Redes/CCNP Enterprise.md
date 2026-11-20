---
proyecto: Conocimiento
tipo: certificacion
proveedor: Cisco
codigo: "350-401 ENCOR + 1 de concentración"
estado_cert: pendiente
fecha_examen:
fecha_obtenida:
vence:
credencial_id:
enlace_verificacion:
costo_usd: 
vigencia: "3 años"
verificado_el: 2026-11-09
area: Redes
tags: [certificacion, certificaciones]
---
# CCNP Enterprise

Parte de: [[18-Certificaciones/04 Redes/_Índice Redes|Redes]] · Paso previo: [[CCNA]]

> [!info] Estado de la certificación (`estado_cert`)
> **pendiente** → **preparando** → **lista** (cumples todos los criterios de abajo) → **agendada** (pon `fecha_examen`) → **obtenida** (llena fecha, `vence`, `credencial_id`, `enlace_verificacion` y adjunta el certificado) → **vencida** si no renuevas.

## Datos del examen

| Dato | Valor |
|---|---|
| Nombre | CCNP Enterprise |
| Código | 350-401 ENCOR (núcleo) + una concentración: 300-410 ENARSI, 300-415 ENSDWI, 300-420 ENSLD, 300-435 ENAUTO, 300-440 ENCC o 300-445 ENNA |
| Duración | ENCOR 120 min; concentraciones 90 min |
| Precio | ENCOR 400 USD; cada concentración 300 USD (más impuestos) |
| Idiomas | Inglés y japonés (ENNA solo inglés) |
| Requisitos | Ninguno formal |
| Dónde | Pearson VUE (centro u OnVUE) |
| Vigencia | 3 años; se renueva con examen o 80 créditos de educación continua (CE) |
| Cambios | Cisco anuncia actualizaciones de sus certificaciones enterprise; fechas y versiones Pendiente de confirmar en la página oficial |

> Datos revisados el 2026-11-09. Confírmalos en la página oficial antes de pagar.

Fuentes: [1](https://www.cisco.com/site/us/en/learn/training-certifications/certifications/enterprise/ccnp-enterprise/exams-and-training.html/) · [2](https://www.cisco.com/site/us/en/learn/training-certifications/exams/encor.html) · [3](https://www.cisco.com/c/dam/en_us/training-events/certifications/continuing-education-portal-guide.pdf)

## Dónde y cómo hacerla

1. Crea tu cuenta del proveedor y vincúlala con Pearson VUE.
2. Elige centro de examen u OnVUE en línea, fecha e idioma.
3. Paga con tarjeta o voucher comprado solo al proveedor o a Pearson VUE.
4. Revisa políticas de reprogramación y reintento al agendar.

## ¿Cuándo estoy listo?

- [ ] Tengo CCNA o domino su temario.
- [ ] Todas las notas de [[19-Redes/_Índice Redes|19-Redes]] en `dominado`, incluida la sección 09 Avanzado.
- [ ] Configuro sin apuntes OSPF multiárea, BGP básico, EtherChannel, HSRP y QoS básico en un laboratorio (Packet Tracer no basta para todo: usa CML o GNS3/EVE-NG).
- [ ] Hago automatización básica con Python o Ansible contra dispositivos (parte de ENCOR).
- [ ] 85 % o más en dos exámenes de práctica seguidos (fuentes oficiales o reconocidas; nunca *dumps*).
- [ ] Repasé el temario oficial punto por punto y cada punto tiene una nota mía.

Cuando todo esté marcado, cambia `estado_cert` a **lista** y pídeme una revisión: reviso tu avance en la bóveda y te digo qué reforzar.

## Mi avance en los temas

```base
filters:
  and:
    - or:
        - file.inFolder("19-Redes")
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
> 2. Guárdalos en la carpeta `Certificados` junto a esta nota como `CCNP Enterprise - certificado.pdf`.
> 3. Quita el `%%` de la línea de abajo para verlo aquí.
> 4. Llena `fecha_obtenida`, `vence`, `credencial_id` y `enlace_verificacion`.

%% ![[CCNP Enterprise - certificado.pdf]] %%

## Renovación

- [ ] Vence a los 3 años: renueva con un examen o 80 créditos CE. Pon `vence` y un recordatorio 6 meses antes.
