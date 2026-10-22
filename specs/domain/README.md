# Modelo de Dominio y Glosario del Negocio (`specs/domain/`)

Este directorio contiene la especificación formal del **Modelo de Dominio** del sistema `MedioCurso` para **ABC Software Corp**, formalizando la transición desde su sistema básico de registro de mercadería hacia una solución integral de gestión de comprobantes de pago electrónicos conforme a la normativa de la **SUNAT** y el estándar **OASIS UBL 2.1**.

---

## 1. Propósito y Principios de Dominio

1. **Agnosticismo Tecnológico:** Los conceptos de dominio descritos aquí representan reglas puras del negocio e invariantes tributarias. No dependen de bases de datos, frameworks web ni librerías de serialización XML.
2. **Distinción entre Dominio, DTO y XML UBL:**
   - **Modelo de Dominio (`specs/domain/`):** Entidades puras y objetos de valor con comportamiento, identidad y reglas de cálculo.
   - **Contratos de Datos y DTOs (`specs/contracts/schemas/`):** Estructuras planas de entrada y salida para el transporte entre capas y clientes externos.
   - **Representación XML UBL 2.1 (`specs/contracts/` / `UBL/`):** Esquema estándar OASIS regulado por SUNAT para el intercambio fiscal.
3. **Lenguaje Ubicuo (Glosario Compartido):** Establece el vocabulario común entre los desarrolladores, el caso de estudio de ABC Software Corp y las resoluciones de la SUNAT (RS 097-2012/SUNAT y normas modificatorias).

---

## 2. Documentos en esta Sección

- [`domain-model.md`](file:///d:/Projects/MedioCurso/MedioCurso/specs/domain/domain-model.md): Catálogo completo de entidades, objetos de valor, agregados, reglas de negocio, invariantes y catálogo de conceptos del dominio.

---

## 3. Glosario Fundamental de Términos (Ubiquitous Language)

| Término | Definición de Negocio en ABC Software Corp / SUNAT | Clasificación en el Modelo |
|---|---|---|
| **Mercadería / Producto** | Bien o servicio comercializado por ABC Software Corp, registrado originalmente en su sistema legado con código, descripción, stock y precio base. | Entidad (`Producto`) |
| **Comprobante de Pago Electrónico (CPE)** | Documento tributario regulado emitido en formato digital (Factura, Boleta de Venta, Nota de Crédito, Nota de Débito). | Raíz de Agregado (`ComprobanteElectronico`) |
| **Emisor** | ABC Software Corp, contribuyente obligado tributariamente que emite el comprobante electrónico con RUC y certificado digital. | Entidad (`EmpresaEmisora`) |
| **Receptor / Cliente** | Persona natural o jurídica que adquiere la mercadería, identificada con documento tributario válido. | Entidad (`Cliente`) |
| **Serie y Correlativo** | Identificador fiscal del comprobante. La serie alfanumérica identifica el punto de emisión (`F###` para facturas, `B###` para boletas); el correlativo es numérico secuencial. | Objeto de Valor (`SerieCorrelativo`) |
| **Afectación al IGV** | Régimen tributario aplicable a la línea de venta según Catálogo SUNAT N.º 07 (Gravado, Exonerado, Inafecto, Exportación). | Objeto de Valor (`TipoAfectacionIGV`) |
| **Sistema de Cálculo del ISC** | Método aplicable si el producto está afecto al Impuesto Selectivo al Consumo según Catálogo SUNAT N.º 08 (Al Valor, Específico, Al Precio de Venta). | Objeto de Valor (`TipoCalculoISC`) |
| **Constancia de Recepción (CDR)** | Archivo XML firmado por SUNAT que acredita la recepción, estado de validez (Aceptado/Rechazado) y observaciones de un comprobante emitido. | Entidad / Objeto de Valor (`ConstanciaRecepcionCDR`) |
| **Firma Digital (XAdES-BES)** | Mecanismo criptográfico incorporado en el XML UBL mediante el certificado digital del emisor para garantizar autenticidad e integridad. | Concepto Técnico / Infraestructura |
| **Conciliación Legada** | Proceso de importación, homologación y mapeo de los registros de mercadería y movimientos previos de ABC Software Corp hacia el nuevo esquema. | Servicio de Dominio / Integración |
