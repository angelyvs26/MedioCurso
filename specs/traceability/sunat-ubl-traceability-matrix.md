# Matriz de Trazabilidad Normativa SUNAT – Estándar UBL 2.1

- **Proyecto:** ABC Software Corp - Sistema de Gestión de Comprobantes de Pago Electrónicos (SEE)
- **Marco Legal:** Resolución de Superintendencia N.° 097-2012/SUNAT y modificatorias
- **Estándar Técnico:** OASIS UBL 2.1 (`UBL-Invoice-2.1.xsd`, `UBL-CreditNote-2.1.xsd`, `UBL-DebitNote-2.1.xsd`)
- **Estado de la Matriz:** Versión 1.0 (Línea Base Normativa Verificada)
- **Ubicación:** `specs/traceability/sunat-ubl-traceability-matrix.md`

---

## 1. Guía de Interpretación de Estados de Verificación

Para evitar falsas declaraciones de cumplimiento legal, cada correspondencia se categoriza bajo los siguientes estados:
- **Confirmado:** Correspondencia verificada directamente en el texto normativo (RS 097-2012/SUNAT) y en el esquema XSD de OASIS UBL 2.1.
- **Transformación Necesaria:** El dato en el modelo interno de ABC Software Corp difiere en estructura o unidad respecto a la exigencia UBL/SUNAT y requiere mapeo algorítmico.
- **Regla Adicional Tributaria:** Validación de negocio peruana no restringida por el XSD sintáctico de OASIS (ej. validación del algoritmo módulo 11 del RUC o cuadre de centavos).
- **Pendiente de Confirmación:** Aspecto normativo sujeto a la recepción de las especificaciones completas del caso de estudio de ABC Software Corp o a confirmación de vigencia.

---

## 2. Matriz Principal de Correspondencia SUNAT – Dominio – Contratos – UBL 2.1

