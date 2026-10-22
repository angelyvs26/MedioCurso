# Especificación del Modelo de Dominio (Domain Model)

- **Proyecto:** ABC Software Corp - Sistema de Gestión de Comprobantes de Pago Electrónicos (SEE)
- **Marco Metodológico:** Spec-Driven Development (SDD) - Spec-First
- **Normativa Base:** Resolución de Superintendencia N.° 097-2012/SUNAT y normas complementarias
- **Estándar de Intercambio:** OASIS Universal Business Language (UBL) 2.1
- **Estado del Documento:** Versión 1.0 (Diseño Lógico Preliminar Integrado)

---

## 1. Visión y Descomposición del Dominio

El dominio del sistema resuelve la modernización del registro de mercadería de **ABC Software Corp**, transformándolo en una plataforma comercial y fiscal capaz de emitir, validar y gestionar comprobantes electrónicos (Facturas, Boletas de Venta, Notas de Crédito y Notas de Débito) conforme al estándar UBL 2.1 y la regulación tributaria peruana de SUNAT.

El dominio se estructura conceptualmente en:
1. **Entidades Principales y Agregados:** Poseen identidad única en el tiempo y encapsulan estados e invariantes de negocio.
2. **Objetos de Valor (Value Objects):** Inmutables, definidos por sus atributos, sin identidad propia.
3. **Catálogos Normativos (Tablas de Parámetros Tributarios):** Diccionarios de códigos estandarizados fijados por SUNAT.
4. **Conceptos Técnicos Externos y de Ciclo de Vida:** Firma digital, serialización UBL, tickets y estados de comunicación con SUNAT.

---

## 2. Catálogo de Entidades y Agregados del Dominio

### 2.1. [ENT-001] Agregado: `ComprobanteElectronico` (Aggregate Root)
- **Propósito:** Representa el documento fiscal y comercial emitido por ABC Software Corp ante una transacción comercial con un cliente.
- **Identidad:** Compuesta por `tipoComprobante` + `serie` + `correlativo` (identificador fiscal único e irrepetible).
- **Atributos Conceptuales:**
  - `id`: UUID (Identificador interno del dominio).
  - `tipoComprobante`: `TipoDocumento` (Catálogo 01: 01=Factura, 03=Boleta de Venta, 07=Nota de Crédito, 08=Nota de Débito).
  - `serieCorrelativo`: `SerieCorrelativo` (Value Object: Serie `F###`/`B###` y correlativo numérico).
  - `fechaEmision`: Date (Fecha de generación del comprobante; formato YYYY-MM-DD).
  - `horaEmision`: Time (Hora de emisión; formato HH:MM:SS).
  - `fechaVencimiento`: Date (Opcional, aplicable a operaciones a crédito).
  - `tipoOperacion`: `TipoOperacion` (Catálogo 17, ej. 0101=Venta Interna).
  - `moneda`: `TipoMoneda` (Catálogo 02, ISO 4217: PEN=Soles, USD=Dólares Americanos).
  - `emisor`: `EmpresaEmisora` (Referencia obligatoria a ABC Software Corp).
  - `cliente`: `Cliente` (Referencia obligatoria al adquirente/usuario).
  - `lineas`: List<`DetalleComprobante`> (Colección de 1 a N ítems de mercadería comercializada).
  - `totales`: `ResumenTotalesTributarios` (Value Object con el cálculo de bases imponibles e impuestos).
  - `documentoReferenciado`: `DocumentoReferenciado` (Opcional, obligatorio para Notas de Crédito y Débito).
  - `estado`: `EstadoComprobante` (Enum: `BORRADOR`, `FIRMADO`, `ENVIADO_SUNAT`, `ACEPTADO`, `RECHAZADO`, `DE_BAJA`).
  - `codigoHash`: String (DigestValue extraído de la firma digital XAdES-BES).
  - `observaciones`: List<String> (Mensajes devueltos por SUNAT en el CDR o notas comerciales).
- **Relaciones y Cardinalidades:**
  - Pertenece a 1 `EmpresaEmisora` (1:1).
  - Emitido a 1 `Cliente` (1:1).
  - Compuesto por 1..N `DetalleComprobante` (Composición estricta: si se elimina el comprobante, mueren sus líneas).
  - Posee 0..1 `DocumentoReferenciado` (1:0..1).
  - Registra 0..N `RegistroEnvioSUNAT` (1:0..N).
