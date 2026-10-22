# Requisito Funcional: RF-001 - Migración y Conciliación del Inventario Legado

- **Identificador Único:** `RF-001`
- **Nombre:** Migración, Mapeo y Conciliación de Movimientos de Mercadería Históricos
- **Tipo de Requisito:** Funcional (Gestión de Inventario y Migración)
- **Prioridad:** Alta
- **Estado:** Aprobado (Derivado del Caso de Estudio)
- **Fecha de Registro:** 2026-10-22
- **Autor / Analista:** Equipo de Arquitectura SDD

---

## 1. Descripción General
El sistema debe proveer un mecanismo automático y auditable para importar, acoplar y mapear limpiamente toda la información histórica del software básico de mercadería de **ABC Software Corp** hacia el nuevo modelo de dominio sin perder trazabilidad. Los datos de origen provienen de fichas informales con los campos: `fecha`, `ingreso`, `salida`, `producto` y `nro_documento`. El sistema debe transformar estos registros en un historial de inventario formal acorde con el **Registro de Inventario Permanente Valorizado (Kardex)** exigido por la **RS N° 234-2006/SUNAT**.

---

## 2. Justificación y Fuente del Caso de Estudio
- **Fuente:** Caso de estudio oficial ABC Software Corp, Sección 1 y 2 ("Legado y Migración de Datos").
- **Cita Textual:**
  > "Tienen un software operativo básico que cumple con lo justo y necesario para registrar el día a día del almacén (controlan fichas sencillas con campos como fecha, ingreso, salida, producto y un número de documento interno bastante informal)... Toda la información histórica del software básico de mercadería (fecha, ingreso, salida, producto, nro_documento) debe acoplarse y mapearse limpiamente hacia las nuevas entidades del sistema sin perder trazabilidad, por lo que el sistema debe manejar migración de datos."
- **Justificación Normativa:** La RS N° 234-2006/SUNAT establece los formatos oficiales y campos obligatorios para el control de inventarios físicos y valorizados.

---

## 3. Criterios de Aceptación Verificables

#### Criterio 1: Ingesta completa y mapeo sin pérdida de datos
- **Dado:** Un lote histórico de registros del software básico que contiene `fecha`, `ingreso`, `salida`, `producto` y `nro_documento`.
- **Cuando:** El administrador ejecuta el proceso de migración de datos legados.
- **Entonces:** El sistema crea o actualiza las entidades `Producto` de mercadería, registra los movimientos en el Kardex asociando la fecha histórica, el tipo de movimiento (ingreso o salida), las unidades físicas y conserva en un campo de referencia el `nro_documento` informal original sin alterarlo.

#### Criterio 2: Conciliación de saldos de stock
- **Dado:** La secuencia histórica de ingresos y salidas de un producto de abarrotes.
- **Cuando:** Concluye la importación de las fichas legadas.
- **Entonces:** El saldo de `stockActual` en la entidad `Producto` debe ser exactamente igual a $\sum \text{ingresos} - \sum \text{salidas}$, registrando una entrada de conciliación si existe discrepancia física justificada.

#### Criterio 3: Manejo de productos no catalogados o duplicados
- **Dado:** Un registro legado con una descripción de producto no estandarizada o código repetido.
- **Cuando:** El motor de migración procesa la línea.
- **Entonces:** El sistema marca el registro en estado `PENDIENTE_CORRECCION` en `RegistroMercaderiaHistorico`, permitiendo la homologación manual sin detener el resto del lote.

---

## 4. Dependencias y Relaciones de Trazabilidad
- **Entidades de Dominio:** `Producto`, `RegistroMercaderiaHistorico`, `KardexMovimiento`.
- **Contratos de API:** `[CONT-005]` `POST /api/v1/mercaderia/importar-legado`.
- **Funcionalidad Asociada:** `FEAT-001-migracion-mercaderia-legada`.
- **Pruebas Automatizadas:** `TEST-016` (Prueba de importación y cuadre de saldos de inventario).
