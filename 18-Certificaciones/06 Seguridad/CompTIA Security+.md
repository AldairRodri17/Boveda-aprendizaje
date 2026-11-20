---
proyecto: Conocimiento
tipo: certificacion
proveedor: CompTIA
codigo: "SY0-701 → SY0-801"
estado_cert: pendiente
fecha_examen:
fecha_obtenida:
vence:
credencial_id:
enlace_verificacion:
costo_usd: 
vigencia: "3 años (educación continua)"
verificado_el: 2026-11-09
area: Seguridad
tags: [certificacion, certificaciones]
---
# CompTIA Security+

Parte de: [[18-Certificaciones/06 Seguridad/_Índice Seguridad|Seguridad]] · Paso previo recomendado: [[CompTIA Network+]]

> [!info] Estado de la certificación (`estado_cert`)
> **pendiente** → **preparando** → **lista** (cumples todos los criterios de abajo) → **agendada** (pon `fecha_examen`) → **obtenida** (llena fecha, `vence`, `credencial_id`, `enlace_verificacion` y adjunta el certificado) → **vencida** si no renuevas.

## Datos del examen

| Dato | Valor |
|---|---|
| Nombre | CompTIA Security+ |
| Código | SY0-701 (V7) y la nueva SY0-801 (V8) a partir del 17 de noviembre de 2026 |
| Duración | 90 minutos |
| Preguntas | Máximo 90, opción múltiple y de desempeño |
| Aprobación | 750 en escala de 100 a 900 |
| Precio | Pendiente de confirmar (consúltalo en store.comptia.org con tu país) |
| Idiomas | V7: inglés, japonés, portugués, español y tailandés · V8: solo inglés al lanzamiento |
| Retiro de V7 | Inglés: 11 de junio de 2027; otros idiomas (incluido español): 13 de agosto de 2027 |
| Requisitos | Ninguno formal (recomendado: Network+ y 2 años en seguridad o sistemas) |
| Dónde | Pearson VUE (centro u OnVUE) |
| Vigencia | 3 años; se renueva con educación continua (CE) y cuota anual |

> Datos revisados el 2026-11-09. Confírmalos en la página oficial antes de pagar.

Fuentes: [1](https://www.comptia.org/en-us/certifications/security/v7/) · [2](https://www.comptia.org/en-us/certifications/security/v8/) · [3](https://www.comptia.org/en-us/resources/ce/learn/how-to-renew/ce-renewal-cycle/)

## Dónde y cómo hacerla

1. Crea tu cuenta del proveedor y vincúlala con Pearson VUE.
2. Elige centro de examen u OnVUE en línea, fecha e idioma.
3. Paga con tarjeta o voucher comprado solo al proveedor o a Pearson VUE.
4. Revisa políticas de reprogramación y reintento al agendar.
5. Decide versión: V7 en español hasta agosto de 2027, o V8 (en inglés) con temario nuevo.

## ¿Cuándo estoy listo?

- [ ] Todas las notas de 12-Seguridad/01 Fundamentos de seguridad y 05 Herramientas defensivas en `visto` o más.
- [ ] Todas las notas de 19-Redes/07 Seguridad de redes en `visto` o más.
- [ ] Estudio con el temario (exam objectives) de la versión exacta que voy a presentar.
- [ ] 85 % o más en dos exámenes de práctica seguidos (fuentes oficiales o reconocidas; nunca *dumps*).
- [ ] Repasé el temario oficial punto por punto y cada punto tiene una nota mía.

Cuando todo esté marcado, cambia `estado_cert` a **lista** y pídeme una revisión: reviso tu avance en la bóveda y te digo qué reforzar.

## Mi avance en los temas

```base
filters:
  and:
    - or:
        - file.inFolder("12-Seguridad")
        - file.inFolder("19-Redes/07 Seguridad de redes")
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
> 2. Guárdalos en la carpeta `Certificados` junto a esta nota como `CompTIA Security+ - certificado.pdf`.
> 3. Quita el `%%` de la línea de abajo para verlo aquí.
> 4. Llena `fecha_obtenida`, `vence`, `credencial_id` y `enlace_verificacion`.

%% ![[CompTIA Security+ - certificado.pdf]] %%

## Renovación

- [ ] Vence a los 3 años: junta créditos CE y paga la cuota anual. Pon `vence`.
