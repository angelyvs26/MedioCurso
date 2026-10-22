# Especificación de Contratos de API y Servicios de Aplicación

- **Proyecto:** ABC Software Corp - Sistema de Gestión de Comprobantes de Pago Electrónicos (SEE)
- **Documento:** Catálogo de Contratos de Interfaz (HTTP y Servicios Internos)
- **Estado:** Versión 1.0 (Diseño Preliminar Integrado)
- **Ubicación:** `specs/contracts/api-and-service-contracts.md`

---

## 1. Clasificación de Contratos de Comunicación

Para salvaguardar la cohesión del monolito y evitar exponer operaciones internas innecesarias, los contratos se dividen en:
1. **Contratos de Interfaz Externa / HTTP (API de Transporte):** Operaciones expuestas por los controladores (`src/controllers/`) consumidas por la interfaz de usuario (escritorio Windows o móvil Android).
2. **Contratos Internos de Servicios de Aplicación (Interfaces de Casos de Uso):** Contratos lógicos entre capas internas del monolito (ej. generación UBL, firma digital y cliente SOAP de SUNAT).

---

## 2. Catálogo de Contratos de Interfaz de Transporte (HTTP / API)

### 2.1. [CONT-001] Creación y Emisión de Comprobante Electrónico
- **Nombre:** `POST /api/v1/comprobantes` (Emitir Comprobante)
- **Propósito:** Recibir la solicitud comercial de emisión de Factura o Boleta, calcular bases imponibles e impuestos, validar reglas SUNAT, generar XML UBL 2.1, firmarlo digitalmente y persistirlo.
- **Actor Consumidor:** Módulo de Ventas / Facturación (UI Windows / UI Android).
- **Componente Responsable:** `ComprobantesController` $\rightarrow$ `FacturacionService`.
- **Método y Ruta Propuestos:** `POST /api/v1/comprobantes`
- **DTO de Entrada:** `[DTO-IN-001] CrearComprobanteRequestDTO` (ver `specs/contracts/schemas/dtos-and-schemas.md`).
- **DTO de Salida:** `[DTO-OUT-001] ComprobanteResponseDTO`.
- **Validaciones Principales:**
  - Existencia y stock suficiente de mercadería referenciada en `items`.
  - Validación de RUC activo/habido si el tipo de comprobante es Factura (`01`).
  - Validación de serie autorizada (`F###` para factura, `B###` para boleta).
  - Cuadre aritmético de importes (tolerancia máxima de 1 centavo por redondeo según SUNAT).
- **Errores y Excepciones:**
  - `400 Bad Request`: Formato de DTO inválido o regla de validación incumplida (`COD_ERR_VALIDACION_DATOS`).
  - `409 Conflict`: Numeración correlativa duplicada para la serie solicitada (`COD_ERR_CORRELATIVO_DUPLICADO`).
  - `422 Unprocessable Entity`: Inconsistencia tributaria (ej. cliente sin RUC en Factura).
- **Reglas de Autorización:** Rol `Vendedor` o `Administrador`.
- **Requisitos Vinculados:** `RF-001`, `RF-002`, `RNF-003`.
- **Estado:** Propuesto (Pendiente de confirmación con el caso de estudio).

---

### 2.2. [CONT-002] Consulta y Descarga de Comprobante y Artefactos
- **Nombre:** `GET /api/v1/comprobantes/{tipo}/{serie}-{correlativo}`
- **Propósito:** Consultar los datos de un comprobante emitido, su estado ante SUNAT y enlaces de descarga de su representación XML firmada y CDR.
- **Actor Consumidor:** Módulo de Consulta / Auditoría / Cliente Final.
- **Componente Responsable:** `ComprobantesController` $\rightarrow$ `ConsultaComprobanteService`.
- **Método y Ruta:** `GET /api/v1/comprobantes/{tipo}/{serie}-{correlativo}`
- **DTO de Entrada:** Parámetros de ruta (`tipo`, `serie`, `correlativo`).
- **DTO de Salida:** `[DTO-OUT-001] ComprobanteResponseDTO`.
- **Errores:** `404 Not Found` si el comprobante no existe en la persistencia.
- **Estado:** Propuesto.

---

### 2.3. [CONT-003] Transmisión y Consulta de Estado ante SUNAT
- **Nombre:** `POST /api/v1/comprobantes/{id}/transmitir-sunat`
- **Propósito:** Enviar el paquete XML UBL firmado a los servicios web de SUNAT y procesar de forma síncrona o asíncrona la Constancia de Recepción (CDR).
- **Actor Consumidor:** Proceso automático en segundo plano o acción manual del usuario de caja.
- **Componente Responsable:** `ComprobantesController` $\rightarrow$ `SunatDeliveryService`.
- **Método y Ruta:** `POST /api/v1/comprobantes/{id}/transmitir-sunat`
- **DTO de Entrada:** Parámetro `id` del comprobante en la ruta.
- **DTO de Salida:** `[DTO-OUT-002] RespuestaEnvioSUNATDTO`.
- **Errores:**
  - `400 Bad Request`: Comprobante no se encuentra en estado `FIRMADO`.
  - `502 Bad Gateway`: Timeout o falla de conexión contra los servidores de SUNAT.
  - `422 Unprocessable`: Rechazo formal de SUNAT con código de error tributario (100 - 3999).
