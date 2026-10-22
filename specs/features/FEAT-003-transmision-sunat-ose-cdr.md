# [FEAT-003]: Transmisión a SUNAT / OSE y Gestión de Constancia de Recepción (CDR)

- **Identificador Único:** `FEAT-003`
- **Nombre:** Envío Automatizado a SUNAT / OSE con Cola de Reintentos y Validación de CDR
- **Estado:** Aprobada
- **Requisitos de Origen:** `RF-004`, `RF-005`
- **Fecha de Creación:** 2026-10-22
- **Última Modificación:** 2026-10-22

---

## 1. Objetivo y Alcance
- **Objetivo:** Garantizar la transmisión legal de los comprobantes electrónicos firmados hacia los servidores de **SUNAT** o de un **Operador de Servicios Electrónicos (OSE)**, procesando la Constancia de Recepción (**CDR**), gestionando de manera transparente caídas de red o interrupciones en el servicio web tributario mediante una cola de reintentos, y almacenando las evidencias auditables.
- **Alcance Incluido:**
  - Empaquetado ZIP de comprobantes según la nomenclatura oficial `[RUC]-[TIPO]-[SERIE]-[NUM].zip`.
  - Envío síncrono para Facturas y Notas vinculadas.
  - Envío asíncrono y consulta de tickets para Boletas y Resúmenes Diarios.
  - Descompresión y lectura del XML de respuesta CDR.
  - Manejo de contingencia por desconexión temporal.

---

## 2. Actores y Participantes
- **Sistema de Facturación (Servicio de Transmisión):** Proceso en segundo plano o invocación síncrona.
- **Web Service SUNAT / OSE:** Punto final SOAP autenticado con credenciales secundarias SOL (Usuario y Clave SOL).

---

## 3. Flujo Principal (Happy Path)
1. El sistema identifica un comprobante en estado `FIRMADO`.
2. Empaqueta el archivo XML firmado dentro de un archivo ZIP con el nombre reglamentario:
   `20123456789-01-F001-00000001.zip`
3. Invoca la operación `sendBill` del Web Service de SUNAT/OSE adjuntando el binario ZIP codificado en Base64.
4. El servidor de SUNAT/OSE valida la firma digital, el RUC del emisor, la estructura XSD y las reglas de negocio.
5. El servidor devuelve el archivo `R-20123456789-01-F001-00000001.zip`.
6. El sistema extrae el XML del CDR, lee el tag `/ApplicationResponse/cac:DocumentResponse/cac:Response/cbc:ResponseCode`:
   - Código `"0"`: Estado `ACEPTADO`. Almacena las observaciones (si existen) y el hash del CDR.
7. Notifica a la interfaz que el comprobante fue aceptado legalmente por SUNAT.

---

## 4. Flujo de Contingencia por Falla de Conexión (Opiniones de Usuarios / Estilo Nubefact)
1. Durante el intento de envío, el servidor de SUNAT no responde (timeout 504) o la conexión a internet local de ABC Software Corp falla.
2. El sistema **no bloquea la venta ni cancela el ticket al cliente**: el comprobante permanece válido en el almacén en estado `PENDIENTE_ENVIO` con su firma digital y código QR generado localmente.
3. El servicio de fondo encola el comprobante y reintenta la transmisión periódicamente respetando el plazo legal de 3 a 7 días hábiles estipulado por SUNAT.
4. En cuanto se restablece la comunicación, se transmite el lote y se actualizan los estados al recibir los CDRs.

---

## 5. Criterios de Aceptación
- [ ] **CA-01:** Ninguna falla de red externa detiene la venta en mostrador de abarrotes de ABC Software Corp.
- [ ] **CA-02:** Los CDRs aceptados quedan almacenados en la base de datos o sistema de archivos vinculados a su comprobante para auditoría.
- [ ] **CA-03:** Ante un rechazo formal de SUNAT (código $\ge 100$), el sistema muestra la descripción exacta del error (ej. "Error 2119: La fecha de emisión no puede ser anterior a...").
