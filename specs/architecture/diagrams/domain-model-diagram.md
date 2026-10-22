# Diagramas del Modelo de Dominio

- **Proyecto:** ABC Software Corp - Sistema de Comprobantes Electrónicos (SEE)
- **Documento:** Diagrama Conceptual y de Relaciones de Entidades del Dominio
- **Referencia Documental:** [`specs/domain/domain-model.md`](file:///d:/Projects/MedioCurso/MedioCurso/specs/domain/domain-model.md)

---

## 1. Diagrama Conceptual del Dominio y Agregados (Mermaid)

Este diagrama modela las fronteras del agregado principal `ComprobanteElectronico`, sus entidades satélite y los objetos de valor inmutables:

```mermaid
classDiagram
    direction TB

    class EmpresaEmisora {
        +String ruc
        +String razonSocial
        +String nombreComercial
        +String codigoEstablecimiento
        +String certificadoDigitalId
        +String estadoPadron
    }

    class Cliente {
        +UUID id
        +TipoDocumentoIdentidad tipoDocIdentidad
        +String numeroDocumento
        +String razonSocialONombres
        +String correoElectronico
    }

    class Producto {
        +UUID id
        +String codigoInterno
        +String codigoSunat
        +String descripcion
        +UnidadMedida unidadMedida
        +Decimal precioBaseVenta
        +TipoAfectacionIGV tipoAfectacionPredeterminada
        +Boolean afectoISC
        +Decimal stockActual
    }

    class ComprobanteElectronico {
        <<Aggregate Root>>
        +UUID id
        +TipoDocumento tipoComprobante
        +Date fechaEmision
        +Time horaEmision
        +Date fechaVencimiento
        +TipoOperacion tipoOperacion
        +TipoMoneda moneda
        +EstadoComprobante estado
        +String codigoHash
        +List~String~ observaciones
        +validarInvariantes()
        +calcularTotales()
        +marcarComoAceptado(cdr)
    }

    class DetalleComprobante {
        <<Entity>>
        +Integer numeroItem
        +UUID productoId
        +String codigoMercaderia
        +String descripcion
        +UnidadMedida unidadMedida
        +Decimal cantidad
        +Decimal valorUnitario
        +Decimal precioUnitario
        +Decimal porcentajeIGV
        +Decimal montoIGV
        +Decimal montoISC
        +Decimal valorVentaLinea
        +Decimal descuentoLinea
        +calcularSubtotales()
    }

    class SerieCorrelativo {
        <<Value Object>>
        +String tipoComprobante
        +String serie
        +Integer correlativo
        +toString() String
    }

    class ResumenTotalesTributarios {
        <<Value Object>>
        +Decimal totalOperacionesGravadas
        +Decimal totalOperacionesExoneradas
        +Decimal totalOperacionesInafectas
        +Decimal totalOperacionesGratuitas
        +Decimal totalIGV
        +Decimal totalISC
        +Decimal totalDescuentos
        +Decimal totalOtrosCargos
        +Decimal importeTotalPagar
    }

    class DireccionFiscal {
        <<Value Object>>
        +String ubigeo
        +String direccion
        +String departamento
        +String provincia
        +String distrito
    }

    class DocumentoReferenciado {
        <<Value Object>>
        +String serieCorrelativoOrigen
        +TipoDocumento tipoDocumentoOrigen
        +Date fechaEmisionOrigen
        +String codigoTipoNota
        +String motivoSustento
    }

    class RegistroEnvioSUNAT {
        <<Entity>>
        +UUID id
        +DateTime fechaHoraEnvio
        +String ticketEnvio
        +String codigoRespuesta
        +String mensajeRespuesta
        +Binary constanciaCDRXml
        +String hashCDR
        +EstadoEnvio estadoEnvio
    }

    class RegistroMercaderiaHistorico {
        <<Entity>>
        +UUID id
        +String codigoOrigen
        +String datosOrigenPayload
        +DateTime fechaImportacion
        +EstadoConciliacion estadoConciliacion
    }

    %% Relaciones
    EmpresaEmisora "1" --> "0..*" ComprobanteElectronico : emite
    EmpresaEmisora *-- "1" DireccionFiscal : domicilio fiscal
    Cliente "1" --> "0..*" ComprobanteElectronico : receptor de
    Cliente *-- "0..1" DireccionFiscal : direccion

    ComprobanteElectronico *-- "1" SerieCorrelativo : identificador fiscal
    ComprobanteElectronico *-- "1" ResumenTotalesTributarios : liquidacion fiscal
    ComprobanteElectronico *-- "1..*" DetalleComprobante : lineas de venta
    ComprobanteElectronico *-- "0..1" DocumentoReferenciado : referencia nota credito/debito
    ComprobanteElectronico "1" --> "0..*" RegistroEnvioSUNAT : ciclo de transmision

    DetalleComprobante --> "1" Producto : referencia comercial mercaderia
    RegistroMercaderiaHistorico --> "0..1" Producto : concilia hacia
```

---

## 2. Diagrama de Estados del Ciclo de Vida del Comprobante

```mermaid
stateDiagram-v2
    [*] --> BORRADOR : Crear Comprobante
    BORRADOR --> BORRADOR : Modificar Líneas / Calcular Totales
    BORRADOR --> GENERADO_XML : Generar XML UBL 2.1
    GENERADO_XML --> FIRMADO : Firma Digital XAdES-BES
    FIRMADO --> ENVIADO_SUNAT : Transmitir a Web Service SUNAT
    
    state ENVIADO_SUNAT {
        [*] --> ESPERANDO_RESPUESTA
        ESPERANDO_RESPUESTA --> PROCESANDO_TICKET : Si envío asíncrono
        PROCESANDO_TICKET --> CDR_RECIBIDO : Consultar Ticket
        ESPERANDO_RESPUESTA --> CDR_RECIBIDO : Si envío síncrono
    }

    ENVIADO_SUNAT --> ACEPTADO : CDR Código '0' (Aceptado sin reparos)
    ENVIADO_SUNAT --> ACEPTADO_CON_OBSERVACIONES : CDR Código '0' + Warnings
    ENVIADO_SUNAT --> RECHAZADO : CDR con Códigos de Error (100 - 3999)
    ENVIADO_SUNAT --> ERROR_ENVIO : Timeout / Falla de Red

    ERROR_ENVIO --> ENVIADO_SUNAT : Reintentar Envío
    ACEPTADO --> DE_BAJA : Comunicación de Baja / Nota de Crédito
    ACEPTADO_CON_OBSERVACIONES --> DE_BAJA : Nota de Crédito
```

---

## 3. Justificación de Agrupación y Fronteras del Agregado

1. **Raíz del Agregado (`ComprobanteElectronico`):**
   - Garantiza el cuadre aritmético entre la sumatoria de las líneas de detalle (`DetalleComprobante`) y los totales tributarios (`ResumenTotalesTributarios`).
   - Ninguna línea de detalle puede existir de forma independiente sin su comprobante contenedor.
2. **Desacoplamiento del Inventario (`Producto`):**
   - La entidad `Producto` (mercadería de ABC Software Corp) vive fuera del agregado del comprobante. Al facturar, el detalle del comprobante congela el código, descripción, unidad de medida y precio histórico, protegiendo al documento fiscal contra modificaciones futuras del catálogo comercial.
3. **Aislamiento de la Infraestructura de Envío (`RegistroEnvioSUNAT`):**
   - La transmisión tributaria se desacopla del estado lógico del comprobante, permitiendo registrar múltiples intentos de envío, respuestas de contingencia, reintentos de red y preservación del archivo CDR.
