# Matrices de Trazabilidad (`docs/traceability/`)

Este directorio contiene las matrices y herramientas que garantizan la trazabilidad bidireccional entre todas las etapas del ciclo de vida del software, desde la concepción del requisito hasta su prueba automatizada.

---

## 1. Modelo de Trazabilidad SDD

Cada elemento del sistema debe mantener una relación explícita según el siguiente flujo:

$$\text{Requisito Funcional (RF)} \longleftrightarrow \text{Diseño / ADR} \longleftrightarrow \text{Contrato API} \longleftrightarrow \text{Tarea de Planificación} \longleftrightarrow \text{Prueba de Verificación}$$

### Estructura de la Matriz (Plantilla de Referencia)

| ID Requisito | Descripción | Componente / Caso de Uso | Contrato API | Tarea Planificada | Pruebas Unitarias / Integración / E2E | Estado |
|---|---|---|---|---|---|---|
| *RF-001* | *Pendiente caso de estudio* | *Servicio / Dominio* | *Contrato YAML* | *TASK-001* | *tests/...* | *Por especificar* |

---

## 2. Beneficios

- **Cero código no especificado:** Previene la inclusión de funcionalidades no documentadas ("gold plating").
- **Evaluación de impacto inmediata:** Si un requisito cambia, la matriz indica con exactitud qué contratos, archivos de código y pruebas deben modificarse.
- **Auditoría de cobertura:** Asegura que ningún requisito quede sin su respectiva prueba automatizada.
