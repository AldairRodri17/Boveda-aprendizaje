---
proyecto: Conocimiento
tipo: certificacion
proveedor: EC-Council
codigo: "CEH v13 (312-50, Pendiente de confirmar)"
estado_cert: pendiente
fecha_examen:
fecha_obtenida:
vence:
credencial_id:
enlace_verificacion:
costo_usd: 
vigencia: "Pendiente de confirmar (requiere créditos de educación continua)"
verificado_el: 2026-11-09
area: Seguridad
tags: [certificacion, certificaciones]
contenido: completo
---
# CEH

Parte de: [[18-Certificaciones/06 Seguridad/_Índice Seguridad|Seguridad]] · Alternativa práctica: [[eJPT]] u [[OSCP]]

> [!info] Estado de la certificación (`estado_cert`)
> **pendiente** → **preparando** → **lista** (cumples todos los criterios de abajo) → **agendada** (pon `fecha_examen`) → **obtenida** (llena fecha, `vence`, `credencial_id`, `enlace_verificacion` y adjunta el certificado) → **vencida** si no renuevas.

## Datos del examen

| Dato | Valor |
|---|---|
| Nombre | Certified Ethical Hacker (CEH) v13 |
| Código | 312-50 (Pendiente de confirmar: la página oficial no lo muestra) |
| Duración | Examen de conocimiento: 125 preguntas, 4 horas · Práctico opcional (CEH Master): 20 retos, 6 horas |
| Aprobación | Entre 60 % y 85 % según la forma del examen |
| Precio | Desde 1,699 USD con curso en línea a tu ritmo (paquetes más caros con clase en vivo); si no tomas curso oficial: solicitud de elegibilidad de 100 USD más el examen (Pendiente de confirmar el precio del examen solo) |
| Idiomas | Pendiente de confirmar |
| Requisitos | Curso oficial o centro autorizado, o 2 años de experiencia en seguridad y solicitud aprobada |
| Dónde | En línea por el portal de exámenes de EC-Council |
| Vigencia | Pendiente de confirmar (política de CPE/ECE de EC-Council) |

> Datos revisados el 2026-11-09. Confírmalos en la página oficial antes de pagar.

Fuentes: [1](https://www.eccouncil.org/train-certify/certified-ethical-hacker-ceh/)

## Dónde y cómo hacerla

1. Decide si tomarás el curso oficial (te hace elegible) o pedirás elegibilidad por experiencia.
2. Compra el paquete y agenda en el portal de EC-Council.

## ¿Cuándo estoy listo?

- [ ] Todas las notas de 12-Seguridad en `visto` o más.
- [ ] Domino nmap, Wireshark, Metasploit, Burp y conceptos de malware, ingeniería social, wireless y nube a nivel de definición.
- [ ] 85 % o más en dos exámenes de práctica seguidos (fuentes oficiales o reconocidas; nunca *dumps*).
- [ ] Repasé el temario oficial punto por punto y cada punto tiene una nota mía.

Cuando todo esté marcado, cambia `estado_cert` a **lista** y pídeme una revisión: reviso tu avance en la bóveda y te digo qué reforzar.

## Mi avance en los temas

```base
filters:
  and:
    - or:
        - file.inFolder("12-Seguridad")
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
> 2. Guárdalos en la carpeta `Certificados` junto a esta nota como `CEH - certificado.pdf`.
> 3. Quita el `%%` de la línea de abajo para verlo aquí.
> 4. Llena `fecha_obtenida`, `vence`, `credencial_id` y `enlace_verificacion`.

%% ![[CEH - certificado.pdf]] %%

## Renovación

- [ ] Revisa la política de créditos de educación continua de EC-Council al obtenerla y pon `vence`.
