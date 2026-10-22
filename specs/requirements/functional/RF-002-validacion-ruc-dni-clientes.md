# Requisito Funcional: RF-002 - Validación Algorítmica y Consulta de Documentos de Identidad

- **Identificador Único:** `RF-002`
- **Nombre:** Validación y Consulta de Documentos de Identidad (RUC y DNI)
- **Tipo de Requisito:** Funcional (Validación Tributaria y Clientes)
- **Prioridad:** Alta
- **Estado:** Aprobado (Derivado del Caso de Estudio)
- **Fecha de Registro:** 2026-10-22
- **Autor / Analista:** Equipo de Arquitectura SDD

---

## 1. Descripción General
El sistema debe validar rigurosamente la estructura y autenticidad de los documentos de identidad de los adquirentes (clientes) según el **Catálogo N.º 06 de SUNAT**. Para personas jurídicas y naturales con negocio (RUC de 11 dígitos), se debe aplicar el algoritmo oficial de verificación de dígito módulo 11. Para personas naturales (DNI de 8 dígitos numéricos), se debe validar la longitud y composición numérica. Adicionalmente, el sistema debe proveer una capacidad de autocompletado y consulta (inspirada en la experiencia de usuario de plataformas como **Nubefact**) que obtenga la Razón Social y condición fiscal (Activo/Habido) para agilizar la venta en mostrador de abarrotes.

---

## 2. Justificación y Fuente del Caso de Estudio
- **Fuente:** Caso de estudio oficial ABC Software Corp, Sección 1 ("Descripción del caso") y Sección 3 ("Descubrimiento de los Casos de Uso").
- **Cita Textual:**
  > "El sistema actual tiene vacío por todos lados: no valida RUCs, no calcula correctamente los impuestos... La gerencia desea que detalle los casos posibles, revisando opiniones de usuarios de facturación electrónica, por lo que puede revisar otros softwares en línea como es el caso de Nubefact."
- **Justificación Normativa:** La RS N.° 097-2012/SUNAT (Anexo 1, Inciso 6) exige que en toda Factura Electrónica el adquirente cuente con número de RUC válido en estado Activo y con condición de domicilio Habido.

---

## 3. Criterios de Aceptación Verificables

#### Criterio 1: Validación algorítmica de RUC mediante módulo 11
- **Dado:** Un número de RUC ingresado por el vendedor o cliente.
- **Cuando:** El sistema procesa la entrada antes de registrar o emitir.
- **Entonces:** El sistema valida que tenga exactamente 11 dígitos numéricos, inicie con los prefijos legales permitidos (`10`, `15`, `17`, `20`), y que el último dígito coincida con el residuo del algoritmo ponderado módulo 11 de la SUNAT. Si no coincide, rechaza la operación inmediatamente indicando `RUC Inválido`.

#### Criterio 2: Obligatoriedad de RUC en Facturas
- **Dado:** La intención de emitir una Factura Electrónica (`tipoComprobante = 01`).
- **Cuando:** El usuario intenta registrar un cliente con DNI (`tipo = 1`) o sin documento.
- **Entonces:** El sistema bloquea la emisión y exige un cliente con RUC válido (`tipo = 6`).

#### Criterio 3: Control de límite legal de S/ 700.00 en Boletas de Venta
- **Dado:** La emisión de una Boleta de Venta Electrónica (`tipoComprobante = 03`).
- **Cuando:** El importe total calculado es igual o superior a S/ 700.00 (setecientos soles).
- **Entonces:** El sistema exige obligatoriamente consignar el Tipo y Número de Documento de Identidad del cliente (DNI o RUC) y sus nombres completos, impidiendo la emisión a "Clientes Varios / Sin Documento".

#### Criterio 4: Consulta y autocompletado ágil (Estilo Nubefact)
- **Dado:** Un vendedor digitando un RUC o DNI en la interfaz de Windows o Android.
- **Cuando:** Se completan los dígitos válidos y se pulsa consultar.
- **Entonces:** El sistema consulta el padrón local o servicio de consulta, autocompletando la Razón Social y la Dirección Fiscal del cliente, alertando si el contribuyente figura como `NO HABIDO` o `NO ACTIVO`.

---

## 4. Dependencias y Relaciones de Trazabilidad
- **Entidades de Dominio:** `Cliente`, `DocumentoIdentidad`.
- **Contratos de API:** `[CONT-001]`, `GET /api/v1/consultas/ruc/{ruc}`, `GET /api/v1/consultas/dni/{dni}`.
- **Funcionalidad Asociada:** `FEAT-002-emision-comprobantes-con-tributos`.
- **Pruebas Automatizadas:** `TEST-005` (Validación de RUC y DNI).
