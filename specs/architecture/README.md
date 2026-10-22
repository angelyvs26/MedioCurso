# Especificación Arquitectónica del Sistema (`specs/architecture/`)

Este directorio documenta la arquitectura técnica del proyecto `MedioCurso` para **ABC Software Corp**, detallando la estructura monolítica limpia basada en el patrón MVC y su proyección hacia Arquitectura Hexagonal.

---

## 1. Documentos Principales

- **[`mvc-and-hexagonal-architecture.md`](file:///d:/Projects/MedioCurso/MedioCurso/specs/architecture/mvc-and-hexagonal-architecture.md):** Especificación arquitectónica integrada (`ARC-001`) con diagramas Mermaid del flujo MVC monolítico, estrategia de puertos/adaptadores hexagonales y análisis de restricciones de plataforma Windows (MSI) y Android (APK).
- **[`diagrams/`](file:///d:/Projects/MedioCurso/MedioCurso/specs/architecture/diagrams):** Modelos visuales, incluyendo el diagrama conceptual del dominio ([`domain-model-diagram.md`](file:///d:/Projects/MedioCurso/MedioCurso/specs/architecture/diagrams/domain-model-diagram.md)).
- **[`decisions/`](file:///d:/Projects/MedioCurso/MedioCurso/specs/architecture/decisions):** Registro inmutable de Decisiones Arquitectónicas (ADRs).

---

## 2. Guardarraíles Arquitectónicos del Proyecto

1. **Monolito Cohesionado:** Todo el núcleo reside en una sola unidad en `src/`. No introducir microservicios de forma prematura.
2. **Separación Estricta MVC:**
   - **Presentación (`controllers/`, `views/`):** Manejo exclusivo de interacción con el cliente (Windows/Android) y formateo de respuesta. Prohibido insertar lógica de negocio o sentencias SQL/ORM directas.
   - **Dominio (`domain/`):** Entidades y reglas invariantes completamente independientes de frameworks y bases de datos.
   - **Servicios de Aplicación (`services/`):** Casos de uso que orquestan las operaciones entre el dominio y las abstracciones técnicas.
   - **Infraestructura (`infrastructure/`):** Implementaciones de persistencia (`persistence/`), validación XSD UBL (`tradeshift-ubl-xsd`) y clientes de comunicación con SUNAT (`external/`).
3. **Facilitación de Evolución Hexagonal:** Puertos de entrada y salida desacoplados que permiten reemplazar adaptadores sin tocar el dominio.
