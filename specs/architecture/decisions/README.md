# Registro de Decisiones Arquitectónicas - ADRs (`specs/architecture/decisions/`)

Este directorio contiene el registro inmutable de las decisiones arquitectónicas adoptadas y propuestas para el proyecto `MedioCurso` de **ABC Software Corp**.

---

## 1. Convenciones y Flujo de Aprobación

- **Nomenclatura:** `NNNN-[titulo-en-kebab-case].md` (ej. `0002-plataformas-distribucion-cliente.md`).
- **Plantilla Base:** Copiar [`specs/templates/adr-template.md`](file:///d:/Projects/MedioCurso/MedioCurso/specs/templates/adr-template.md).
- **Control de Estados:**
  - `Propuesto`: Decisión en análisis técnico.
  - `Aceptado`: Decisión formalmente consensuada y aprobada.
  - `Rechazado`: Decisión descartada tras evaluación.

---

## 2. Índice de Decisiones Registradas

| ID | Título | Estado | Fecha | Requisitos Asociados |
|---|---|---|---|---|
| [ADR-0001](file:///d:/Projects/MedioCurso/MedioCurso/docs/architecture/adrs/0001-baseline-architecture.md) | Adopción de Arquitectura Monolítica Limpia MVC y Metodología Spec-First SDD | Aceptado | 2026-10-22 | Fundacional |
| [ADR-0002](file:///d:/Projects/MedioCurso/MedioCurso/specs/architecture/decisions/0002-plataformas-distribucion-cliente.md) | Estrategia Arquitectónica para Distribución Multiplataforma (Windows MSI y Android APK) | Propuesto | 2026-10-22 | RNF-001, RNF-002, RF-001 |
