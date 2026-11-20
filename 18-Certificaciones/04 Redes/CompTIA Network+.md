---
proyecto: Conocimiento
tipo: certificacion
proveedor: CompTIA
codigo: "N10-009"
estado_cert: pendiente
fecha_examen:
fecha_obtenida:
vence:
credencial_id:
enlace_verificacion:
costo_usd: 
vigencia: "3 años (educación continua)"
verificado_el: 2026-11-09
area: Redes
tags: [certificacion, certificaciones]
---
# CompTIA Network+

Parte de: [[18-Certificaciones/04 Redes/_Índice Redes|Redes]] · Alternativa neutral a: [[CCNA]]

> [!info] Estado de la certificación (`estado_cert`)
> **pendiente** → **preparando** → **lista** (cumples todos los criterios de abajo) → **agendada** (pon `fecha_examen`) → **obtenida** (llena fecha, `vence`, `credencial_id`, `enlace_verificacion` y adjunta el certificado) → **vencida** si no renuevas.

## Datos del examen

| Dato | Valor |
|---|---|
| Nombre | CompTIA Network+ (V9) |
| Código | N10-009 (lanzado el 20 de junio de 2024; retiro estimado en 2027) |
| Duración | 90 minutos |
| Preguntas | Máximo 90, opción múltiple y de desempeño (performance-based) |
| Aprobación | 720 en escala de 100 a 900 |
| Precio | Pendiente de confirmar (consúltalo en store.comptia.org con tu país; varía por región; hay descuento para estudiantes) |
| Idiomas | Inglés, alemán, japonés, portugués y español |
| Requisitos | Ninguno formal (recomendado: A+ y 9–12 meses como técnico de redes) |
| Dónde | Pearson VUE (centro u OnVUE) |
| Vigencia | 3 años; se renueva con educación continua (CE) y cuota anual |

> Datos revisados el 2026-11-09. Confírmalos en la página oficial antes de pagar.

Fuentes: [1](https://www.comptia.org/en-us/certifications/network/) · [2](https://www.comptia.org/en-us/resources/ce/learn/how-to-renew/ce-renewal-cycle/)

## Dónde y cómo hacerla

1. Crea tu cuenta del proveedor y vincúlala con Pearson VUE.
2. Elige centro de examen u OnVUE en línea, fecha e idioma.
3. Paga con tarjeta o voucher comprado solo al proveedor o a Pearson VUE.
4. Revisa políticas de reprogramación y reintento al agendar.

## ¿Cuándo estoy listo?

- [ ] Todas las notas de [[19-Redes/_Índice Redes|19-Redes]] (excepto 09 Avanzado) en `visto` o más.
- [ ] Hago subnetting sin calculadora y explico OSI/TCP-IP, DNS, DHCP, VLANs, inalámbrico y troubleshooting con comandos (ping, tracert, nslookup).
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
> 2. Guárdalos en la carpeta `Certificados` junto a esta nota como `CompTIA Network+ - certificado.pdf`.
> 3. Quita el `%%` de la línea de abajo para verlo aquí.
> 4. Llena `fecha_obtenida`, `vence`, `credencial_id` y `enlace_verificacion`.

%% ![[CompTIA Network+ - certificado.pdf]] %%

## Renovación

- [ ] Vence a los 3 años: junta créditos CE (o una certificación superior de CompTIA) y paga la cuota anual. Pon `vence`.