- **Reglas de Negocio e Invariantes:**
  - **[RN-CPE-01]:** La serie de Facturas debe comenzar con la letra `F` seguida de 3 caracteres alfanuméricos (`F[A-Z0-9]{3}`); la de Boletas con `B` (`B[A-Z0-9]{3}`).
  - **[RN-CPE-02]:** El correlativo numérico debe ser un entero estrictamente creciente entre `1` y `99999999`.
  - **[RN-CPE-03]:** Una Factura (`01`) exige obligatoriamente que el cliente cuente con número de RUC activo y habido (Catálogo 06, Tipo "6").
  - **[RN-CPE-04]:** Un comprobante en estado `ACEPTADO` es inmutable; no puede modificarse, sólo neutralizarse mediante Nota de Crédito o Comunicación de Bajas según corresponda.
  - **[RN-CPE-05]:** La fecha de emisión no puede ser posterior a la fecha actual y no puede tener una antigüedad mayor a la permitida por SUNAT (regla general de 3 a 7 días hábiles según vigencia normativa).

---

### 2.2. [ENT-002] Entidad: `DetalleComprobante` / `LineaComprobante`
- **Propósito:** Representa cada ítem individual de mercadería facturada dentro del comprobante electrónico.
- **Identidad:** Identidad local dentro del agregado `ComprobanteElectronico` (`numeroItem`: 1, 2, 3...).
- **Atributos Conceptuales:**
  - `numeroItem`: Integer (Posición secuencial ordinal dentro del comprobante, iniciando en 1).
  - `productoId`: UUID (Referencia al producto del inventario de ABC Software Corp).
  - `codigoMercaderia`: String (Código comercial interno del sistema legado).
  - `descripcion`: String (Detalle claro del bien o servicio vendido).
  - `unidadMedida`: `UnidadMedida` (Catálogo 03 SUNAT / UN/ECE rec 20, ej. `NIU`=Unidad, `KGM`=Kilogramo).
  - `cantidad`: Decimal (Cantidad transferida; mayor a 0, hasta 12 enteros y 10 decimales).
  - `valorUnitario`: Decimal (Valor de venta por unidad, sin incluir IGV ni otros impuestos).
  - `precioUnitario`: Decimal (Precio de venta unitario que incluye tributos; Catálogo 16: "01").
  - `tipoAfectacionIGV`: `TipoAfectacionIGV` (Catálogo 07: ej. 10=Gravado Oneroso, 20=Exonerado, 30=Inafecto).
  - `porcentajeIGV`: Decimal (Tasa aplicable de IGV, por defecto 18.00% para afectación onerosa gravada).
  - `montoIGV`: Decimal (Impuesto resultante: `valorVentaLinea * (porcentajeIGV / 100)` si es gravado).
  - `tipoCalculoISC`: `TipoCalculoISC` (Opcional, Catálogo 08: 01=Al Valor, 02=Específico, 03=Al PVP; null si no aplica).
  - `montoISC`: Decimal (Opcional, 0.00 si no aplica).
  - `valorVentaLinea`: Decimal (`cantidad * valorUnitario` neto de descuentos de ítem).
  - `descuentoLinea`: Decimal (Opcional; rebajas directas aplicadas al ítem).
- **Invariantes:**
  - **[RN-LIN-01]:** `valorVentaLinea` debe ser igual al producto exacto de `cantidad * valorUnitario` menos `descuentoLinea`.
  - **[RN-LIN-02]:** Si `tipoAfectacionIGV` es Gravado (`10`), el `montoIGV` debe ser estrictamente mayor a 0 y corresponder a la tasa legal. Si es Exonerado (`20`) o Inafecto (`30`), `montoIGV` debe ser estrictamente `0.00`.
  - **[RN-LIN-03]:** Se debe distinguir conceptualmente el Catálogo 07 (Afectación al IGV) del Catálogo 08 (Sistemas de cálculo del ISC). No deben mezclarse sus códigos ni tasas.

---

