# Reglas de Agente para Spec-Driven Development (SDD Spec-First)

Este archivo define las reglas de comportamiento activo para agentes en el entorno Antigravity:

1. **Prioridad Spec-First:** Consultar `specs/README.md` antes de actuar. Las especificaciones en `specs/` son la fuente vinculante de verdad.
2. **Uso de Plantillas:** Toda nueva especificación debe originarse a partir de las plantillas en `specs/templates/`.
3. **Control de Ambigüedad:** No codificar ninguna funcionalidad que presente dudas abiertas, contradicciones o supuestos sin validar.
4. **No Invención:** Queda estrictamente prohibido inventar requisitos, módulos, entidades o reglas de negocio no presentes en el caso de estudio.
5. **Aislamiento Arquitectónico MVC Limpio:**
   - `src/controllers/` y `src/views/`: Exclusivamente presentación y transporte. Prohibido ejecutar lógica de negocio o consultas SQL/ORM.
   - `src/domain/`: Reglas de negocio puras, sin dependencias de frameworks ni de infraestructura.
   - `src/services/`: Orquestación de casos de uso de negocio.
   - `src/infrastructure/`: Adaptadores de base de datos (`persistence/`) y clientes externos (`external/`).
6. **Trazabilidad Continua:** Mantener actualizadas las matrices en `specs/traceability/` asegurando el enlace Requisito ↔ Feature ↔ Contrato ↔ Tarea ↔ Prueba.
7. **Separación de Espacios:**
   - `specs/`: Especificaciones activas y vivas del sistema.
   - `docs/`: Documentación general, guías complementarias y registros fundacionales.
   - `src/`: Código fuente de producción.
   - `tests/`: Batería de pruebas automatizadas.
8. **No Duplicación:** No generar documentación repetitiva ni sobredimensionada que no aporte valor.
9. **Respeto Tecnológico:** Respetar los lenguajes, frameworks y bases de datos que se formalicen tras recibir el caso de estudio.
