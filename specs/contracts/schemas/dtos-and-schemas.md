# Especificación de DTOs, Schemas y Contratos de Datos

- **Proyecto:** ABC Software Corp - Sistema de Comprobantes de Pago Electrónicos (SEE)
- **Ámbito:** Definición agnóstica de estructuras de datos para transporte, persistencia conceptual y transformación hacia OASIS UBL 2.1
- **Ubicación:** `specs/contracts/schemas/dtos-and-schemas.md`

---

## 1. Separación de Capas de Datos

Para salvaguardar la arquitectura limpia y evitar acoplamientos nocivos entre la API, la base de datos y el formato XML exigido por SUNAT, se formaliza la siguiente distinción de estructuras:

```text
┌────────────────────────────────────────────────────────────────────────┐
│ 1. DTOs de API (Entrada / Salida en Transporte HTTP o Interfaz UI)     │
│    -> Formatos planos, ligeros, diseñados para el cliente.             │
└───────────────────────────────────┬────────────────────────────────────┘
                                    │ (Mapeo / Validación Sintáctica)
                                    ▼
┌────────────────────────────────────────────────────────────────────────┐
│ 2. Entidades del Dominio (Core de Negocio)                             │
│    -> Modelan reglas, invariantes, cálculos de impuestos y agregados.  │
└───────────────┬───────────────────────────────────────┬────────────────┘
                │ (Mapeo de Persistencia)               │ (Mapeo Tributario XML)
                ▼                                       ▼
┌───────────────────────────────┐       ┌────────────────────────────────┐
│ 3. Esquema de Persistencia    │       │ 4. Representación UBL 2.1      │
│    (Tablas / Documentos BD)   │       │    (XML regulado por SUNAT)    │
└───────────────────────────────┘       └────────────────────────────────┘
```

---

## 2. Catálogo de DTOs de Entrada (Requests)

### 2.1. [DTO-IN-001] `CrearComprobanteRequestDTO`
- **Propósito:** Carga útil recibida desde la interfaz de usuario o clientes de ABC Software Corp para solicitar la creación y cálculo de un comprobante de pago.
- **Campos y Tipos:**
  - `tipoComprobante`: String (Obligatorio; 2 caracteres numéricos según Catálogo 01: `"01"` Factura, `"03"` Boleta).
  - `serie`: String (Obligatorio; 4 caracteres alfanuméricos: regex `^[FB][A-Z0-9]{3}$`).
  - `fechaEmision`: String (Obligatorio; formato ISO `YYYY-MM-DD`).
  - `horaEmision`: String (Opcional; formato `HH:MM:SS`, default: hora del servidor).
  - `moneda`: String (Obligatorio; Catálogo 02, default: `"PEN"`).
  - `tipoOperacion`: String (Obligatorio; Catálogo 17, default: `"0101"` Venta interna).
  - `cliente`: `ClienteInputDTO` (Obligatorio; datos del receptor).
    - `tipoDocumento`: String (Obligatorio; Catálogo 06: `"6"` RUC, `"1"` DNI).
    - `numeroDocumento`: String (Obligatorio; 11 dígitos para RUC, 8 para DNI).
    - `razonSocial`: String (Obligatorio; 1..250 caracteres).
    - `direccion`: String (Opcional en boleta < 700 PEN, obligatoria en factura).
    - `correo`: String (Opcional; formato email).
  - `cantidadBolsasPlasticas`: Integer (Opcional, default: 0; bolsas entregadas gravadas con ICBPER según Ley 30884).
  - `items`: List<`LineaComprobanteInputDTO`> (Obligatorio; min 1, max 500 ítems).
    - `codigoProducto`: String (Obligatorio; código interno de mercadería).
    - `descripcion`: String (Obligatorio; 1..250 caracteres).
    - `unidadMedida`: String (Obligatorio; Catálogo 03: `"NIU"`, `"KGM"`, etc.).
    - `cantidad`: Decimal (Obligatorio; > 0, hasta 10 decimales).
    - `valorUnitario`: Decimal (Obligatorio; > 0, valor sin IGV).
    - `tipoAfectacionIGV`: String (Obligatorio; Catálogo 07: `"10"` Gravado, `"20"` Exonerado, `"30"` Inafecto).
    - `tipoCalculoISC`: String (Opcional; Catálogo 08: `"01"` Al valor, `"02"` Específico, `"03"` Al PVP; null si no aplica).
    - `tasaISC`: Decimal (Opcional; porcentaje o cuota fija por unidad/litro según Catálogo 08).
    - `descuento`: Decimal (Opcional; >= 0).