### 2.3. [ENT-003] Entidad: `EmpresaEmisora`
- **Propósito:** Modela la información tributaria y fiscal de **ABC Software Corp** como emisor electrónico autorizado ante la SUNAT.
- **Identidad:** `ruc` (11 dígitos, iniciando típicamente con 20 para personas jurídicas).
- **Atributos Conceptuales:**
  - `ruc`: String (11 dígitos numéricos con validación de dígito verificador módulo 11).
  - `razonSocial`: String (Denominación social formal inscrita en el RUC: "ABC Software Corp").
  - `nombreComercial`: String (Nombre de fantasía comercial; opcional según RUC).
  - `domicilioFiscal`: `DireccionFiscal` (Value Object con ubigeo, dirección, distrito, provincia, departamento).
  - `codigoEstablecimientoAnexo`: String (Código asignado por SUNAT, por defecto "0000" para sede principal).
  - `certificadoDigitalId`: String (Identificador o alias del certificado digital X.509 activo para la firma).
  - `estadoPadron`: String (Estado en el padrón SUNAT: Activo y Habido).
- **Reglas de Negocio:**
  - Solo puede existir un emisor principal activo en la instalación estándar de ABC Software Corp.

---

### 2.4. [ENT-004] Entidad: `Cliente` / `Receptor`
- **Propósito:** Representa a la persona natural o jurídica que adquiere los productos de mercadería.
- **Identidad:** `tipoDocumentoIdentidad` + `numeroDocumento`.
- **Atributos Conceptuales:**
  - `id`: UUID.
  - `tipoDocumentoIdentidad`: `TipoDocumentoIdentidad` (Catálogo 06: 6=RUC, 1=DNI, 4=Carnet Extranjería, 7=Pasaporte).
  - `numeroDocumento`: String (Validado según tipo: 11 dígitos para RUC, 8 dígitos para DNI).
  - `razonSocialONombres`: String (Apellidos y nombres completos o razón social registrada).
  - `direccion`: `DireccionFiscal` (Opcional en Boletas de bajo monto, obligatoria en Facturas).
  - `correoElectronico`: String (Para el envío de la representación impresa PDF y XML UBL).
- **Reglas de Negocio:**
  - **[RN-CLI-01]:** Para emisión de Facturas, el cliente debe poseer tipo de documento `6` (RUC).
  - **[RN-CLI-02]:** En Boletas de Venta por importes totales iguales o superiores a S/ 700.00 (según normativa SUNAT), es obligatorio identificar al cliente con su número de documento (DNI o RUC).

---

### 2.5. [ENT-005] Entidad: `Producto` / `Mercaderia`
- **Propósito:** Representa los bienes del catálogo comercial de ABC Software Corp, heredados y sincronizados desde su sistema legado de registro de mercadería.
- **Identidad:** `codigoInterno` (Código único asignado en el inventario de ABC Software Corp).
- **Atributos Conceptuales:**
  - `id`: UUID.
  - `codigoInterno`: String (Código alfanumérico legado, ej. `PROD-001`).
  - `codigoSunat`: String (Opcional, código de producto según catálogo UNSPSC / GS1 si aplica).
  - `descripcion`: String (Nombre claro de la mercadería).
  - `unidadMedida`: `UnidadMedida` (Catálogo 03 SUNAT, ej. `NIU`).
  - `precioBaseVenta`: Decimal (Precio de venta estándar sin impuestos).
  - `tipoAfectacionPredeterminada`: `TipoAfectacionIGV` (Catálogo 07, típicamente `10` Gravado).
  - `afectoISC`: Boolean (Indica si el producto está sujeto a Impuesto Selectivo al Consumo).
  - `stockActual`: Decimal (Control de existencias proveniente del sistema legado).
  - `estadoRegistro`: Enum (`ACTIVO`, `DISCONTINUADO`, `MIGRADO_LEGADO`).
- **Reglas de Negocio:**
  - La importación desde el sistema legado debe respetar los códigos existentes y mapear las unidades de medida comerciales a la codificación de SUNAT.

---