| N.° | Fuente Normativa | Artículo / Anexo | Regla o Dato Exigido (SUNAT) | Requisito Sistema | Entidad y Atributo Dominio | DTO / Schema | Elemento UBL 2.1 (OASIS) | Restricción XSD / Validación | Contrato / Servicio Responsable | Prueba Prevista | Estado de Verificación | Observaciones y Cuestiones Pendientes |
|---|---|---|---|---|---|---|---|---|---|---|---|---|
| **01** | RS 097-2012 | Anexo 1, Inciso 1 | Número de RUC del Emisor (11 dígitos numéricos) | `RF-001` | `EmpresaEmisora.ruc` | `ComprobanteResponseDTO.emisorRuc` | `/Invoice/cac:AccountingSupplierParty/cac:Party/cac:PartyIdentification/cbc:ID` | `schemeID="6"`, tipo `xsd:string`, regex `^[0-9]{11}$` | `CONT-001`, `UBLGenerationService` | `TEST-001` | **Confirmado** | RUC de ABC Software Corp debe ser válido y coincidir con el certificado digital. |
| **02** | RS 097-2012 | Anexo 1, Inciso 2 | Nombre o Razón Social del Emisor | `RF-001` | `EmpresaEmisora.razonSocial` | `ComprobanteResponseDTO.emisorRazonSocial` | `/Invoice/cac:AccountingSupplierParty/cac:Party/cac:PartyLegalEntity/cbc:RegistrationName` | `tipo xsd:string`, max 100 caracteres | `CONT-001`, `UBLGenerationService` | `TEST-001` | **Confirmado** | Debe coincidir exactamente con el padrón RUC de SUNAT. |
| **03** | RS 097-2012 | Anexo 1, Inciso 3 | Tipo de Comprobante de Pago (Catálogo 01: 01 Factura, 03 Boleta) | `RF-001` | `ComprobanteElectronico.tipoComprobante` | `CrearComprobanteRequestDTO.tipoComprobante` | `/Invoice/cbc:InvoiceTypeCode` | `listID="0101"`, catálogo 01: valores `"01"`, `"03"` | `CONT-001`, `FacturacionService` | `TEST-002` | **Confirmado** | Nota de Crédito usa `/CreditNote/cbc:CreditNoteTypeCode` (`"07"`). |
| **04** | RS 097-2012 | Anexo 1, Inciso 4 | Serie del Comprobante (4 caracteres alfanuméricos) | `RF-001` | `SerieCorrelativo.serie` | `CrearComprobanteRequestDTO.serie` | Primeros 4 caracteres en `/Invoice/cbc:ID` | Regex Factura: `^F[A-Z0-9]{3}$`, Boleta: `^B[A-Z0-9]{3}$` | `CONT-001`, `FacturacionService` | `TEST-002` | **Confirmado** | Primera letra obligatoria `F` para factura o `B` para boleta. |
| **05** | RS 097-2012 | Anexo 1, Inciso 4 | Número Correlativo (1 a 8 dígitos) | `RF-001` | `SerieCorrelativo.correlativo` | Parte final en `ComprobanteResponseDTO.serieCorrelativo` | Componente posterior a guion en `/Invoice/cbc:ID` | Entero positivo, formateado de 1 a 8 dígitos con ceros a la izquierda | `CONT-001`, `FacturacionService` | `TEST-003` | **Confirmado** | Unicidad estricta y correlatividad continua por serie. |
| **06** | RS 097-2012 | Anexo 1, Inciso 5 | Fecha de Emisión del Comprobante | `RF-001` | `ComprobanteElectronico.fechaEmision` | `CrearComprobanteRequestDTO.fechaEmision` | `/Invoice/cbc:IssueDate` | Formato `xsd:date` (`YYYY-MM-DD`) | `CONT-001`, `FacturacionService` | `TEST-004` | **Confirmado** | No puede ser futura ni exceder días hábiles máximos de envío. |
| **07** | RS 097-2012 | Anexo 1, Inciso 6 | Tipo y Número de Documento del Adquirente | `RF-002` | `Cliente.documentoIdentidad` | `ClienteInputDTO.tipoDocumento`, `numeroDocumento` | `/Invoice/cac:AccountingCustomerParty/cac:Party/cac:PartyIdentification/cbc:ID` | Atributo `schemeID` = Catálogo 06 (`"6"` RUC, `"1"` DNI) | `CONT-001`, `FacturacionService` | `TEST-005` | **Regla Adicional Tributaria** | En Facturas es mandatorio RUC (`6`). En Boletas >= 700 PEN es mandatorio DNI/RUC. |
| **08** | RS 097-2012 | Anexo 1, Inciso 7 | Nombre o Razón Social del Adquirente | `RF-002` | `Cliente.razonSocialONombres` | `ClienteInputDTO.razonSocial` | `/Invoice/cac:AccountingCustomerParty/cac:Party/cac:PartyLegalEntity/cbc:RegistrationName` | `tipo xsd:string`, max 100 caracteres | `CONT-001`, `FacturacionService` | `TEST-005` | **Confirmado** | Obligatorio en Facturas y Boletas identificadas. |
| **09** | RS 097-2012 | Anexo 1, Inciso 8 | Tipo de Moneda de la transacción | `RF-001` | `ComprobanteElectronico.moneda` | `CrearComprobanteRequestDTO.moneda` | `/Invoice/cbc:DocumentCurrencyCode` | Catálogo 02 / ISO 4217 (`"PEN"`, `"USD"`) | `CONT-001`, `FacturacionService` | `TEST-006` | **Confirmado** | Si moneda es extranjera, se debe registrar tipo de cambio. |
| **10** | RS 097-2012 | Anexo 1, Inciso 9 | Identificación de Ítems / Mercadería (Líneas) | `RF-003` | `DetalleComprobante.numeroItem` | `LineaComprobanteInputDTO` | `/Invoice/cac:InvoiceLine/cbc:ID` | `xsd:string`, entero secuencial 1..N | `CONT-001`, `UBLGenerationService` | `TEST-007` | **Confirmado** | Mapeado desde el catálogo de mercadería de ABC Software Corp. |
| **11** | RS 097-2012 | Anexo 1, Inciso 10 | Cantidad y Unidad de Medida del Bien | `RF-003` | `DetalleComprobante.cantidad`, `unidadMedida` | `LineaComprobanteInputDTO.cantidad`, `unidadMedida` | `/Invoice/cac:InvoiceLine/cbc:InvoicedQuantity` | Atributo `unitCode` = Catálogo 03 (ej. `"NIU"`, `"KGM"`) | `CONT-001`, `UBLGenerationService` | `TEST-007` | **Transformación Necesaria** | Homologar códigos de unidad legados hacia Catálogo 03. |
| **12** | RS 097-2012 | Anexo 1, Inciso 11 | Descripción detallada de la Mercadería | `RF-003` | `DetalleComprobante.descripcion` | `LineaComprobanteInputDTO.descripcion` | `/Invoice/cac:InvoiceLine/cac:Item/cbc:Description` | `tipo xsd:string`, no vacío, max 250 caracteres | `CONT-001`, `UBLGenerationService` | `TEST-007` | **Confirmado** | Extraído de la entidad `Producto`. |
| **13** | RS 097-2012 | Anexo 1, Inciso 12 | Valor Unitario por Ítem (Sin Impuestos) | `RF-003` | `DetalleComprobante.valorUnitario` | `LineaComprobanteInputDTO.valorUnitario` | `/Invoice/cac:InvoiceLine/cac:Price/cbc:PriceAmount` | Decimal > 0, hasta 10 decimales en UBL | `CONT-001`, `FacturacionService` | `TEST-008` | **Confirmado** | Base de cálculo antes de afectación. |
| **14** | RS 097-2012 | Anexo 1, Inciso 13 | Precio de Venta Unitario (Con Impuestos) | `RF-003` | `DetalleComprobante.precioUnitario` | Calculado en dominio | `/Invoice/cac:InvoiceLine/cac:PricingReference/cac:AlternativeConditionPrice/cbc:PriceAmount` | Atributo `priceTypeCode="01"` (Catálogo 16) | `CONT-001`, `UBLGenerationService` | `TEST-008` | **Confirmado** | Incluye IGV e ISC si aplica. |
| **15** | RS 097-2012 | Anexo 1, Inciso 14 | Tipo de Afectación al IGV por Ítem | `RF-003` | `DetalleComprobante.tipoAfectacionIGV` | `LineaComprobanteInputDTO.tipoAfectacionIGV` | `/Invoice/cac:InvoiceLine/cac:TaxTotal/cac:TaxSubtotal/cac:TaxCategory/cbc:TaxExemptionReasonCode` | Catálogo 07: `"10"` Gravado, `"20"` Exonerado, `"30"` Inafecto | `CONT-001`, `FacturacionService` | `TEST-009` | **Confirmado** | **Catálogo 07** (distinto de Catálogo 08). |
| **16** | RS 097-2012 | Anexo 1, Inciso 14 | Impuesto General a las Ventas (IGV de la Línea) | `RF-003` | `DetalleComprobante.montoIGV` | Calculado en dominio | `/Invoice/cac:InvoiceLine/cac:TaxTotal/cac:TaxSubtotal/cbc:TaxAmount` | `tipo xsd:decimal`, redondeo 2 decimales | `CONT-001`, `FacturacionService` | `TEST-009` | **Regla Adicional Tributaria** | Fórmula: `valorVenta * 0.18` para código `10`. |
| **17** | RS 097-2012 | Anexo 1, Inciso 15 | Sistema de Cálculo del ISC (Catálogo 08) | `RF-003` | `DetalleComprobante.tipoCalculoISC`, `montoISC` | `LineaComprobanteInputDTO.isc` | `/Invoice/cac:InvoiceLine/cac:TaxTotal/cac:TaxSubtotal/cac:TaxCategory[TaxScheme/ID='2000']/cbc:TierRange` | Catálogo 08: `"01"` Al valor, `"02"` Específico, `"03"` Al PVP | `CONT-001`, `FacturacionService` | `TEST-010` | **Confirmado** | Requerimiento clave en abarrotes (bebidas azucaradas, néctares y licores). |
| **18** | RS 097-2012 | Anexo 1, Inciso 16 | Total Operaciones Gravadas (Base Imponible Global) | `RF-001` | `ResumenTotalesTributarios.totalOperacionesGravadas` | `ComprobanteResponseDTO.totales` | `/Invoice/cac:TaxTotal/cac:TaxSubtotal[TaxCategory/TaxScheme/ID='1000']/cbc:TaxableAmount` | Sumatoria de valores de venta de líneas con Cat. 07 `"10"` | `CONT-001`, `FacturacionService` | `TEST-011` | **Confirmado** | Se refleja también en código 1001 de Catálogo 15. |
| **19** | RS 097-2012 | Anexo 1, Inciso 17 | Monto Total del IGV Global | `RF-001` | `ResumenTotalesTributarios.totalIGV` | `ComprobanteResponseDTO.totales.totalIGV` | `/Invoice/cac:TaxTotal/cbc:TaxAmount` | Sumatoria de IGV de todas las líneas | `CONT-001`, `FacturacionService` | `TEST-011` | **Confirmado** | Asociado a Catálogo 14 código `"1000"`, Name `"IGV"`, TaxTypeCode `"VAT"`. |
| **20** | RS 097-2012 | Anexo 1, Inciso 18 | Importe Total de la Venta | `RF-001` | `ResumenTotalesTributarios.importeTotalPagar` | `ComprobanteResponseDTO.totales.importeTotal` | `/Invoice/cac:LegalMonetaryTotal/cbc:PayableAmount` | `LineExtensionAmount + TaxAmount + Charges - Allowances` | `CONT-001`, `FacturacionService` | `TEST-012` | **Confirmado** | Importe final a cancelar por el cliente. |
| **21** | RS 097-2012 | Anexo 1, Inciso 19 | Firma Digital XAdES-BES y Resumen Criptográfico | `RF-004` | `ComprobanteElectronico.codigoHash` | `ComprobanteResponseDTO.codigoHash` | `/Invoice/ext:UBLExtensions/ext:UBLExtension/ext:ExtensionContent/ds:Signature` | Estándar W3C XMLDSig, tag `ds:DigestValue` | `CONT-001`, `FirmaDigitalService` | `TEST-013` | **Confirmado** | Obligatorio antes del envío a SUNAT/OSE. |
| **22** | RS 097-2012 | Art. 10 | Estructura de Nombrado de Archivo XML | `RF-005` | Formato generado en infraestructura | Nombre físico del archivo XML | `[RUC]-[TIPO_DOC]-[SERIE]-[CORRELATIVO].xml` | Ejemplo: `20123456789-01-F001-00000001.xml` | `UBLGenerationService` | `TEST-014` | **Confirmado** | El archivo ZIP enviado a SUNAT lleva el mismo nombre base. |
| **23** | RS 097-2012 | Art. 12 | Constancia de Recepción (CDR) de SUNAT / OSE | `RF-005` | `RegistroEnvioSUNAT.constanciaCDRXml` | `RespuestaEnvioSUNATDTO` | Archivo XML de respuesta (`R-[RUC]-[TIPO]-[SERIE]-[NUM].xml`) | Tag `/ApplicationResponse/cac:DocumentResponse/cac:Response/cbc:ResponseCode` | `SunatDeliveryService` | `TEST-015` | **Confirmado** | Código `"0"` = Aceptado. Códigos `>= 100` = Rechazo formal con motivo. |
| **24** | RS 234-2006 | Art. 13 | Registro de Inventario Permanente Valorizado (Kardex) | `RF-001` | `Producto.stockActual`, `KardexMovimiento` | `ImportarMercaderiaLegadaRequestDTO` | No aplica a UBL (Exigencia de libros y registros tributarios) | Mapeo obligatorio de campos legados: `fecha`, `ingreso`, `salida`, `producto`, `nro_documento` | `CONT-005`, `MercaderiaImportService` | `TEST-016` | **Confirmado** | Regula la formalización del almacén legado de ABC Software Corp. |
| **25** | Ley 30884 | Art. 12 | Impuesto al Consumo de Bolsas de Plástico (ICBPER) | `RF-003` | `ResumenTotalesTributarios.totalICBPER` | `CrearComprobanteRequestDTO.cantidadBolsas` | `/Invoice/cac:TaxTotal/cac:TaxSubtotal[TaxCategory/TaxScheme/ID='7152']` | Catálogo 14: `"7152"`, Name `"ICBPER"`, TaxTypeCode `"OTH"`, cobro por unidad | `CONT-001`, `FacturacionService` | `TEST-017` | **Confirmado** | Aplicación directa en ventas de mostrador de abarrotes. |
| **26** | RS 117-2017 | Art. 3 | Envío a través de Operador de Servicios Electrónicos (OSE) | `RF-005` | `RegistroEnvioSUNAT.endpointOse` | `RespuestaEnvioSUNATDTO.emisorCertificador` | SOAP `sendBill` / WSDL del OSE homologado | Token / Clave de conexión OSE y validación de CDR OSE | `CONT-003`, `SunatDeliveryService` | `TEST-018` | **Confirmado** | Alternativa y exigencia para medianos contribuyentes. |

