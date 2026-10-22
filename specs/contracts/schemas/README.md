# Esquemas de Datos y DTOs (`specs/contracts/schemas/`)

Este subdirectorio aloja los esquemas estructurados de intercambio de datos y definiciones de DTO (Data Transfer Objects).

---

## 1. Contenido y Formato

- **Esquemas JSON Schema / YAML:** Definiciones declarativas de campos, tipos, obligatoriedad, formatos y rangos válidos.
- **DTOs de Entrada (Requests):** Estructura exacta de los datos recibidos por los controladores.
- **DTOs de Salida (Responses/ViewModels):** Estructura de los datos expuestos hacia los clientes o vistas.

---

## 2. Relación con la Arquitectura Limpia

Los esquemas y DTOs definidos aquí pertenecen al límite de transporte/presentación. **No deben confundirse con las entidades de dominio en `src/domain/`**:
- Un DTO traslada datos a través de la frontera de la aplicación.
- Una entidad de dominio en `src/domain/` contiene reglas e invariantes del negocio.
- Los controladores y servicios de aplicación son responsables de mapear entre DTOs y entidades de dominio.

---

## 3. Estado Actual

> **Pendiente:** Definición de esquemas una vez definidos los contratos del caso de estudio.
