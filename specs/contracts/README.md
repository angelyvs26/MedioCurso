# Contratos de Interfaz y Comunicación (`specs/contracts/`)

Este directorio contiene los contratos formales de interfaz y esquemas de validación que regulan la interacción entre los clientes de **ABC Software Corp** (Windows/Android) y los servicios internos del monolito.

---

## 1. Documentos Principales

- **[`api-and-service-contracts.md`](file:///d:/Projects/MedioCurso/MedioCurso/specs/contracts/api-and-service-contracts.md):** Catálogo de contratos de interfaz externa HTTP (`CONT-001` a `CONT-006`) y contratos internos de servicios de aplicación (`CONT-INT-001` a `CONT-INT-003`).
- **[`schemas/dtos-and-schemas.md`](file:///d:/Projects/MedioCurso/MedioCurso/specs/contracts/schemas/dtos-and-schemas.md):** Especificación agnóstica de DTOs de entrada (`DTO-IN-XXX`), DTOs de salida (`DTO-OUT-XXX`), persistencia conceptual y correspondencias con el estándar OASIS UBL 2.1.
- **[`schemas/`](file:///d:/Projects/MedioCurso/MedioCurso/specs/contracts/schemas):** Directorio para definiciones estructuradas adicionales (JSON Schema / YAML).

---

## 2. Principios de Contratos

1. **Agnosticismo Tecnológico:** Especificación neutral que no impone un framework HTTP particular.
2. **Desacoplamiento Estricto:** Los DTOs de transporte se diferencian nítidamente de las entidades del dominio (`specs/domain/`) y de la representación XML UBL 2.1.
