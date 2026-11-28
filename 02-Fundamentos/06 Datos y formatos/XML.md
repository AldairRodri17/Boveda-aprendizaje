---
proyecto: Conocimiento
tipo: concepto
nivel: basico
progreso: pendiente
estado: pendiente
area: Fundamentos
tags: [fundamentos]
contenido: completo
---
# XML

Parte de: [[02-Fundamentos/06 Datos y formatos/_Índice Datos y formatos|Datos y formatos]]

> [!info] Cómo avanzar
> `progreso`: **pendiente** → **viendo** (estudiando) → **visto** (contestaste la autoevaluación sin abrir las respuestas) → **dominado** (hiciste el laboratorio o lo aplicaste de verdad).

## Contenido

### Qué es
**XML** (eXtensible Markup Language) es un formato de texto para documentos y datos estructurados en forma de **árbol de elementos** con etiquetas de apertura y cierre. Es más verboso que JSON, pero tiene capacidades que JSON no trae de serie: **namespaces** para mezclar vocabularios, **validación por esquema** (XSD), **firmas digitales** estándar (XML-DSig) y consultas con **XPath**. En México lo encontrarás sobre todo en el **CFDI** (factura electrónica del SAT), en servicios SOAP, en configuración heredada (`web.config`, `.csproj`) y en integraciones bancarias.

### Cómo funciona
Un documento XML:

```xml
<?xml version="1.0" encoding="UTF-8"?>
<pedido id="P-1" moneda="MXN">
  <cliente rfc="XAXX010101000">
    <nombre>Ana Pérez</nombre>
  </cliente>
  <partidas>
    <partida sku="A1" cantidad="2"/>
    <partida sku="B7" cantidad="1"/>
  </partidas>
  <notas><![CDATA[Entregar "antes" de las 5 & llamar]]></notas>
</pedido>
```

Reglas básicas: un solo elemento raíz, etiquetas bien anidadas y cerradas, sensible a mayúsculas, atributos entre comillas, caracteres especiales escapados (`&lt;`, `&amp;`) o dentro de `CDATA`.

**Elementos vs atributos:**

| | Elementos | Atributos |
|---|---|---|
| Sintaxis | `<nombre>Ana</nombre>` | `<cliente rfc="...">` |
| Pueden anidar otros | Sí | No, solo texto |
| Pueden repetirse | Sí (`<partida>` varias veces) | No, uno por nombre en cada elemento |
| Uso típico | Contenido, estructuras, listas | Metadatos, identificadores, valores simples |

No hay una regla única; los estándares eligen. El CFDI, por ejemplo, usa muchísimos atributos (`Total="1432.09"`).

**Namespaces.** Cuando un documento combina vocabularios de distintos dueños, los nombres pueden chocar (`<Concepto>` del SAT y `<Concepto>` de tu complemento). Un namespace es un URI que califica los nombres:

```xml
<cfdi:Comprobante xmlns:cfdi="http://www.sat.gob.mx/cfd/4" Version="4.0" Total="1432.09">
  <cfdi:Emisor Rfc="EKU9003173C9" Nombre="ESCUELA KEMPER URGATE"/>
</cfdi:Comprobante>
```

El prefijo (`cfdi`) es solo un alias; lo que identifica al elemento es el **URI** más el nombre local. Al consultar con XPath o LINQ to XML **hay que usar el namespace**, o no encuentras nada: buscar `Comprobante` sin namespace devuelve vacío.

**XXE (XML External Entity).** XML permite declarar entidades en un DTD, incluidas **externas**:

```xml
<!DOCTYPE x [ <!ENTITY secreto SYSTEM "file:///etc/passwd"> ]>
<pedido><nota>&secreto;</nota></pedido>
```

Un parser que resuelve entidades externas lee el archivo del servidor y lo incluye en el documento (que quizá tu API devuelve o registra), o hace peticiones a URLs internas (SSRF). También existe el ataque de **expansión de entidades** ("billion laughs") que agota memoria. Se evita **deshabilitando DTD y entidades externas**. En .NET moderno, `XmlReader.Create` usa por defecto `DtdProcessing.Prohibit` y `XmlDocument`/`XDocument` no resuelven recursos externos por defecto; el riesgo aparece si alguien cambia esas opciones o usa código heredado de .NET Framework.