---

## 3. Verificación Especial: Catálogo N.º 07 (IGV) vs Catálogo N.º 08 (ISC)

Para evitar la confusión crítica advertida en las directrices de diseño:

```text
┌─────────────────────────────────────────────────┐   ┌─────────────────────────────────────────────────┐
│ CATÁLOGO N.º 07: CÓDIGOS DE AFECTACIÓN AL IGV    │   │ CATÁLOGO N.º 08: CÓDIGOS DE CÁLCULO DEL ISC     │
├─────────────────────────────────────────────────┤   ├─────────────────────────────────────────────────┤
│ 10: Gravado - Operación Onerosa                 │   │ 01: Sistema al Valor (Tasa sobre base imponible)│
│ 11: Gravado - Retiro por premio                 │   │ 02: Sistema Específico (Monto fijo por unidad)  │
│ 20: Exonerado - Operación Onerosa               │   │ 03: Sistema al Precio de Venta al Público (PVP) │
│ 30: Inafecto - Operación Onerosa                │   │                                                 │
│ 40: Exportación de Bienes o Servicios           │   │                                                 │
├─────────────────────────────────────────────────┤   ├─────────────────────────────────────────────────┤
│ Ubicación UBL 2.1:                              │   │ Ubicación UBL 2.1:                              │
│ /cac:TaxCategory/cbc:TaxExemptionReasonCode     │   │ /cac:TaxCategory[TaxScheme/ID='2000']/          │
│                                                 │   │  cbc:TierRange o cálculo de Base ISC            │
└─────────────────────────────────────────────────┘   └─────────────────────────────────────────────────┘
```

---

## 4. Auditoría de Cobertura y Pendientes de Verificación

1. **Vigencia de Tasas:** Confirmada tasa de IGV general en 18% (16% IGV + 2% Impuesto de Promoción Municipal IPM).
2. **Normativa de Detracciones y Percepciones:** Se identifican como conceptos de Catálogo 15 (`2001`, `2002`, `2003`), pero quedan clasificados como **Pendientes de Confirmación** según el giro comercial final de ABC Software Corp.
3. **Validación contra XSD Local:** Se verificó que los archivos de esquema en `UBL/tradeshift-ubl-xsd/` (`UBL-Invoice-2.2.xsd` y ejemplos 2.1) soportan todos los elementos mapeados en esta matriz.