### 2.6. [ENT-006] Entidad: `RegistroEnvioSUNAT` / `TrazabilidadEmision`
- **Propósito:** Modela el ciclo de vida de comunicación tributaria entre el sistema del contribuyente y el servicio web de SUNAT (envío síncrono o asíncrono con ticket).
- **Identidad:** `id`: UUID.
- **Atributos Conceptuales:**
  - `comprobanteId`: UUID.
  - `fechaHoraEnvio`: DateTime.
  - `ticketEnvio`: String (Opcional; devuelto por SUNAT en envíos de resúmenes diarios o envíos asíncronos masivos).
  - `codigoRespuesta`: String (Código numérico retornado por SUNAT; "0" indica aceptación exitosa).
  - `mensajeRespuesta`: String (Texto descriptivo del estado o motivo de rechazo según catálogo de errores SUNAT).
  - `constanciaCDRXml`: Binary / String (Contenido íntegro del XML de la Constancia de Recepción CDR).
  - `hashCDR`: String (Código hash SHA del CDR entregado por SUNAT).
  - `estadoEnvio`: Enum (`EN_COLA`, `ENVIADO_ESPERANDO_CDR`, `ACEPTADO`, `ACEPTADO_CON_OBSERVACIONES`, `RECHAZADO`, `ERROR_COMUNICACION`).
- **Invariantes:**
  - Un comprobante no puede marcarse como `ACEPTADO` sin la validación previa de la firma del CDR retornado por SUNAT.

---

### 2.7. [ENT-007] Entidad: `RegistroMercaderiaHistorico` (Conciliación Legada)
- **Propósito:** Almacena los registros históricos de mercadería y movimientos del sistema anterior de ABC Software Corp para auditoría, conciliación y trazabilidad.
- **Atributos Conceptuales:**
  - `id`: UUID.
  - `codigoOrigen`: String.
  - `datosOrigenPayload`: String / JSON (Datos crudos tal como venían del archivo o tabla legada).
  - `fechaImportacion`: DateTime.
  - `estadoConciliacion`: Enum (`HOMOLOGADO`, `PENDIENTE_CORRECCION`, `CONFLICTO_DUPLICIDAD`).
  - `productoMapeadoId`: UUID (Vínculo a la entidad `Producto` generada).

---

## 3. Catálogo de Objetos de Valor (Value Objects)

1. **`SerieCorrelativo`:**
   - Atributos: `tipoComprobante` (String), `serie` (String: 4 caracteres), `correlativo` (Integer: 1..8 dígitos).
   - Formato textual: `[serie]-[correlativo_8_digitos]` (ej. `F001-00000045`).
   - Inmutabilidad: No cambia una vez asignado.
2. **`DocumentoIdentidad`:**
   - Atributos: `tipo` (`TipoDocumentoIdentidad`), `numero` (String).
   - Métodos de validación de formato: Algoritmo módulo 11 para RUC (longitud 11, inicio 10, 15, 17, 20); 8 dígitos numéricos para DNI.
3. **`DireccionFiscal`:**
   - Atributos: `ubigeo` (String de 6 dígitos según INEI), `direccion` (String), `departamento` (String), `provincia` (String), `distrito` (String), `urbanizacion` (String opcional).
4. **`ResumenTotalesTributarios`:**
   - Atributos:
     - `totalOperacionesGravadas`: Decimal (Código 1001 Catálogo 15).
     - `totalOperacionesExoneradas`: Decimal (Código 1002 Catálogo 15).
     - `totalOperacionesInafectas`: Decimal (Código 1003 Catálogo 15).
     - `totalOperacionesGratuitas`: Decimal (Código 1004 Catálogo 15).
     - `totalIGV`: Decimal (Suma de IGV de todas las líneas gravadas).
     - `totalISC`: Decimal (Suma de ISC de las líneas aplicables).
     - `totalDescuentos`: Decimal.
     - `totalOtrosCargos`: Decimal.
     - `importeTotalPagar`: Decimal (`totalValorVenta + totalIGV + totalISC + totalOtrosCargos - totalDescuentos`).
   - Invariante de cuadre aritmético: El `importeTotalPagar` debe ser matemáticamente consistente con la suma de bases y tributos redondeados a 2 decimales según la regla SUNAT.
5. **`DocumentoReferenciado`:**
   - Atributos: `serieCorrelativoOrigen` (String), `tipoDocumentoOrigen` (Catálogo 01), `fechaEmisionOrigen` (Date), `codigoTipoNota` (Catálogo 09 para NC o Catálogo 10 para ND), `motivoSustento` (String descriptivo).

---

## 4. Catálogos Normativos SUNAT Referenciados

