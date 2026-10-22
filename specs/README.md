# Espacio de Especificaciones SDD (Spec-First) - `specs/`

Este directorio constituye la **fuente de referencia principal y vinculante** para todo el ciclo de vida del software en el proyecto `MedioCurso` de **ABC Software Corp**, operando bajo la metodología **Spec-Driven Development (SDD)** con un enfoque **Spec-First**.

---

## 1. Principio Fundamental: Spec-First

En este repositorio, **ninguna línea de código de producción ni prueba automatizada se escribe sin una especificación previa aprobada y trazada**. 

El espacio `specs/` gobierna el desarrollo del sistema, asegurando que:
- Los requisitos provengan fielmente de las normas tributarias de la SUNAT (RS 097-2012/SUNAT) y del caso de estudio de ABC Software Corp.
- Los comportamientos, reglas invariantes de cálculo y contratos de interfaz queden formalizados antes de codificar.
- La arquitectura monolítica MVC mantenga una separación estricta de responsabilidades, facilitando una futura evolución hacia arquitectura hexagonal o Clean Architecture.

---

## 2. Flujo de Trabajo SDD Spec-First

```text
┌────────────────────────┐
│ 1. Caso de Estudio     │  specs/case-study/ (incorporación de partes y fuentes SUNAT/UBL)
└───────────┬────────────┘
            ▼
┌────────────────────────┐
│ 2. Análisis y Dudas    │  Aclaraciones, preguntas abiertas y supuestos explícitos
└───────────┬────────────┘
            ▼
┌────────────────────────┐
│ 3. Modelo de Dominio   │  specs/domain/ (entidades, objetos de valor, agregados, glosario)
└───────────┬────────────┘
            ▼
┌────────────────────────┐
│ 4. Requisitos / Feat   │  specs/requirements/ y specs/features/
└───────────┬────────────┘
            ▼
┌────────────────────────┐
│ 5. Arquitectura y      │  specs/architecture/ (diagramas MVC, hexagonal y ADRs)
│    Contratos           │  specs/contracts/ (interfaces HTTP, contratos internos y DTOs)
└───────────┬────────────┘
            ▼
┌────────────────────────┐
│ 6. Planificación       │  specs/planning/tasks/ (desglose de tareas técnicas)
└───────────┬────────────┘
            ▼
┌────────────────────────┐
│ 7. Implementación      │  src/ (Clean MVC: controllers, views, domain, services, infra)
└───────────┬────────────┘
            ▼
┌────────────────────────┐
│ 8. Pruebas             │  tests/ (unitarias, integración, e2e contra criterios de aceptación)
└───────────┬────────────┘
            ▼
┌────────────────────────┐
│ 9. Trazabilidad        │  specs/traceability/ (matriz normativa SUNAT–UBL 2.1)
└────────────────────────┘
```

---

## 3. Estructura Interna de `specs/`

```text
specs/
├── README.md                      # Este documento (marco metodológico y flujo SDD)
├── case-study/                    # Material bruto y partes del caso de estudio provisto
├── templates/                     # Plantillas normalizadas para todos los artefactos SDD
├── domain/                        # Modelo de dominio puro, entidades, agregados y glosario
│   ├── README.md
│   └── domain-model.md
├── requirements/                  # Requisitos catalogados y clasificados
│   ├── functional/                # Requisitos funcionales (RF-XXX)
│   └── non-functional/            # Requisitos no funcionales y atributos de calidad (RNF-XXX)
├── features/                      # Especificaciones detalladas de funcionalidades (FEAT-XXX)
├── architecture/                  # Documentación de diseño técnico y decisiones
│   ├── README.md
│   ├── mvc-and-hexagonal-architecture.md  # Especificación ARC-001
│   ├── diagrams/                  # Diagramas Mermaid (dominio, MVC, hexagonal)
│   │   ├── README.md
│   │   └── domain-model-diagram.md
│   └── decisions/                 # Registros inmutables de decisiones arquitectónicas (ADRs)
│       ├── README.md
│       └── 0002-plataformas-distribucion-cliente.md
├── contracts/                     # Contratos de interfaces HTTP e internas
│   ├── README.md
│   ├── api-and-service-contracts.md
│   └── schemas/                   # DTOs de entrada/salida, esquemas y mapeo UBL 2.1
│       ├── README.md
│       └── dtos-and-schemas.md
├── planning/                      # Estrategia de planificación
│   └── tasks/                     # Tareas detalladas de codificación y verificación (TASK-XXX)
└── traceability/                  # Matrices y auditoría de cobertura integral
    ├── README.md
    └── sunat-ubl-traceability-matrix.md   # Matriz normativa principal SUNAT - UBL 2.1
```

---

## 4. Estado Actual: Fase 2 Completada

Se ha consolidado la primera versión integrada de los cuatro entregables técnicos:
1. **Modelo de Dominio y Contratos de Datos:** Documentados en [`specs/domain/domain-model.md`](file:///d:/Projects/MedioCurso/MedioCurso/specs/domain/domain-model.md) y [`specs/contracts/schemas/dtos-and-schemas.md`](file:///d:/Projects/MedioCurso/MedioCurso/specs/contracts/schemas/dtos-and-schemas.md).
2. **Diagramas de Arquitectura MVC y Evolución Hexagonal:** Documentados en [`specs/architecture/mvc-and-hexagonal-architecture.md`](file:///d:/Projects/MedioCurso/MedioCurso/specs/architecture/mvc-and-hexagonal-architecture.md).
3. **Contratos API y Especificación de Servicios:** Documentados en [`specs/contracts/api-and-service-contracts.md`](file:///d:/Projects/MedioCurso/MedioCurso/specs/contracts/api-and-service-contracts.md).
4. **Matriz de Trazabilidad Normativa SUNAT–UBL:** Documentada en [`specs/traceability/sunat-ubl-traceability-matrix.md`](file:///d:/Projects/MedioCurso/MedioCurso/specs/traceability/sunat-ubl-traceability-matrix.md).
