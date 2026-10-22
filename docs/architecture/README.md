# Diseño Técnico y Decisiones Arquitectónicas (`docs/architecture/`)

Este directorio centraliza la documentación arquitectónica del proyecto, asegurando que las decisiones técnicas queden formalmente registradas antes de la fase de codificación.

---

## 1. Estructura del Directorio

- **`adrs/` (Architectural Decision Records):** Registro histórico inmutable de decisiones técnicas significativas, su contexto, alternativas evaluadas y consecuencias.
  - Convención: `NNNN-[titulo-en-kebab-case].md` (ej. `0001-baseline-architecture.md`).
- **`diagrams/`:** Diagramas visuales en formatos estándar (Mermaid, PlantUML, C4 Model o esquemas SVG/PNG):
  - Diagrama de Contexto del Sistema (C4 nivel 1).
  - Diagrama de Contenedores/Componentes (C4 nivel 2/3).
  - Modelo Conceptual de Dominio (Entidades y relaciones lógicas).

---

## 2. Principios de Diseño del Proyecto

1. **Monolito Modular:** Organización interna limpia sin acoplamiento no deseado.
2. **Clean MVC:**
   - La capa de presentación (`controllers/`, `views/`) no contiene lógica de negocio.
   - La capa de dominio (`domain/`) no depende de la base de datos ni de frameworks externos.
   - La capa de aplicación (`services/`) orquesta casos de uso entre dominio e infraestructura.
3. **Evolución Sostenible:** El diseño desacoplado facilita una transición natural hacia Arquitectura Hexagonal (Puertos y Adaptadores) o Clean Architecture si la complejidad del sistema crece.