- **Validaciones Sintácticas y de Negocio:**
  - Si `tipoComprobante == "01"`, `cliente.tipoDocumento` debe ser obligatoriamente `"6"` (RUC).
  - Si `tipoComprobante == "03"` y el importe total calculado supera S/ 700.00, `cliente.numeroDocumento` no puede ser vacío.
  - Si `tipoCalculoISC` está presente, debe validar la tasa correspondiente y adicionarse a la base del IGV conforme a la RS 097-2012.
- **Transformación hacia el Dominio:**
  - El servicio de aplicación valida el DTO, instancia `Cliente`, resuelve los productos en el inventario de abarrotes, crea las entidades `DetalleComprobante`, calcula el ICBPER por bolsas plásticas y construye la raíz `ComprobanteElectronico` aplicando el cálculo de `ResumenTotalesTributarios`.

---

### 2.2. [DTO-IN-002] `EmitirNotaCreditoRequestDTO`
- **Propósito:** Solicitar la emisión de una Nota de Crédito vinculada a un comprobante previo.
- **Campos Adicionales:**
  - `serie`: String (Obligatorio; regex `^[FB][A-Z0-9]{3}$`).
  - `documentoModificado`: Objeto con `tipoDocumento`, `serieCorrelativo`, `fechaEmision`.
  - `codigoMotivo`: String (Obligatorio; Catálogo 09 SUNAT: `"01"` Anulación, `"07"` Devolución, etc.).
  - `sustentoMotivo`: String (Obligatorio; explicación del ajuste).

---

### 2.3. [DTO-IN-003] `ImportarMercaderiaLegadaRequestDTO`
- **Propósito:** Ingesta por lotes de los datos históricos del software básico de mercadería de ABC Software Corp para formalización del Kardex (RS 234-2006).
- **Campos:**
  - `formatoOrigen`: String (`"CSV"`, `"JSON"`, `"EXCEL"`).
  - `sobreescribirExistentes`: Boolean (Bandera de control de duplicados).
  - `registros`: List<`FichaMercaderiaLegadaDTO`> (Colección de registros históricos):
    - `fecha`: String (Obligatorio; fecha original del movimiento `YYYY-MM-DD`).
    - `ingreso`: Decimal (Cantidad de unidades que entraron al almacén, >= 0).
    - `salida`: Decimal (Cantidad de unidades que salieron del almacén, >= 0).
    - `producto`: String (Código o descripción informal del producto en el software anterior).
    - `nro_documento`: String (Número de documento interno informal original preservado para trazabilidad).

---

## 3. Catálogo de DTOs de Salida (Responses)

