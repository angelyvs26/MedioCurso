# Requisito Funcional: RF-004 - Emisión de Facturas y Boletas de Venta Electrónicas

- **Identificador Único:** `RF-004`
- **Nombre:** Emisión, Numeración y Control de Facturas y Boletas Electrónicas
- **Tipo de Requisito:** Funcional (Emisión y Ciclo de Vida Fiscal)
- **Prioridad:** Crítica
- **Estado:** Aprobado (Derivado del Caso de Estudio)
- **Fecha de Registro:** 2026-10-22
- **Autor / Analista:** Equipo de Arquitectura SDD

---

## 1. Descripción General
El sistema debe permitir la emisión de Facturas Electrónicas (Tipo `01`) y Boletas de Venta Electrónicas (Tipo `03`) garantizando el control de numeración correlativa estricta y secuencial por serie, la generación del documento XML estructurado bajo el estándar **OASIS UBL 2.1** (`UBL-Invoice-2.1.xsd`), la incorporación de la firma digital **XAdES-BES**, y la generación de la representación gráfica impresa en formato Ticket (80mm/58mm para venta en mostrador de abarrotes) y formato A4, incluyendo el código QR oficial de SUNAT.

---

## 2. Justificación y Fuente del Caso de Estudio
- **Fuente:** Caso de estudio oficial ABC Software Corp, Sección 1 y 2.
- **Cita Textual:**
  > "El problema grave es que la SUNAT, mediante la RS N° 097-2012, RS N° 234-2006 y las normativas modernas de masificación de comprobantes electrónicos, les exige llevar registros formales y emitir comprobantes legales estructurados bajo el estándar internacional UBL... no genera XMLs firmados digitalmente y menos se comunica con un Operador de Servicios Electrónicos (OSE) o directamente con la SUNAT."

---

## 3. Criterios de Aceptación Verificables

#### Criterio 1: Estructura de Series y Correlatividad
- **Dado:** La configuración del emisor ABC Software Corp en el punto de venta de caja.
- **Cuando:** Se emite una Factura o Boleta.
- **Entonces:** El sistema asigna automáticamente el correlativo numérico consecutivo sin saltos ni duplicados para la serie asignada (regex Factura: `^F[A-Z0-9]{3}-[0-9]{1,8}$`, Boleta: `^B[A-Z0-9]{3}-[0-9]{1,8}$`).

#### Criterio 2: Generación del XML UBL 2.1
- **Dado:** Un comprobante validado y calculado en el dominio.
- **Cuando:** Se invoca la serialización fiscal.
- **Entonces:** El sistema genera un archivo XML que cumple estrictamente con el esquema `UBL-Invoice-2.1.xsd`, incorporando los espacios de nombres oficiales, el tipo de documento en `cbc:InvoiceTypeCode`, y todos los tags de cabecera, impuestos (`cac:TaxTotal`) y líneas (`cac:InvoiceLine`).

#### Criterio 3: Firma Digital XAdES-BES
- **Dado:** El XML UBL generado sin firma.
- **Cuando:** Se ejecuta el servicio criptográfico con el certificado X.509 activo de ABC Software Corp.
- **Entonces:** Se incrusta la firma digital dentro del nodo `/Invoice/ext:UBLExtensions/ext:UBLExtension/ext:ExtensionContent/ds:Signature`, extrayendo el `DigestValue` que actúa como código hash de control fiscal.

---

## 4. Dependencias y Relaciones de Trazabilidad
- **Entidades de Dominio:** `ComprobanteElectronico`, `SerieCorrelativo`, `DetalleComprobante`.
- **Contratos de API:** `[CONT-001]`, `[CONT-INT-001]`, `[CONT-INT-002]`.
- **Matriz Normativa:** Puntos 03, 04, 05, 10, 21 y 22 de `sunat-ubl-traceability-matrix.md`.
- **Pruebas Automatizadas:** `TEST-002`, `TEST-003`, `TEST-013`, `TEST-014`.