| Código Catálogo | Denominación SUNAT | Uso en el Dominio |
|---|---|---|
| **Catálogo N.º 01** | Código de Tipo de Documento | `01` Factura, `03` Boleta de Venta, `07` Nota de Crédito, `08` Nota de Débito. |
| **Catálogo N.º 02** | Código de Tipo de Moneda | `PEN` Soles, `USD` Dólares Americanos (ISO 4217). |
| **Catálogo N.º 03** | Código de Tipo de Unidad de Medida Comercial | `NIU` Unidad, `KGM` Kilogramo, `BX` Caja, `LTR` Litro. |
| **Catálogo N.º 06** | Código de Tipo de Documento de Identidad | `0` Doc. no domiciliado, `1` DNI, `4` Carnet de extranjería, `6` RUC, `7` Pasaporte. |
| **Catálogo N.º 07** | Código de Tipo de Afectación al IGV | `10` Gravado - Operación Onerosa, `20` Exonerado - Operación Onerosa, `30` Inafecto - Operación Onerosa, `40` Exportación. *(Diferenciado estrictamente del Catálogo 08)*. |
| **Catálogo N.º 08** | Código de Tipo de Sistema de Cálculo del ISC | `01` Sistema al Valor, `02` Sistema Específico, `03` Sistema al Precio de Venta al Público. |
| **Catálogo N.º 09** | Códigos de Tipo de Nota de Crédito Electrónica | `01` Anulación de la operación, `02` Anulación por error en el RUC, `03` Corrección por error en descripción, `04` Descuento global, `07` Devolución por ítem. |
| **Catálogo N.º 10** | Códigos de Tipo de Nota de Débito Electrónica | `01` Intereses por mora, `02` Aumento en el valor, `03` Penalidades. |
| **Catálogo N.º 14** | Códigos de Tipos de Tributos | `1000` IGV (VAT), `2000` ISC (EXC), `9999` Otros tributos (OTH). |
| **Catálogo N.º 15** | Códigos de Leyendas y Conceptos Tributarios | `1000` Monto en letras, `1001` Total gravado, `1002` Total exonerado, `1003` Total inafecto, `1004` Total gratuito. |
| **Catálogo N.º 16** | Tipo de Precio de Venta Unitario | `01` Precio unitario con tributos, `02` Valor referencial en operaciones no onerosas. |
| **Catálogo N.º 17** | Código de Tipo de Operación | `0101` Venta interna, `0102` Exportación, `0103` No domiciliados. |

---

## 5. Decisiones Pendientes del Dominio

1. **Gestión de Certificado Digital:** Pendiente definir si el certificado del emisor se cargará en formato PKCS#12 (`.p12` / `.pfx`) desde el almacenamiento local de Windows/Android o si se integrará un servicio de firma remoto.
2. **Ambiente de Emisión Inicial:** Pendiente definir si el sistema iniciará operando contra el servicio web de homologación/beta de SUNAT o producción directa.
3. **Mecanismo de Almacenamiento Legado:** Pendiente conocer el formato fuente del registro de mercadería actual de ABC Software Corp (archivos CSV/Excel, base de datos SQLite o tablas relacionales previas).     - 	otalISC: Decimal (Suma de ISC de las líneas aplicables según Catálogo 08).
     - 	otalICBPER: Decimal (Impuesto a las bolsas plásticas según Ley 30884 y Catálogo 14: 7152).
     - 	otalDescuentos: Decimal.
     - 	otalOtrosCargos: Decimal.
     - importeTotalPagar: Decimal (	otalValorVenta + totalIGV + totalISC + totalICBPER + totalOtrosCargos - totalDescuentos).
   - Invariante de cuadre aritmético: El importeTotalPagar debe ser matemáticamente consistente con la suma de bases y tributos redondeados a 2 decimales según la regla SUNAT.
5. **DocumentoReferenciado:**
   - Atributos: serieCorrelativoOrigen (String), 	ipoDocumentoOrigen (Catálogo 01), echaEmisionOrigen (Date), codigoTipoNota (Catálogo 09 para NC o Catálogo 10 para ND), motivoSustento (String descriptivo).
6. **KardexMovimiento [ENT-008]:**
   - Propósito: Registro formal de movimientos de mercadería conforme al Registro de Inventario Permanente Valorizado de la RS N.° 234-2006/SUNAT.
   - Atributos: id (UUID), echa (Date), productoId (UUID), 	ipoOperacionInventario (Catálogo 12 SUNAT / Entrada o Salida), cantidad (Decimal), costoUnitario (Decimal), costoTotal (Decimal), documentoReferencia (String, con preservación del 
ro_documento legado o serie-correlativo del comprobante electrónico).
