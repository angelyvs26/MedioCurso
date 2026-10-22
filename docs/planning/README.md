# Planificación de Tareas de Implementación (`docs/planning/`)

Este directorio aloja la planificación técnica y el desglose de tareas de desarrollo (`tasks.md`), asegurando que la fase de codificación se ejecute de manera ordenada, incremental y estrictamente alineada con las especificaciones.

---

## 1. Metodología de Planificación en SDD

En Spec-Driven Development, la planificación de tareas **nunca se realiza a partir de suposiciones informales**, sino directamente de:
1. Las historias de usuario y requisitos en `docs/requirements/`.
2. Los diagramas y decisiones de diseño en `docs/architecture/`.
3. Los contratos de API definidos en `docs/contracts/`.

---

## 2. Estructura Estándar de `tasks.md`

Cuando se reciba el caso de estudio, se creará el archivo `tasks.md` estructurado en fases incrementales:

- **Fase 1: Configuración de Dominio y Reglas Centrales**
  - Implementación de entidades puras y objetos de valor en `src/domain/`.
  - Pruebas unitarias de dominio en `tests/unit/`.
- **Fase 2: Servicios de Aplicación (Casos de Uso)**
  - Implementación de lógica de aplicación en `src/services/`.
  - Pruebas unitarias con mocks en `tests/unit/`.
- **Fase 3: Infraestructura y Persistencia**
  - Adaptadores de base de datos y repositorios en `src/infrastructure/persistence/`.
  - Pruebas de integración de persistencia en `tests/integration/`.
- **Fase 4: Controladores, Rutas y Vistas (MVC)**
  - Mapeo de rutas en `src/routes/`.
  - Controladores en `src/controllers/` y vistas en `src/views/`.
  - Pruebas de integración de endpoints en `tests/integration/`.
- **Fase 5: Verificación End-to-End**
  - Pruebas e2e en `tests/e2e/` contra los criterios de aceptación iniciales.

---

## 3. Estado Actual

> **Pendiente:** Redacción del plan de tareas detallado tras la recepción del caso de estudio.