### Ejemplo
Leer los datos principales de un CFDI 4.0 en Wee.Reporteria con LINQ to XML, de forma segura:

```csharp
using System.Xml;
using System.Xml.Linq;

XNamespace cfdi = "http://www.sat.gob.mx/cfd/4";

var ajustes = new XmlReaderSettings
{
    DtdProcessing = DtdProcessing.Prohibit, // explícito: sin DTD ni entidades
    XmlResolver = null
};

using var lector = XmlReader.Create("factura.xml", ajustes);
var doc = XDocument.Load(lector);

var comprobante = doc.Root!; // cfdi:Comprobante
var total = decimal.Parse((string)comprobante.Attribute("Total")!, CultureInfo.InvariantCulture);
var rfcEmisor = (string)comprobante.Element(cfdi + "Emisor")!.Attribute("Rfc")!;
var conceptos = comprobante.Element(cfdi + "Conceptos")!.Elements(cfdi + "Concepto").Count();

Console.WriteLine($"{rfcEmisor} · {conceptos} conceptos · {total:C}");
```

Nota el `cfdi + "Emisor"`: sin el namespace, `Element("Emisor")` devuelve `null`. Y `InvariantCulture` al parsear el total, porque el XML siempre usa punto decimal (ver [[Internacionalización y localización (es-MX)]]).

### Cuándo usarlo y cuándo no
- **Sí**: cuando un estándar lo exige (CFDI, SOAP, SAML, RSS, Office Open XML), cuando necesitas validación XSD o firmas XML, o documentos con contenido mixto (texto con marcas).
- **No**: APIs nuevas entre tus sistemas; JSON es más simple y ligero (ver [[JSON]]).

### Errores comunes
1. **Ignorar los namespaces** al consultar. Las consultas devuelven vacío. Declara `XNamespace` o usa `XmlNamespaceManager` en XPath.
2. **Habilitar DTD** o usar resolvers en documentos de usuarios. Abre XXE. Mantén `DtdProcessing.Prohibit`.
3. **Armar XML concatenando cadenas.** Un `&` o `<` en un dato rompe el documento o permite inyección. Usa `XElement` o `XmlWriter`.
4. **Parsear números con la cultura del servidor.** `1432.09` falla o se lee mal en culturas con coma decimal. Usa `InvariantCulture`.
5. **Modificar un XML firmado** (como un CFDI timbrado), aunque sea reformatearlo. Invalida la firma.

### Para profundizar
- [W3C: Extensible Markup Language (XML) 1.0](https://www.w3.org/TR/xml/)
- [W3C: Namespaces in XML 1.0](https://www.w3.org/TR/xml-names/)
- [OWASP: XML External Entity Prevention Cheat Sheet](https://cheatsheetseries.owasp.org/cheatsheets/XML_External_Entity_Prevention_Cheat_Sheet.html)
- [Microsoft Learn: introducción a LINQ to XML](https://learn.microsoft.com/dotnet/standard/linq/linq-xml-overview)

## Autoevaluación

Contesta primero en voz alta o en papel; luego despliega la respuesta.

> [!question]- ¿Qué diferencia hay entre atributos y elementos en XML?
> Elementos: `<cliente><nombre>Ana</nombre></cliente>`, pueden anidar y repetirse. Atributos: `<cliente id="7">`, valores simples y únicos por elemento. Suelen usarse para metadatos e identificadores.

> [!question]- ¿Para qué sirven los namespaces?
> Para evitar choques de nombres cuando se combinan vocabularios (`xmlns:cfdi="..."`). Al consultar con XPath o LINQ to XML hay que usar el namespace, o no encuentra los nodos.

> [!question]- ¿Qué es XXE y cómo se evita?
> XML External Entity: un documento declara entidades externas que el parser resuelve y puede leer archivos del servidor o hacer peticiones internas. Se evita deshabilitando DTD y entidades externas (en .NET moderno `XmlReader` lo prohíbe por defecto: `DtdProcessing.Prohibit`).

