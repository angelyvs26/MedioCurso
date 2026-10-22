# Directrices Vinculantes para Agentes de IA (AGENTS.md)

Este repositorio opera bajo la metodología **Spec-Driven Development (SDD)** con un enfoque **Spec-First** y una **Arquitectura Monolítica Limpia basada en MVC**.

Cualquier agente de IA o desarrollador que interactúe con este proyecto debe acatar estrictamente las siguientes reglas operativas:

---

## 1. Reglas Fundamentales de Operación del Agente (SDD Spec-First)

1. **Consulta Previa Obligatoria:** Consultar siempre [`specs/README.md`](file:///d:/Projects/MedioCurso/MedioCurso/specs/README.md) y las instrucciones SDD antes de iniciar cualquier labor.
2. **Revisión de Especificaciones Existentes:** Revisar minuciosamente las especificaciones existentes en `specs/` antes de proponer o crear documentos nuevos, evitando duplicidades.
3. **Uso Obligatorio de Plantillas Estandarizadas:** Utilizar exclusivamente la plantilla correspondiente de [`specs/templates/`](file:///d:/Projects/MedioCurso/MedioCurso/specs/templates) para cada nueva especificación, requisito, arquitectura, ADR, contrato, tarea o prueba.
4. **Especificaciones como Fuente de Verdad:** Mantener permanentemente las especificaciones de `specs/` como la única fuente de referencia vinculante para el desarrollo.
5. **Bloqueo de Implementación por Ambigüedad:** Prohibido comenzar la implementación en código (`src/`) de una funcionalidad si su especificación presenta contradicciones, dudas abiertas o información crítica sin resolver.
6. **Explicitación de Supuestos y Dudas:** Identificar y documentar explícitamente cualquier supuesto de trabajo y formular con claridad las preguntas pendientes dirigidas al docente/cliente.
7. **Prohibición de Invención de Requisitos:** No inventar requisitos, módulos, entidades ni presentar suposiciones o propuestas como decisiones técnicas ya aprobadas.
8. **Identificadores Únicos y Trazabilidad:** Mantener identificadores únicos normalizados (`RF-XXX`, `RNF-XXX`, `FEAT-XXX`, `ADR-XXXX`, `CONT-XXX`, `TASK-XXX`, `TEST-XXX`) y registrar rigurosamente sus enlaces en [`specs/traceability/`](file:///d:/Projects/MedioCurso/MedioCurso/specs/traceability).
9. **Sincronización y No Inconsistencia:** Actualizar oportunamente las especificaciones cuando cambien los requisitos o se adopten nuevas decisiones, evitando discrepancias entre artefactos.
10. **Separación de Responsabilidades y Espacios:** Mantener estrictamente separadas las especificaciones (`specs/`), la documentación general y complementaria (`docs/`), el código fuente (`src/`) y las pruebas automatizadas (`tests/`).
11. **Eficiencia y Calidad Documental:** No generar automáticamente grandes volúmenes de documentación genérica o repetitiva si no aporta valor real al proyecto.
12. **Respeto a Restricciones Tecnológicas Posteriores:** Respetar los lenguajes, frameworks, bases de datos y directrices técnicas que se definan una vez recibido y validado el caso de estudio.

---

## 2. Flujo de Trabajo Obligatorio

$$\text{Caso de Estudio} \rightarrow \text{Análisis/Dudas} \rightarrow \text{Requisitos} \rightarrow \text{Features} \rightarrow \text{Arquitectura/Contratos} \rightarrow \text{Tareas} \rightarrow \text{Implementación} \rightarrow \text{Pruebas} \rightarrow \text{Trazabilidad}$$

- La implementación en `src/` sólo puede iniciarse cuando la especificación (`specs/features/`) y el contrato (`specs/contracts/`) de la funcionalidad estén definidos, verificables y coherentes.
- No es necesario esperar a que todo el sistema esté especificado para implementar una funcionalidad independiente con alcance cerrado.

---

## 3. Guardarraíles de Arquitectura Monolítica Limpia (Clean MVC)

1. **Monolito Cohesionado:** Toda la funcionalidad debe residir en `src/`. No introducir microservicios ni desacoplamientos de red prematuros.
2. **Separación de Capas:**
   - **Controladores (`src/controllers/`):** Recepción de entrada (HTTP/CLI), validación de transporte y delegación a servicios. **Prohibido incluir lógica de negocio o consultas SQL/ORM directas.**
   - **Vistas (`src/views/`):** Formateo de salida o renderizado. Sin lógica de negocio ni acceso a datos.
   - **Dominio (`src/domain/`):** Entidades puras y reglas invariantes. **Totalmente libre de dependencias de frameworks o bases de datos.**
   - **Servicios de Aplicación (`src/services/`):** Orquestación de casos de uso entre dominio e infraestructura.
   - **Infraestructura (`src/infrastructure/`):** Persistencia concreta (`persistence/`) y clientes externos (`external/`).
3. **Regla de Dependencia:** Las dependencias del código fuente siempre apuntan hacia el interior: Presentación/Infraestructura $\rightarrow$ Servicios $\rightarrow$ Dominio.

---

## 4. Estado Actual del Proyecto

- **Fase:** Fase 1 (Configuración de Estructura SDD Spec-First).
- **Caso de estudio:** Pendiente de recepción. Prohibido inventar entidades o implementar código de negocio.