### 3.1. [DTO-OUT-001] `ComprobanteResponseDTO`
- **Propósito:** Devolver la información completa del comprobante emitido, su estado actual y metadatos tributarios.
- **Campos:**
  - `id`: UUID.
  - `tipoComprobante`: String (`"01"` Factura, `"03"` Boleta).
  - `serieCorrelativo`: String (`"F001-00000123"`).
  - `fechaEmision`: String (`"YYYY-MM-DD"`).
  - `horaEmision`: String (`"HH:MM:SS"`).
  - `cliente`: Objeto con tipoDoc, numeroDoc, razonSocial.
  - `moneda`: String (`"PEN"`).
  - `totales`:
    - `totalOperacionesGravadas`: Decimal.
    - `totalOperacionesExoneradas`: Decimal.
    - `totalOperacionesInafectas`: Decimal.
    - `totalIGV`: Decimal.
    - `totalISC`: Decimal (Catálogo 08: bebidas azucaradas, licores).
    - `totalICBPER`: Decimal (Ley 30884: bolsas plásticas).
    - `importeTotal`: Decimal.
  - `items`: Lista de ítems procesados con subtotales calculados.
  - `estado`: String (`"BORRADOR"`, `"FIRMADO"`, `"ACEPTADO_SUNAT"`, etc.).
  - `codigoHash`: String (DigestValue de la firma).
  - `enlacesDescarga`:
    - `xml`: String (URL o identificador de descarga del XML UBL firmado).
    - `pdf`: String (URL o identificador de descarga de la representación impresa).

---

### 3.2. [DTO-OUT-002] `RespuestaEnvioSUNATDTO`
- **Propósito:** Detalle del resultado de la transmisión hacia los servicios web de SUNAT.
- **Campos:**
  - `comprobanteId`: UUID.
  - `serieCorrelativo`: String.
  - `fechaHoraTransmision`: String (ISO 8601).
  - `codigoRespuestaSunat`: String (`"0"` = Éxito / Catálogo de Errores SUNAT).
  - `descripcionRespuesta`: String (Mensaje del CDR o detalle de rechazo).
  - `estadoFinal`: String (`"ACEPTADO"`, `"RECHAZADO"`, `"OBSERVADO"`).
  - `tieneCdr`: Boolean.
  - `observaciones`: List<String> (Lista de advertencias devueltas por SUNAT).

---

## 4. Representación UBL 2.1 y Reglas de Mapeo

El estándar **UBL 2.1 (Universal Business Language)** de OASIS se mapea formalmente desde el Dominio al generar el XML tributario para SUNAT:

