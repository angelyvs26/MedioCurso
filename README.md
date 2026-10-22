# Proyecto MedioCurso - Arquitectura Monolítica Limpia MVC & SDD Spec-First

Este repositorio aloja la solución de software para el proyecto **MedioCurso**, desarrollado bajo la metodología **Spec-Driven Development (SDD)** con un estricto enfoque **Spec-First** y una **Arquitectura Monolítica Limpia basada en el patrón Modelo-Vista-Controlador (MVC)**.

---

## 1. Filosofía de Desarrollo

### 1.1. Enfoque Spec-First (SDD)
El principio rector del proyecto es que **la especificación precede invariablemente a la implementación**. Ningún módulo, interfaz, entidad ni prueba de integración se codifica en `src/` o `tests/` sin que existan previamente especificaciones funcionales, contratos de interfaz y tareas aprobadas en el espacio vinculante [`specs/`](file:///d:/Projects/MedioCurso/MedioCurso/specs).

### 1.2. Arquitectura Monolítica Limpia (Clean MVC)
El sistema se concibe como un monolito modular cohesionado, evitando la complejidad innecesaria de microservicios prematuros. A diferencia de las implementaciones tradicionales de MVC donde la lógica suele acoplarse a controladores o modelos ORM, aquí se implementa una separación limpia de capas:
- **Capa de Dominio (`src/domain/`):** Contiene las entidades puras y las reglas de negocio invariantes, completamente aisladas de frameworks, bases de datos o mecanismos de transporte.
- **Capa de Servicios de Aplicación (`src/services/`):** Orquesta los casos de uso del sistema coordinando el dominio con abstracciones técnicas.
- **Capa de Presentación (`src/controllers/`, `src/views/`, `src/routes/`):** Maneja la entrada/salida de datos, mediando con los clientes HTTP/CLI sin contener lógica de negocio.
- **Capa de Infraestructura (`src/infrastructure/`):** Implementa el acceso a datos concreto (`persistence/`) y clientes externos (`external/`).
- **Preparación para Evolución Hexagonal:** Esta separación permite que el sistema pueda transicionar de manera natural hacia **Arquitectura Hexagonal (Puertos y Adaptadores)** o **Clean Architecture** si los requerimientos de escala futura lo exigen, manteniendo el núcleo intacto.

---

## 2. Flujo de Trabajo SDD Spec-First

El desarrollo del proyecto avanza de forma secuencial y trazable:

$$\text{Caso de Estudio} \rightarrow \text{Análisis y Aclaraciones} \rightarrow \text{Requisitos} \rightarrow \text{Especificaciones de Funcionalidades} \rightarrow \text{Diseño Arquitectónico y Contratos} \rightarrow \text{Planificación de Tareas} \rightarrow \text{Implementación} \rightarrow \text{Pruebas} \rightarrow \text{Verificación de Trazabilidad}$$

Una funcionalidad puede comenzar su fase de implementación en `src/` en cuanto su especificación en `specs/features/` y su contrato en `specs/contracts/` estén completamente verificados y cerrados, sin requerir que la totalidad de los requisitos globales estén finalizados.

---

## 3. Organización de Espacios del Repositorio

El repositorio divide claramente sus responsabilidades en cuatro directorios principales:

| Directorio | Propósito Principal |
|---|---|
| [`specs/`](file:///d:/Projects/MedioCurso/MedioCurso/specs) | **Espacio Principal de Especificaciones SDD (Fuente de Verdad):** Contiene el caso de estudio original, el catálogo de plantillas oficiales, requisitos clasificados (`functional/`, `non-functional/`), especificaciones de funcionalidades (`features/`), diseño arquitectónico y ADRs (`architecture/`), contratos de interfaz y esquemas DTO (`contracts/`), desglose de tareas técnicas (`planning/tasks/`) y matrices de trazabilidad (`traceability/`). |
| [`src/`](file:///d:/Projects/MedioCurso/MedioCurso/src) | **Código Fuente de la Aplicación Monolítica Clean MVC:** Aloja la implementación en producción dividida limpiamente en `config/`, `controllers/`, `views/`, `domain/`, `services/`, `infrastructure/` y `routes/`. |
| [`tests/`](file:///d:/Projects/MedioCurso/MedioCurso/tests) | **Suite de Pruebas Automatizadas:** Aloja las baterías de pruebas unitarias (`unit/`), de integración (`integration/`) y extremo a extremo (`e2e/`), vinculadas directamente a los criterios de aceptación de las especificaciones. |
| [`docs/`](file:///d:/Projects/MedioCurso/MedioCurso/docs) | **Documentación General y Complementaria:** Contiene material complementario de soporte, guías conceptuales del proyecto, decisiones fundacionales de arquitectura ([ADR-0001](file:///d:/Projects/MedioCurso/MedioCurso/docs/architecture/adrs/0001-baseline-architecture.md)) y documentación explicativa no normativa. |

Adicionalmente, [`AGENTS.md`](file:///d:/Projects/MedioCurso/MedioCurso/AGENTS.md) y [`.agents/rules/sdd-rules.md`](file:///d:/Projects/MedioCurso/MedioCurso/.agents/rules/sdd-rules.md) establecen las directrices operativas que rigen el comportamiento de los asistentes de IA en el workspace.

---

## 4. Estructura General del Proyecto

```text
MedioCurso/
├── .agents/
│   └── rules/
│       └── sdd-rules.md
├── AGENTS.md
├── README.md
├── docs/
│   ├── README.md
│   ├── architecture/
│   │   ├── README.md
│   │   ├── adrs/
│   │   │   └── 0001-baseline-architecture.md
│   │   └── diagrams/
│   ├── contracts/
│   │   └── README.md
│   ├── planning/
│   │   └── README.md
│   ├── requirements/
│   │   └── README.md
│   └── traceability/
│       └── README.md
├── specs/
│   ├── README.md
│   ├── case-study/
│   │   └── README.md
│   ├── templates/
│   │   ├── README.md
│   │   ├── requirement-template.md
│   │   ├── feature-spec-template.md
│   │   ├── architecture-template.md
│   │   ├── adr-template.md
│   │   ├── contract-template.md
│   │   ├── task-plan-template.md
│   │   ├── test-plan-template.md
│   │   └── traceability-matrix-template.md
│   ├── requirements/
│   │   ├── README.md
│   │   ├── functional/
│   │   │   └── README.md
│   │   └── non-functional/
│   │       └── README.md
│   ├── features/
│   │   └── README.md
│   ├── architecture/
│   │   ├── README.md
│   │   ├── diagrams/
│   │   └── decisions/
│   │       └── README.md
│   ├── contracts/
│   │   ├── README.md
│   │   └── schemas/
│   │       └── README.md
│   ├── planning/
│   │   ├── README.md
│   │   └── tasks/
│   │       └── README.md
│   └── traceability/
│       └── README.md
├── src/
│   ├── config/
│   ├── controllers/
│   ├── domain/
│   ├── infrastructure/
│   │   ├── external/
│   │   └── persistence/
│   ├── routes/
│   ├── services/
│   └── views/
└── tests/
    ├── e2e/
    ├── integration/
    └── unit/
```

---

## 5. Decisiones Pendientes (Bloqueadas por el Caso de Estudio)

A la espera del enunciado definitivo del caso de estudio, permanecen formalmente pendientes:
1. **Lenguaje y Entorno de Ejecución:** Selección del stack base.
2. **Framework Monolítico:** Selección del framework compatible con Clean MVC.
3. **Motor de Persistencia y Base de Datos:** Relacional vs documental.
4. **Mecanismo de Renderizado / Presentación:** SSR, REST API JSON o CLI.
5. **Esquema de Autenticación y Autorización:** Roles y seguridad.
6. **Entidades, Módulos y Reglas de Negocio:** Delimitación de dominios y servicios específicos.