- **Estado:** Propuesto.

---

### 2.4. [CONT-004] Consulta de Catálogo de Mercadería y Stock
- **Nombre:** `GET /api/v1/mercaderia`
- **Propósito:** Obtener la lista de productos disponibles para facturación, filtrando por código, descripción o categoría, heredados del registro de ABC Software Corp.
- **Actor Consumidor:** Pantalla de facturación (búsqueda de ítems para venta).
- **Componente Responsable:** `MercaderiaController` $\rightarrow$ `GestionMercaderiaService`.
- **Método y Ruta:** `GET /api/v1/mercaderia?q={filtro}&limite={n}`
- **DTO de Salida:** Lista de `ProductoDTO` (código, descripción, unidadMedida, precioBase, afectacionIGV).
- **Estado:** Propuesto.

---

### 2.5. [CONT-005] Importación y Conciliación de Mercadería del Sistema Legado
- **Nombre:** `POST /api/v1/mercaderia/importar-legado`
- **Propósito:** Cargar un lote de mercadería desde el archivo de datos del sistema previo de ABC Software Corp, validar coherencia y homologar unidades de medida según Catálogo 03 SUNAT.
- **Actor Consumidor:** Administrador del sistema / Proceso de migración.
- **Componente Responsable:** `MercaderiaController` $\rightarrow$ `MercaderiaImportService`.
- **Método y Ruta:** `POST /api/v1/mercaderia/importar-legado`
- **DTO de Entrada:** `[DTO-IN-003] ImportarMercaderiaLegadaRequestDTO`.
- **DTO de Salida:** `ResultadoImportacionDTO` (total procesados, homologados, errores).
- **Estado:** Propuesto.

---

### 2.6. [CONT-006] Consulta de Catálogos Tributarios Normativos
- **Nombre:** `GET /api/v1/catalogos/{numeroCatalogo}`
- **Propósito:** Proveer a los clientes de interfaz los diccionarios tributarios oficiales de SUNAT para poblar selectores (Catálogo 01 Tipos de Comprobante, Catálogo 06 Tipos de Documento Identidad, Catálogo 07 Afectación IGV, Catálogo 08 Cálculo ISC).
- **Componente Responsable:** `CatalogosController` $\rightarrow$ `CatalogosTributariosService`.
- **DTO de Salida:** Lista de pares `{ codigo: string, descripcion: string }`.
- **Estado:** Confirmado según Anexo 8 de RS 097-2012/SUNAT.

---

## 3. Catálogo de Contratos Internos de Servicios de Aplicación (Interfaces Lógicas)

Estos contratos definen la interacción puramente desacoplada dentro de `src/services/` y `src/infrastructure/`, operando como Puertos de Salida:

### 3.1. [CONT-INT-001] `IUBLDocumentGeneratorPort` (Servicio Generador XML UBL 2.1)
- **Firma del Método:**
  `generarXmlUbl(comprobante: ComprobanteElectronico): DocumentoXML`
- **Propósito:** Recibe la entidad pura del dominio `ComprobanteElectronico` y la transforma en un documento XML que cumple estrictamente la sintaxis y nombres de elementos de `UBL-Invoice-2.1.xsd` o `UBL-CreditNote-2.1.xsd`.
- **Validación Integrada:** Validación sintáctica contra los XSDs ubicados en `UBL/tradeshift-ubl-xsd/`.

---

### 3.2. [CONT-INT-002] `IDigitalSignaturePort` (Servicio Criptográfico de Firma Digital)
- **Firma del Método:**
  `firmarXml(xmlSinFirma: DocumentoXML, certificadoId: String): DocumentoXMLFirmado`
- **Propósito:** Inserta el nodo `ds:Signature` bajo el estándar W3C XMLDSig / XAdES-BES dentro del nodo `ext:UBLExtension` de la cabecera UBL, calculando el valor hash de resumen (`cbc:DigestValue`).

---

### 3.3. [CONT-INT-003] `ISunatDeliveryClientPort` (Cliente de Comunicación SOAP SUNAT)
- **Firma del Método:**
  `enviarComprobante(nombreZip: String, archivoZipBytes: ByteArray): RespuestaSunatRaw`
- **Propósito:** Empaqueta el XML firmado en formato ZIP (`[RUC]-[TIPO]-[SERIE]-[NUMERO].zip`), invoca la operación `sendBill` del Web Service SOAP de SUNAT (`BillService`), desempaqueta la respuesta y extrae el XML de la Constancia de Recepción (CDR).

---

## 4. Decisiones Pendientes sobre Contratos

1. **Protocolo de Comunicación Final:** La especificación actual utiliza rutas RESTful neutrales. Pendiente confirmar si se requiere comunicación local IPC/In-Memory para clientes desktop monolíticos monousuario.
2. **Formato Formal de Schemas:** Pendiente confirmar si el cliente prefiere especificación OpenAPI 3.0 YAML o JSON Schemas estándar.
