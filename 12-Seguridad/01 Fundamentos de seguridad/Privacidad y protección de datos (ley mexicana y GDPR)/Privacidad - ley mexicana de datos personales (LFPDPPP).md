---
proyecto: Conocimiento
tipo: concepto
nivel: basico
progreso: pendiente
estado: pendiente
area: Seguridad
tags: [seguridad]
contenido: completo
---
# Privacidad - ley mexicana de datos personales (LFPDPPP)

Parte de: [[Privacidad y protección de datos (ley mexicana y GDPR)]]

> [!info] Cómo avanzar
> `progreso`: **pendiente** → **viendo** (estudiando) → **visto** (contestaste la autoevaluación sin abrir las respuestas) → **dominado** (hiciste el laboratorio o lo aplicaste de verdad).

## Contenido

### Qué es
La **LFPDPPP** (Ley Federal de Protección de Datos Personales en Posesión de los Particulares) regula cómo **empresas y personas particulares** en México recaban, usan, guardan, transfieren y eliminan datos personales. El sector público tiene su propia ley general; si trabajas para una empresa privada, la que te toca es esta.

Sus piezas principales son: el **aviso de privacidad**, el **consentimiento**, los **derechos ARCO** y la obligación de mantener **medidas de seguridad** administrativas, técnicas y físicas.

### Cómo funciona
**Principios**: licitud, consentimiento, información, calidad, finalidad, lealtad, proporcionalidad y responsabilidad. En términos de desarrollo: recaba solo lo necesario, úsalo solo para lo que avisaste, mantenlo exacto y bórralo cuando ya no sirva.

**Aviso de privacidad**: documento que informa quién es el responsable, qué datos se recaban, para qué finalidades, a quién se transfieren y cómo ejercer derechos. Tu formulario de registro debe enlazarlo y, cuando aplique, obtener el consentimiento.

**Datos sensibles**: los que afectan la esfera más íntima o cuyo mal uso puede causar discriminación o riesgo grave (salud, origen étnico, creencias religiosas, preferencia sexual, datos genéticos, opiniones políticas, entre otros). Requieren consentimiento **expreso** (la ley pide más formalidad que para datos ordinarios; confirma los requisitos exactos en el texto vigente) y mayor protección.

**Derechos ARCO**:
- **Acceso**: conocer qué datos tienes de la persona y cómo los usas.
- **Rectificación**: corregir datos inexactos o incompletos.
- **Cancelación**: pedir que se eliminen (normalmente tras un periodo de bloqueo cuando hay obligaciones legales).
- **Oposición**: oponerse a ciertos usos, como mercadotecnia.

**Cambio de 2025**: el 20 de marzo de 2025 se publicó en el DOF una **nueva LFPDPPP**, vigente desde el 21 de marzo de 2025, como parte de la reforma que extinguió al INAI. Las funciones de autoridad en datos personales de particulares pasaron a la **Secretaría Anticorrupción y Buen Gobierno**. Entre los cambios que reportan los análisis de despachos: se pide nuevo consentimiento para finalidades no previstas en el aviso original (ya no basta que sean "compatibles"), las transferencias fuera de las excepciones legales requieren consentimiento y se refuerzan los deberes de confidencialidad. El reglamento correspondiente y los lineamientos de la nueva autoridad deben revisarse en el DOF (Pendiente de confirmar).

### Ejemplo
Requisitos técnicos que salen de la ley para un registro de clientes en Itzamna.Clio y Dionisio:

```sql
-- Guarda cuándo y con qué versión del aviso consintió el titular
CREATE TABLE app.Consentimientos (
    ClienteId     INT           NOT NULL,
    Finalidad     VARCHAR(50)   NOT NULL,   -- 'servicio', 'mercadotecnia'
    VersionAviso  VARCHAR(20)   NOT NULL,   -- '2026-03'
    Otorgado      BIT           NOT NULL,
    Fecha         DATETIME2(0)  NOT NULL DEFAULT SYSUTCDATETIME(),
    CONSTRAINT PK_Consentimientos PRIMARY KEY (ClienteId, Finalidad, Fecha)
);
```

- Casilla de mercadotecnia **separada** y desmarcada por defecto.
- Endpoint para atender solicitudes ARCO (exportar datos del titular, corregir, bloquear y luego cancelar).
- Medidas de seguridad: cifrado en tránsito y en reposo, control de acceso por rol, bitácora de accesos a datos sensibles. Ver [[Cifrado en reposo y en tránsito]].
- Plan de respuesta a vulneraciones: a quién avisar y cómo informar a los titulares.

### Cuándo usarlo y cuándo no
Aplica a casi cualquier sistema mexicano que maneje datos de clientes, empleados o prospectos. Si además hay titulares en la UE, revisa [[Privacidad - GDPR para desarrolladores]]. La interpretación legal le toca al área jurídica; tu papel es que el sistema **pueda cumplir**: inventario, consentimiento registrado, ARCO atendible y seguridad.

### Errores comunes
- **Copiar un aviso de privacidad genérico** que no corresponde a lo que el sistema realmente hace.
- **Usar datos para una finalidad nueva** (por ejemplo, entrenar un modelo o hacer campañas) sin revisar el aviso y el consentimiento.
- **No poder responder una solicitud ARCO** porque los datos están dispersos en hojas de Excel, logs y correos.
- **Seguir citando al INAI** como autoridad vigente en documentos nuevos.

### Para profundizar
- [Texto vigente de la LFPDPPP (Cámara de Diputados)](https://www.diputados.gob.mx/LeyesBiblio/pdf/LFPDPPP.pdf)
- [Diario Oficial de la Federación](https://www.dof.gob.mx/)

## Autoevaluación

Contesta primero en voz alta o en papel; luego despliega la respuesta.

> [!question]- ¿Qué regula la LFPDPPP?
> El tratamiento de datos personales por particulares (empresas y personas) en México: aviso de privacidad, consentimiento, derechos ARCO y medidas de seguridad.

> [!question]- ¿Qué son los derechos ARCO?
> **A**cceso, **R**ectificación, **C**ancelación y **O**posición: el titular puede pedir ver, corregir, eliminar sus datos u oponerse a cierto uso.

> [!question]- ¿Qué cambió en 2025 en la ley mexicana?
> En 2025 se publicó una nueva LFPDPPP y desapareció el INAI; sus funciones pasaron a una dependencia del Ejecutivo federal. Confirma el texto vigente y la autoridad actual en el DOF (Pendiente de confirmar).

