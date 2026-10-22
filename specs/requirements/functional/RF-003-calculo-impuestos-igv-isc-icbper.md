# Requisito Funcional: RF-003 - Cálculo Integral de Tributos: IGV, ISC e ICBPER

- **Identificador Único:** `RF-003`
- **Nombre:** Cálculo y Liquidación de Impuestos (IGV, ISC bajo Catálogo 08 e ICBPER)
- **Tipo de Requisito:** Funcional (Motor de Cálculo Tributario)
- **Prioridad:** Crítica
- **Estado:** Aprobado (Derivado del Caso de Estudio)
- **Fecha de Registro:** 2026-10-22
- **Autor / Analista:** Equipo de Arquitectura SDD

---

## 1. Descripción General
El sistema debe calcular de forma matemática y legalmente exacta los tributos que gravan las operaciones comerciales de **ABC Software Corp** (abarrotes al por mayor y menor), desglosando por cada línea de comprobante y en los totales consolidados:
1. **IGV (Impuesto General a las Ventas - 18%):** Discriminado según el tipo de afectación del **Catálogo N.º 07 de SUNAT** (Operación onerosa gravada, exonerada, inafecta o gratuita).
2. **ISC (Impuesto Selectivo al Consumo):** Aplicado a mercadería gravada específica de abarrotes (bebidas azucaradas, gaseosas, energizantes, cervezas y licores) implementando estrictamente los sistemas de cálculo del **Catálogo N.º 08 de SUNAT** (Al Valor, Específico, Al Precio de Venta al Público).
3. **ICBPER (Impuesto al Consumo de las Bolsas de Plástico):** Cobro tributario fijo por cada bolsa plástica entregada al cliente final para el transporte de mercadería, conforme a la Ley N.° 30884 y Catálogo N.º 14 de SUNAT (código `7152`).

---

## 2. Justificación y Fuente del Caso de Estudio
- **Fuente:** Caso de estudio oficial ABC Software Corp, Sección 1 ("Descripción del caso") y Sección 2 ("Normativa Tributaria Peruana").
- **Cita Textual:**
  > "El sistema actual tiene vacío por todos lados: no valida RUCs, no calcula correctamente los impuestos (IGV, ISC, ICBPER, etc)... Se deberá de dar estricto cumplimiento de los requerimientos de la SUNAT (catálogos de tipos de documentos, afectación al IGV, códigos de moneda, series, correlativos y validaciones de emisión), un punto clave aquí es el catálogo 8 de SUNAT."
- **Justificación Normativa:** RS N.° 097-2012/SUNAT, Anexos 1 y 8; Ley N.° 30884 (Regulación del plástico de un solo uso).

---

## 3. Especificación de Reglas de Cálculo

### 3.1. Cálculo del IGV (Catálogo N.º 07)
- **Operación Gravada (Código `10`):**
  $$\text{ValorVentaLinea} = \text{Cantidad} \times \text{ValorUnitario} - \text{Descuentos}$$
  $$\text{IGVLinea} = \text{ValorVentaLinea} \times 0.18$$
  $$\text{PrecioUnitarioConIGV} = \text{ValorUnitario} \times 1.18$$
- **Operación Exonerada (Código `20` - Frutas, verduras y alimentos primarios sin procesar):**
  $$\text{IGVLinea} = 0.00 \quad \text{y} \quad \text{PrecioUnitario} = \text{ValorUnitario}$$
- **Operación Inafecta (Código `30`):**
  $$\text{IGVLinea} = 0.00$$

### 3.2. Cálculo del ISC bajo el Catálogo N.º 08 de SUNAT (Punto Clave)
El sistema debe implementar los tres métodos oficiales definidos en el Catálogo N.º 08:
1. **Sistema al Valor (Código `01`):** Tasa porcentual fija sobre el valor de venta (base imponible).
   $$\text{ISCLinea} = \text{ValorVentaLinea} \times \left(\frac{\text{PorcentajeISC}}{100}\right)$$
   *Nota:* Para productos afectos a ISC e IGV, el IGV se calcula sobre la suma de la base más el ISC:
   $$\text{BaseIGV} = \text{ValorVentaLinea} + \text{ISCLinea} \quad \longrightarrow \quad \text{IGVLinea} = \text{BaseIGV} \times 0.18$$
2. **Sistema Específico (Código `02`):** Monto fijo en soles por volumen o cantidad física comercializada (ej. S/ por litro de bebida alcohólica).
   $$\text{ISCLinea} = \text{CantidadLitrosOUnidades} \times \text{MontoFijoISC}$$
3. **Sistema al Precio de Venta al Público (PVP) (Código `03`):** Tasa porcentual aplicada sobre el precio de venta sugerido al público dividido entre el factor legal.

### 3.3. Cálculo del ICBPER (Ley N.° 30884)
- Por cada bolsa de plástico adicionada al comprobante en la venta de abarrotes:
  $$\text{TotalICBPER} = \text{CantidadBolsas} \times \text{TasaVigenteICBPER}$$
  *(Tasa fijada anualmente por SUNAT; no forma parte de la base imponible del IGV y se suma al final del comprobante bajo código tributario `7152`)*.

---

## 4. Criterios de Aceptación Verificables

#### Criterio 1: Discriminación y sumatoria de bases imponibles en el comprobante
- **Dado:** Una venta mixta en ABC Software Corp que incluye productos gravados con IGV, productos exonerados (alimentos agrícolas) y bolsas plásticas.
- **Cuando:** El sistema calcula los totales del comprobante.
- **Entonces:** Genera con exactitud:
  - Total Valor de Venta Gravado (código Catálogo 15: `1001`).
  - Total Valor de Venta Exonerado (código Catálogo 15: `1002`).
  - Sumatoria total de IGV (Catálogo 14: `1000`).
  - Sumatoria total de ICBPER (Catálogo 14: `7152`).
  - `ImporteTotalPagar` matemáticamente cuadrado a 2 decimales.

#### Criterio 2: Aplicación del ISC en abarrotes según Catálogo 08
- **Dado:** Un producto registrado como bebida sujeta a ISC con Sistema al Valor (`01`) y tasa del 10%.
- **Cuando:** Se factura una cantidad de 10 unidades a un valor unitario de S/ 10.00 (Total base: S/ 100.00).
- **Entonces:** El sistema calcula `ISC = S/ 10.00`, calcula la base del IGV como `S/ 110.00`, calcula `IGV = S/ 19.80` y el total de la línea como `S/ 129.80`, mapeando el desglose correspondiente en UBL 2.1.

---

## 5. Dependencias y Relaciones de Trazabilidad
- **Entidades de Dominio:** `DetalleComprobante`, `ResumenTotalesTributarios`, `Producto`.
- **Contratos de API:** `[CONT-001]`, `[CONT-003]`.
- **Matriz Normativa:** Puntos 14, 15, 16, 17, 18 y 19 de `sunat-ubl-traceability-matrix.md`.
- **Pruebas Automatizadas:** `TEST-009`, `TEST-010`, `TEST-011`.
