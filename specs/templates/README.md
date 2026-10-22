# Catálogo y Guía de Uso de Plantillas SDD (`specs/templates/`)

Este directorio contiene las plantillas estandarizadas para formalizar cada uno de los artefactos requeridos por la metodología **Spec-Driven Development (SDD)** en su enfoque **Spec-First**.

---

## 1. Mapa de Plantillas y Cuándo Usar Cada Una

| Plantilla | Artefacto Generado | Ubicación de Destino | Cuándo Utilizarla |
|---|---|---|---|
| [`requirement-template.md`](file:///d:/Projects/MedioCurso/MedioCurso/specs/templates/requirement-template.md) | Requisito Funcional o No Funcional | `specs/requirements/functional/` o `specs/requirements/non-functional/` | Al extraer y descomponer una necesidad o regla explícita a partir del material del caso de estudio. |
| [`feature-spec-template.md`](file:///d:/Projects/MedioCurso/MedioCurso/specs/templates/feature-spec-template.md) | Especificación de Funcionalidad | `specs/features/` | Para describir en detalle el comportamiento, flujos, reglas invariantes y validaciones de una funcionalidad concreta antes de su codificación. |
| [`architecture-template.md`](file:///d:/Projects/MedioCurso/MedioCurso/specs/templates/architecture-template.md) | Documento de Arquitectura | `specs/architecture/` | Para documentar la vista estructural, la separación de responsabilidades del monolito MVC o vistas especializadas del sistema. |
| [`adr-template.md`](file:///d:/Projects/MedioCurso/MedioCurso/specs/templates/adr-template.md) | Registro de Decisión Arquitectónica | `specs/architecture/decisions/` | Al tomar decisiones técnicas significativas (selección de BD, lenguaje, framework, estrategia de autenticación). |
| [`contract-template.md`](file:///d:/Projects/MedioCurso/MedioCurso/specs/templates/contract-template.md) | Contrato de Interfaz o Esquema | `specs/contracts/` | Al definir formalmente el acuerdo de intercambio de datos (HTTP, CLI, o llamadas internas entre capas) y esquemas DTO. |
| [`task-plan-template.md`](file:///d:/Projects/MedioCurso/MedioCurso/specs/templates/task-plan-template.md) | Plan de Tarea de Implementación | `specs/planning/tasks/` | Para desglosar una funcionalidad validada en pasos ejecutables de programación y prueba dentro de `src/` y `tests/`. |
| [`test-plan-template.md`](file:///d:/Projects/MedioCurso/MedioCurso/specs/templates/test-plan-template.md) | Plan / Especificación de Pruebas | `specs/planning/` o `tests/` | Para definir escenarios verificables (unitarios, integración o e2e) alineados a criterios de aceptación. |
| [`traceability-matrix-template.md`](file:///d:/Projects/MedioCurso/MedioCurso/specs/templates/traceability-matrix-template.md) | Matriz de Trazabilidad | `specs/traceability/` | Para mantener la relación bidireccional entre Requisitos, Features, ADRs, Contratos, Tareas y Pruebas. |

---

## 2. Procedimiento para Copiar y Adaptar Plantillas

1. **Nunca editar directamente los archivos dentro de `specs/templates/`:** Estas plantillas son de sólo lectura y sirven como molde.
2. **Copiar al destino correspondiente:** Duplicar la plantilla en la carpeta correspondiente asignándole un nombre normalizado según la convención de IDs (ej. `copy specs/templates/feature-spec-template.md specs/features/FEAT-001-autenticacion-usuario.md`).
3. **Completar marcadores editables:** Sustituir los bloques `[...]` con los datos reales del caso de estudio.
4. **Respetar los marcadores de control:**
   - Si un dato aún no está decidido: usar `[Pendiente de definir]`.
   - Si una sección no aplica para el caso: usar `[No aplica, justificar]` con su debida explicación breve.
   - No eliminar secciones obligatorias.

---

## 3. Convención de Identificadores Únicos

Para garantizar la integridad y trazabilidad en todo el ciclo de vida, se debe usar la siguiente nomenclatura estricta:

- `RF-XXX`: Requisito Funcional (ej. `RF-001`, `RF-002`).
- `RNF-XXX`: Requisito No Funcional (ej. `RNF-001`).
- `FEAT-XXX`: Especificación de Funcionalidad (ej. `FEAT-001`).
- `ADR-XXXX`: Registro de Decisión Arquitectónica (ej. `ADR-0001`, `ADR-0002`).
- `CONT-XXX`: Contrato de Interfaz o Esquema (ej. `CONT-001`).
- `TASK-XXX`: Tarea de Implementación (ej. `TASK-001`).
- `TEST-XXX`: Plan o Caso de Prueba (ej. `TEST-001`).

---

## 4. Estrategia para Evitar Duplicación de Información (Single Source of Truth)

- **Referencias por ID:** En lugar de copiar textos largos de requisitos dentro de las especificaciones de tareas o contratos, referencia el identificador único (`[RF-001]`).
- **Separación de roles:** 
  - `specs/requirements/` contiene el *qué* necesita el negocio.
  - `specs/features/` contiene el *cómo se comporta* la solución desde la perspectiva funcional.
  - `specs/contracts/` contiene la *forma exacta* de los datos de entrada y salida.
  - `specs/architecture/` contiene la *estructura técnica y decisiones globales*.
  - `src/` contiene la *implementación ejecutable* derivada de los anteriores.
- **Actualización coherente:** Si un cambio en el caso de estudio modifica una regla de negocio, primero se actualiza el requisito (`RF-XXX`), luego la funcionalidad (`FEAT-XXX`) y su contrato (`CONT-XXX`), y finalmente el código y pruebas.
