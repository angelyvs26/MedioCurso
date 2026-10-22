# [FEAT-001]: Migración de Movimientos Legados y Apertura del Kardex Formal

- **Identificador Único:** `FEAT-001`
- **Nombre:** Ingesta de Fichas Legadas de Almacén y Control de Inventario Permanente (RS 234-2006)
- **Estado:** Aprobada
- **Requisitos de Origen:** `RF-001`, `RNF-005`
- **Fecha de Creación:** 2026-10-22
- **Última Modificación:** 2026-10-22

---

## 1. Objetivo y Alcance
- **Objetivo:** Permitir la migración integral de las fichas informales de almacén de **ABC Software Corp** (`fecha`, `ingreso`, `salida`, `producto`, `nro_documento`) sin pérdida de trazabilidad, homologando los saldos de mercadería hacia el **Registro de Inventario Permanente Valorizado (Kardex)** exigido por la **RS N.° 234-2006/SUNAT**.
- **Alcance Incluido:**
  - Importación por lote desde archivos estructurados (CSV/Excel/JSON).
  - Creación de catálogo de productos con unidad de medida estandarizada (Catálogo 03 SUNAT).
  - Cálculo de saldos iniciales y registro histórico auditable.
- **Fuera del Alcance:** Facturación de los movimientos históricos pasados.

---

## 2. Actores y Participantes
- **Actor Principal:** Administrador de Almacén de ABC Software Corp.
- **Sistemas Involucrados:** Base de datos relacional local/servidor y repositorio de mercadería.

---

## 3. Precondiciones
- Archivo o tabla con los datos históricos del software básico disponible.
- Configuración de las unidades de medida básicas en el sistema (ej. `NIU` para unidades, `KGM` para kilogramos).

---

## 4. Flujos de Comportamiento

### 4.1. Flujo Principal (Happy Path)
1. El Administrador carga el archivo de fichas legadas a través de la interfaz (o endpoint `POST /api/v1/mercaderia/importar-legado`).
2. El sistema valida la consistencia sintáctica de los campos requeridos (`fecha`, `ingreso`, `salida`, `producto`, `nro_documento`).
3. Para cada registro, el sistema:
   - Verifica si el producto ya existe en el catálogo maestro; si no existe, lo crea con estado `MIGRADO_LEGADO`.
   - Crea un registro en `RegistroMercaderiaHistorico` preservando la carga cruda y el `nro_documento` original.
   - Asienta el movimiento en el Kardex (`fecha`, tipo movimiento: entrada/salida, unidades).
4. El sistema actualiza el `stockActual` del producto en la entidad `Producto`.
5. El sistema emite un reporte de conciliación con el total de registros procesados, saldo final por producto y cero errores.

### 4.2. Flujos Alternativos y Excepciones
- **Excepción 1: Producto duplicado con variaciones ortográficas (ej. "Arroz Costeño 1kg" vs "Arroz Costeño x 1 kg"):**
  - El sistema detecta similitud y marca el registro como `PENDIENTE_CORRECCION`.
  - Permite al usuario mapearlo al producto maestro consolidado.
- **Excepción 2: Saldo resultante negativo:**
  - El sistema emite una advertencia de discrepancia física en el informe de conciliación sin detener la carga del resto de registros.

---

## 5. Criterios de Aceptación
- [ ] **CA-01:** Todos los movimientos históricos conservan su `fecha` y `nro_documento` interno original para fines de auditoría.
- [ ] **CA-02:** El stock resultante en el sistema coincide matemáticamente con la diferencia de ingresos y salidas del software básico.
- [ ] **CA-03:** Cada producto queda listo con su unidad de medida según Catálogo 03 para poder emitir comprobantes electrónicos.
