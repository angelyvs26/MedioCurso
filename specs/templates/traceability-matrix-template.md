# Plantilla de Matriz de Trazabilidad Integral (SDD)

> **Instrucciones:** Copia este archivo dentro de `specs/traceability/matrix.md`. Actualiza las filas a medida que se formalicen los requisitos, especificaciones, contratos, tareas y pruebas a partir del caso de estudio. No declares cobertura ni estados de prueba superados si aún no han sido verificados.

---

# Matriz de Trazabilidad de Requisitos y Artefactos

## 1. Convención de Identificadores
- **RF-XXX:** Requisito Funcional
- **RNF-XXX:** Requisito No Funcional
- **FEAT-XXX:** Funcionalidad (Feature Spec)
- **ADR-XXXX:** Registro de Decisión Arquitectónica
- **CONT-XXX:** Contrato de Interfaz / Esquema de Datos
- **TASK-XXX:** Tarea de Implementación
- **TEST-XXX:** Plan / Caso de Prueba Automatizado

---

## 2. Matriz General de Cobertura y Trazabilidad

| ID Requisito | Nombre del Requisito | Funcionalidad (Feature) | Decisión Arq. (ADR) | Contrato de Interfaz | Tarea de Impl. (Task) | Suite de Pruebas (Tests) | Evidencia de Verificación | Estado de Cobertura |
|---|---|---|---|---|---|---|---|---|
| `[RF-001]` | `[Nombre]` | `[FEAT-001]` | `[ADR-0001]` | `[CONT-001]` | `[TASK-001]` | `[TEST-001 (unit/integ)]` | `[Reporte / Logs]` | `[Pendiente de definir]` |
| `[RF-002]` | `[Nombre]` | `[FEAT-002]` | `[No aplica]` | `[CONT-002]` | `[TASK-002]` | `[TEST-002 (e2e)]` | `[Reporte / Logs]` | `[Pendiente de definir]` |
| `[RNF-001]` | `[Nombre RNF]` | `[FEAT-Global]` | `[ADR-0001]` | `[No aplica]` | `[TASK-003]` | `[TEST-003]` | `[Métricas de rendimiento]` | `[Pendiente de definir]` |

---

## 3. Estado de Cobertura por Fase

| Fase del Flujo SDD | Total Artefactos | Completados | En Progreso | Pendientes |
|---|---|---|---|---|
| **Requisitos (RF / RNF)** | 0 | 0 | 0 | 0 *(A la espera del caso de estudio)* |
| **Funcionalidades (Features)** | 0 | 0 | 0 | 0 |
| **Decisiones (ADRs)** | 1 *(ADR-0001)* | 1 | 0 | *(Decisiones técnicas pendientes)* |
| **Contratos (Contracts)** | 0 | 0 | 0 | 0 |
| **Tareas (Tasks)** | 0 | 0 | 0 | 0 |
| **Pruebas (Tests)** | 0 | 0 | 0 | 0 |

---

## 4. Verificación de Integridad
- **Requisitos Huérfanos (sin Feature ni Tarea):** Ninguno detectado.
- **Código Huérfano (código en `src/` sin Requisito asociado):** Prohibido por directrices SDD.
- **Pruebas Faltantes (Requisitos sin Pruebas asociadas):** Se registrarán cuando existan especificaciones aprobadas.
