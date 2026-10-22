# [FEAT-002]: Emisión Rápida en Mostrador de Facturas y Boletas Electrónicas

- **Identificador Único:** `FEAT-002`
- **Nombre:** Emisión Rápida de Comprobantes con Autocompletado, Motor Tributario (IGV, ISC, ICBPER) e Impresión de Tickets
- **Estado:** Aprobada
- **Requisitos de Origen:** `RF-002`, `RF-003`, `RF-004`
- **Fecha de Creación:** 2026-10-22
- **Última Modificación:** 2026-10-22

---

## 1. Objetivo y Alcance
- **Objetivo:** Proveer una experiencia de usuario rápida y sin fricción (estilo **Nubefact**) en los puntos de venta de mostrador y pedidos al por mayor de **ABC Software Corp**, permitiendo registrar ventas en segundos, autocompletar clientes mediante RUC/DNI, liquidar automáticamente los impuestos peruanos (IGV, ISC bajo Catálogo 08, ICBPER) y emitir el comprobante con su ticket físico.
- **Alcance Incluido:**
  - Factura Electrónica (`01`) y Boleta de Venta Electrónica (`03`).
  - Consulta y validación de RUC y DNI con verificación de estado Activo/Habido.
  - Cálculo de IGV (18%), ISC (Catálogo 08) e ICBPER (bolsas plásticas).
  - Formato de impresión en Ticket (80mm/58mm) y A4 con código QR.
- **Fuera del Alcance:** Gestión de créditos o cobranzas financieras complejas.

---

## 2. Actores y Participantes
- **Cajero / Vendedor de Mostrador:** Opera en Windows (equipo de caja) o Android (tableta/móvil).
- **Cliente:** Persona natural o empresa adquirente.
- **Servicio de Facturación:** Orquesta las reglas del dominio y numeración correlativa.

---

## 3. Precondiciones
- Serie de comprobante configurada para el terminal (ej. `F001` para caja principal).
- Certificado digital X.509 activo configurado en el sistema.
- Catálogo de productos con precios y afectaciones tributarias cargados.

---

## 4. Flujo Principal de la Venta (Happy Path)
1. El Cajero abre la pantalla de venta e ingresa los productos:
   - Pistola lectora de código de barras o búsqueda rápida por texto.
   - Si el cliente solicita bolsas plásticas, el cajero indica la cantidad (ej. 2 bolsas).
2. El sistema calcula en tiempo real los subtotales:
   - Calcula valor de venta y aplica IGV (18%).
   - Si algún producto está afecto a ISC (ej. gaseosa o cerveza), aplica el método del **Catálogo 08** configurado (Sistema al Valor `01`, Específico `02`, o PVP `03`).
   - Adiciona el monto exacto de ICBPER según la cantidad de bolsas ingresadas ($2 \times \text{tasa ICBPER}$).
3. El Cajero selecciona el tipo de comprobante:
   - **Para Boleta de Venta:** Si la venta es menor a S/ 700.00, permite dejar cliente genérico o ingresar DNI; el sistema autocompleta nombres. Si supera S/ 700.00, exige DNI/RUC.
   - **Para Factura:** Exige ingresar RUC; el sistema valida el algoritmo módulo 11, autocompleta la Razón Social y verifica que figure como Habido y Activo.
4. El Cajero presiona "Cobrar y Emitir":
   - El sistema reserva atómicamente el siguiente número correlativo.
   - Construye el agregado `ComprobanteElectronico` en memoria y valida el cuadre aritmético.
   - Genera el XML UBL 2.1 y lo firma digitalmente con el certificado.
   - Persiste el comprobante en la base de datos y descuenta el stock en el Kardex.
   - Envía automáticamente la orden de impresión al ticket de caja con el código QR y código hash.
5. El comprobante queda listo en estado `FIRMADO` para su posterior o inmediata transmisión a SUNAT/OSE.

---

## 5. Reglas de Negocio Clave
- **[RN-01]:** No se puede emitir una Factura con DNI ni a un RUC que no esté en estado `ACTIVO` y `HABIDO`.
- **[RN-02]:** El Catálogo 08 (ISC) debe calcularse rigurosamente según el sistema asignado al producto en el inventario de abarrotes.
- **[RN-03]:** El ICBPER no genera débito fiscal de IGV.

---

## 6. Criterios de Aceptación
- [ ] **CA-01:** La emisión de una Boleta de mostrador con 3 ítems y cobro de bolsa plástica toma menos de 10 segundos al cajero.
- [ ] **CA-02:** Los montos de IGV, ISC e ICBPER se desglosan explícitamente en el ticket impreso y en los totales del XML UBL.
- [ ] **CA-03:** El código QR generado contiene los 9 datos obligatorios de SUNAT (RUC emisor, tipo doc, serie, correlativo, IGV, total, fecha, tipo doc cliente, nro doc cliente, hash).
