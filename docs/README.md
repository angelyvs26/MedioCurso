# Documentación General del Proyecto (`docs/`)

Este directorio alberga la **documentación general, complementaria y de soporte** del proyecto `MedioCurso`.

---

## 1. Distinción entre `specs/` y `docs/`

Para evitar duplicaciones y mantener la integridad metodológica del proyecto, se establece la siguiente separación formal de responsabilidades:

| Espacio | Naturaleza | Propósito y Contenido |
|---|---|---|
| [`specs/`](file:///d:/Projects/MedioCurso/MedioCurso/specs) | **Especificaciones Activas y Vinculantes (Spec-First)** | Requisitos funcionales y no funcionales extraídos del caso de estudio, especificaciones de funcionalidades (Features), contratos de interfaz, esquemas de datos, desglose de tareas técnicas (`tasks/`), matrices de trazabilidad activa y plantillas reutilizables. **Es la fuente de verdad del software.** |
| [`docs/`](file:///d:/Projects/MedioCurso/MedioCurso/docs) | **Documentación General y Complementaria** | Documentación de contexto global, manuales del desarrollador, actas de acuerdos de alto nivel, registros fundacionales de arquitectura ([ADR-0001](file:///d:/Projects/MedioCurso/MedioCurso/docs/architecture/adrs/0001-baseline-architecture.md)) y material explicativo no normativo. |

---

## 2. Contenido de `docs/`

- **[`architecture/`](file:///d:/Projects/MedioCurso/MedioCurso/docs/architecture):** Documentación conceptual sobre la arquitectura monolítica limpia y el registro fundacional de arquitectura ([ADR 0001](file:///d:/Projects/MedioCurso/MedioCurso/docs/architecture/adrs/0001-baseline-architecture.md)).
- **[`requirements/`](file:///d:/Projects/MedioCurso/MedioCurso/docs/requirements), [`contracts/`](file:///d:/Projects/MedioCurso/MedioCurso/docs/contracts), [`planning/`](file:///d:/Projects/MedioCurso/MedioCurso/docs/planning), [`traceability/`](file:///d:/Projects/MedioCurso/MedioCurso/docs/traceability):** Guías introductorias iniciales que orientan al desarrollador sobre cómo se integran estos conceptos en el proyecto, sirviendo de referencia complementaria a los artefactos vivos en `specs/`.
