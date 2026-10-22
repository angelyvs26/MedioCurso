# Requisito Funcional: RF-005 - Comunicación con SUNAT / OSE y Gestión de Constancia de Recepción (CDR)

- **Identificador Único:** `RF-005`
- **Nombre:** Comunicación con SUNAT u Operador de Servicios Electrónicos (OSE) y Validación de CDR
- **Tipo de Requisito:** Funcional (Integración y Transmisión Tributaria)
- **Prioridad:** Crítica
- **Estado:** Aprobado (Derivado del Caso de Estudio)
- **Fecha de Registro:** 2026-10-22
- **Autor / Analista:** Equipo de Arquitectura SDD

---

## 1. Descripción General
El sistema debe gestionar la comunicación con los servicios web de **SUNAT** o de un **Operador de Servicios Electrónicos (OSE)** para el envío del comprobante empaquetado en formato ZIP (`[RUC]-[TIPO]-[SERIE]-[NUMERO].zip`). El sistema debe procesar la respuesta síncrona o asíncrona (mediante tickets), extraer y validar la **Constancia de Recepción (CDR)**, verificar si el estado fue `Aceptado` (código `0`) o `Rechazado` (códigos `>= 100`), almacenar el CDR firmado digitalmente por SUNAT/OSE para efectos de auditoría fiscal, y permitir la emisión de Notas de Crédito (`07`) y Débito (`08`) para rectificaciones posteriores.

---

## 2. Justificación y Fuente del Caso de Estudio
- **Fuente:** Caso de estudio oficial ABC Software Corp, Sección 1 ("Descripción del caso").
- **Cita Textual:**
  > "El sistema actual tiene vacío por todos lados: no valida RUCs, no calcula correctamente los impuestos... no genera XMLs firmados digitalmente y menos se comunica con un Operador de Servicios Electrónicos (OSE) o directamente con la SUNAT."
- **Justificación Normativa:** La RS N.° 097-2012/SUNAT (Artículo 12) y normativas de masificación OSE (RS N.° 117-2017/SUNAT) establecen que la validez legal del comprobante emitido desde sistemas de contribuyente queda perfeccionada únicamente con la emisión del CDR correspondiente.

---

## 3. Criterios de Aceptación Verificables

#### Criterio 1: Empaquetado y envío síncrono/asíncrono
- **Dado:** Un comprobante XML firmado digitalmente en estado `FIRMADO`.
- **Cuando:** Se invoca el servicio de transmisión tributaria hacia el endpoint configurado (SUNAT o OSE).
- **Entonces:** El sistema comprime el archivo en ZIP respetando la nomenclatura `[RUC]-[TIPO_DOC]-[SERIE]-[CORRELATIVO].zip`, realiza la llamada SOAP/REST autenticada con credenciales SOL, y recibe el CDR devuelto o el ticket de procesamiento.

#### Criterio 2: Procesamiento del CDR y actualización de estado
- **Dado:** La respuesta recibida del servidor de SUNAT u OSE conteniendo el archivo `R-[RUC]-[TIPO_DOC]-[SERIE]-[CORRELATIVO].zip`.
- **Cuando:** El sistema descomprime y analiza el archivo XML de respuesta.
- **Entonces:**
  - Si el tag `cbc:ResponseCode` es `"0"`, actualiza el comprobante a estado `ACEPTADO`, registra las observaciones devueltas (si existen) y almacena el CDR con su código hash.
  - Si el código es diferente de `"0"`, actualiza el estado a `RECHAZADO`, guarda el código y mensaje de error, y notifica al usuario el motivo tributario exacto.

#### Criterio 3: Soporte para Notas de Crédito y Débito (Catálogos 09 y 10)
- **Dado:** Un comprobante en estado `ACEPTADO` que requiere anulación o modificación comercial en ABC Software Corp (devolución de mercadería, descuento por ítem o corrección de datos).
- **Cuando:** El usuario emite una Nota de Crédito o Débito.
- **Entonces:** El sistema exige referenciar el comprobante original, valida el motivo según Catálogo 09 o 10, recalcula los impuestos proporcionales, y genera el XML correspondiente bajo el esquema `UBL-CreditNote-2.1.xsd` o `UBL-DebitNote-2.1.xsd`.

---

## 4. Dependencias y Relaciones de Trazabilidad
- **Entidades de Dominio:** `ComprobanteElectronico`, `RegistroEnvioSUNAT`, `DocumentoReferenciado`.
- **Contratos de API:** `[CONT-002]`, `[CONT-003]`, `[CONT-INT-003]`.
- **Matriz Normativa:** Puntos 22 y 23 de `sunat-ubl-traceability-matrix.md`.
- **Pruebas Automatizadas:** `TEST-015` (Transmisión y verificación de CDR).