| Concepto en Dominio | Atributo Dominio | Elemento UBL 2.1 (OASIS) | Atributos XML / Esquema UBL |
|---|---|---|---|
| Raíz del Documento | `ComprobanteElectronico` | `/Invoice` (para 01/03) o `/CreditNote` (07) | `xmlns="urn:oasis:names:specification:ubl:schema:xsd:Invoice-2"` |
| Versión UBL | Constante `"2.1"` | `/Invoice/cbc:UBLVersionID` | Texto `"2.1"` |
| Personalización SUNAT | Constante `"2.0"` | `/Invoice/cbc:CustomizationID` | Texto `"2.0"` |
| Serie y Correlativo | `serieCorrelativo` | `/Invoice/cbc:ID` | Cadena formato `F###-########` |
| Fecha de Emisión | `fechaEmision` | `/Invoice/cbc:IssueDate` | Formato `YYYY-MM-DD` |
| Hora de Emisión | `horaEmision` | `/Invoice/cbc:IssueTime` | Formato `HH:MM:SS` |
| Tipo de Comprobante | `tipoComprobante` | `/Invoice/cbc:InvoiceTypeCode` | `listID="0101"` (Catálogo 01: `"01"`, `"03"`) |
| Moneda | `moneda` | `/Invoice/cbc:DocumentCurrencyCode` | Catálogo 02 (ej. `"PEN"`) |
| Emisor (RUC) | `emisor.ruc` | `/Invoice/cac:AccountingSupplierParty/cac:Party/cac:PartyIdentification/cbc:ID` | `schemeID="6"` (Catálogo 06) |
| Emisor (Razón Social) | `emisor.razonSocial` | `/Invoice/cac:AccountingSupplierParty/cac:Party/cac:PartyLegalEntity/cbc:RegistrationName` | Cadena alfanumérica |
| Receptor (Doc. Identidad) | `cliente.numeroDocumento` | `/Invoice/cac:AccountingCustomerParty/cac:Party/cac:PartyIdentification/cbc:ID` | `schemeID` = Tipo Doc (Catálogo 06) |
| Receptor (Razón Social) | `cliente.razonSocialONombres` | `/Invoice/cac:AccountingCustomerParty/cac:Party/cac:PartyLegalEntity/cbc:RegistrationName` | Cadena alfanumérica |
| Total IGV Global | `totales.totalIGV` | `/Invoice/cac:TaxTotal/cbc:TaxAmount` | `currencyID="PEN"` |
| Subtotal IGV Gravado | `totales.totalOperacionesGravadas` | `/Invoice/cac:TaxTotal/cac:TaxSubtotal/cbc:TaxableAmount` | Base imponible gravada |
| Código de Tributo IGV | Constante Cat. 14 (`"1000"`) | `/Invoice/cac:TaxTotal/cac:TaxSubtotal/cac:TaxCategory/cac:TaxScheme/cbc:ID` | `"1000"`, Name=`"IGV"`, TaxTypeCode=`"VAT"` |
| Total Importe a Pagar | `totales.importeTotalPagar` | `/Invoice/cac:LegalMonetaryTotal/cbc:PayableAmount` | `currencyID="PEN"` |
| Total Valor Venta Neto | `totales.totalOperacionesGravadas` | `/Invoice/cac:LegalMonetaryTotal/cbc:LineExtensionAmount` | Sumatoria neta de líneas |
| Ítem / Línea de Detalle | `DetalleComprobante` | `/Invoice/cac:InvoiceLine` | Colección repetitiva |
| Número de Ítem | `linea.numeroItem` | `/Invoice/cac:InvoiceLine/cbc:ID` | Entero secuencial |
| Cantidad de Mercadería | `linea.cantidad` | `/Invoice/cac:InvoiceLine/cbc:InvoicedQuantity` | `unitCode="NIU"` (Catálogo 03) |
| Valor Venta de la Línea | `linea.valorVentaLinea` | `/Invoice/cac:InvoiceLine/cbc:LineExtensionAmount` | `currencyID="PEN"` |
| Precio Unitario con Impuesto | `linea.precioUnitario` | `/Invoice/cac:InvoiceLine/cac:PricingReference/cac:AlternativeConditionPrice/cbc:PriceAmount` | `priceTypeCode="01"` (Catálogo 16) |
| Afectación al IGV Ítem | `linea.tipoAfectacionIGV` | `/Invoice/cac:InvoiceLine/cac:TaxTotal/cac:TaxSubtotal/cac:TaxCategory/cbc:TaxExemptionReasonCode` | Catálogo 07 (ej. `"10"`) |
| Código Interno Producto | `linea.codigoMercaderia` | `/Invoice/cac:InvoiceLine/cac:Item/cac:SellersItemIdentification/cbc:ID` | Cadena de inventario legado |
| Descripción Mercadería | `linea.descripcion` | `/Invoice/cac:InvoiceLine/cac:Item/cbc:Description` | Cadena alfanumérica |
| Valor Unitario sin Impuesto | `linea.valorUnitario` | `/Invoice/cac:InvoiceLine/cac:Price/cbc:PriceAmount` | `currencyID="PEN"` |

---

## 5. Datos Persistidos (Nivel Conceptual de Almacenamiento)

A nivel conceptual, la persistencia monolítica requiere tres grupos tabulares:
1. **`comprobantes`:** Cabecera fiscal (id, tipo, serie, correlativo, fechas, cliente_id, totales_json, estado, hash, fecha_creacion).
2. **`comprobante_detalles`:** Líneas normalizadas (id, comprobante_id, numero_item, producto_id, cantidad, valor_unitario, precio_unitario, afectacion_igv, igv_monto, total_linea).
3. **`envios_sunat_log`:** Trazabilidad de comunicación (id, comprobante_id, fecha_envio, ticket, codigo_cdr, mensaje, cdr_xml_path, hash_cdr, estado_envio).
4. **`productos_mercaderia`:** Catálogo de productos importado del sistema legado de ABC Software Corp.
