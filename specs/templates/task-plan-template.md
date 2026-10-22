# Plantilla de Plan de Tarea de Implementación (Task Plan)

> **Instrucciones:** Copia este archivo dentro de `specs/planning/tasks/` bajo el nombre `[TASK-XXX-nombre-tarea].md` (o incorpóralo en un archivo centralizado `tasks.md`). Toda tarea debe derivarse rigurosamente de especificaciones previamente aprobadas. No planifiques tareas sobre requisitos inexistentes o ambiguos.

---

# [TASK-XXX]: [Nombre Descriptivo de la Tarea]

## 1. Identificación y Metadatos
- **Identificador Único:** `[TASK-XXX]`
- **Nombre:** `[Nombre de la tarea de implementación o verificación]`
- **Fase / Capa Asignada:** `[Dominio | Servicios | Infraestructura/Persistencia | Controladores/Vistas | Pruebas]`
- **Estado:** `[Por iniciar | En progreso | Bloqueada | Completada | Pendiente de definir]`
- **Estimación / Complejidad Relativa:** `[Baja | Media | Alta | Pendiente]`
- **Responsable:** `[Desarrollador / Rol / Agente IA]`

---

## 2. Objetivo de la Tarea
`[Definir de forma puntual qué se construirá o modificará en el código fuente o en la suite de pruebas mediante esta tarea.]`

---

## 3. Especificaciones de Origen (Trazabilidad)
- **Requisitos de Referencia:** `[RF-XXX, RNF-YYY]`
- **Funcionalidad (Feature):** `[FEAT-XXX]`
- **Decisión Arquitectónica de Referencia:** `[ADR-XXXX | No aplica]`
- **Contrato de Interfaz Implicado:** `[CONT-XXX | No aplica]`

---

## 4. Descripción Detallada del Trabajo a Realizar
1. `[Paso 1: Archivos a crear o modificar en src/...]`
2. `[Paso 2: Métodos, clases o interfaces a estructurar]`
3. `[Paso 3: Validaciones y lógica a aplicar]`
4. `[Paso 4: Creación de suite de pruebas correspondiente en tests/...]`

---

## 5. Dependencias Técnicas
- **Tareas Previas Requeridas:** `[TASK-YYY | Ninguna]`
- **Elementos Bloqueantes:** `[Decisión pendiente de framework/BD | Ninguno]`

---

## 6. Criterios de Finalización (Definition of Done)
- [ ] El código cumple con las directrices de Clean MVC (sin lógica de negocio en controladores ni SQL en el dominio).
- [ ] No existen dependencias circulares ni violaciones a la regla de dependencia.
- [ ] La especificación de la Feature (`FEAT-XXX`) se cumple en su totalidad.
- [ ] Se implementaron las pruebas correspondientes y pasan exitosamente al 100%.
- [ ] Se actualizó la matriz de trazabilidad en `specs/traceability/`.

---

## 7. Pruebas y Verificaciones Requeridas
- **Prueba Unitaria:** `[tests/unit/test_[nombre].ext]`
- **Prueba de Integración:** `[tests/integration/test_[nombre].ext | No aplica, justificar]`
- **Comando de Ejecución:** `[Comando de prueba | Pendiente de definir según lenguaje]`

---

## 8. Riesgos y Cuestiones Pendientes
- `[Riesgo técnico identificado o duda pendiente de confirmación]`
