# Planificación y Desglose de Tareas Técnicas (`specs/planning/`)

Este directorio organiza la planificación operativa del desarrollo derivada directamente de las especificaciones y contratos validados.

---

## 1. Organización del Directorio

- **[`tasks/`](file:///d:/Projects/MedioCurso/MedioCurso/specs/planning/tasks):** Fichas y planes de tareas técnicas detalladas (`TASK-XXX-[nombre].md`) generadas a partir de [`specs/templates/task-plan-template.md`](file:///d:/Projects/MedioCurso/MedioCurso/specs/templates/task-plan-template.md).
- **`tasks.md` (Consolidado Futuro):** Roadmap o lista maestra de tareas ordenadas por fases incrementales de entrega.

---

## 2. Regla Fundamental de Planificación en SDD

> **Ninguna tarea se planifica en el vacío.** Cada tarea de implementación en `tasks/` debe estar explícitamente ligada a una funcionalidad aprobada (`specs/features/FEAT-XXX.md`) y a sus criterios de aceptación.

El orden recomendado para la planificación de tareas por funcionalidad es:
1. Modelado de Dominio y pruebas unitarias (`src/domain/`, `tests/unit/`).
2. Servicios de Aplicación y casos de uso (`src/services/`, `tests/unit/`).
3. Persistencia y adaptadores de infraestructura (`src/infrastructure/`, `tests/integration/`).
4. Controladores, vistas y rutas (`src/controllers/`, `src/views/`, `src/routes/`).
5. Verificación de integración y extremo a extremo (`tests/integration/`, `tests/e2e/`).

---

## 3. Estado Actual

> **Pendiente:** Planificación detallada una vez que se cuente con las especificaciones de funcionalidades del caso de estudio.
