# Plantilla de Especificación de Requisito

> **Instrucciones:** Copia este archivo al subdirectorio correspondiente (`specs/requirements/functional/` para requisitos funcionales o `specs/requirements/non-functional/` para requisitos no funcionales). Sustituye los marcadores entre corchetes `[...]` por la información extraída del caso de estudio. No inventes requisitos ni asumas información no especificada.

---

## [ID-REQUISITO]: [Nombre del Requisito]

### 1. Metadatos del Requisito
- **Identificador Único:** `[RF-XXX | RNF-XXX]`
- **Nombre Descriptivo:** `[Nombre conciso y representativo]`
- **Tipo de Requisito:** `[Funcional | No Funcional (Rendimiento / Seguridad / Usabilidad / Mantenibilidad) | Restricción Técnica]`
- **Prioridad:** `[Alta | Media | Baja | Pendiente de definir]`
- **Estado:** `[Borrador | En revisión | Aprobado | Deprecado | Pendiente de definir]`
- **Fecha de Registro:** `[AAAA-MM-DD | Pendiente de definir]`
- **Autor / Analista:** `[Nombre o Rol]`

---

### 2. Descripción General
`[Descripción clara, no ambigua y objetiva de la necesidad o capacidad que el sistema debe satisfacer. Explicar qué debe hacer el sistema, no cómo implementarlo tecnológicamente.]`

---

### 3. Justificación y Fuente del Caso de Estudio
- **Fuente:** `[Caso de estudio - Parte X, Sección Y, Párrafo Z | Referencia documental]`
- **Cita / Evidencia Textual:**
  > "[Texto literal o extracto relevante del caso de estudio que origina este requisito]"
- **Justificación de Negocio:** `[Por qué es necesario este requisito y qué valor aporta]`

---

### 4. Criterios de Aceptación Verificables
Cada criterio debe ser medible y comprobable mediante pruebas automatizadas o de aceptación.

#### Criterio 1: `[Título del criterio]`
- **Dado:** `[Precondición o estado inicial del sistema]`
- **Cuando:** `[Acción ejecutada o evento que ocurre]`
- **Entonces:** `[Resultado esperado o cambio de estado observable]`

#### Criterio 2: `[Título del criterio alternativo o de excepción]`
- **Dado:** `[Precondición]`
- **Cuando:** `[Acción con datos inválidos o condición de borde]`
- **Entonces:** `[Respuesta de validación o manejo de error esperado]`

---

### 5. Dependencias y Restricciones
- **Requisitos Previos / Dependencias:** `[IDs de otros requisitos que deben cumplirse antes, o 'Ninguna']`
- **Restricciones Técnicas u Operativas:** `[Limitaciones de entorno, legales, de plataforma o 'Pendiente de definir']`

---

### 6. Relaciones de Trazabilidad
- **Funcionalidades Vinculadas (Features):** `[FEAT-XXX | Pendiente de especificar]`
- **Contratos de Interfaz Relacionados:** `[CONT-XXX | No aplica, justificar]`
- **Decisiones Arquitectónicas Relacionadas:** `[ADR-XXXX | No aplica, justificar]`
- **Pruebas de Verificación:** `[TEST-XXX | Pendiente de planificar]`

---

### 7. Dudas, Supuestos e Información Pendiente
- **Dudas Abiertas:** `[Aspectos ambiguos del enunciado que requieren aclaración con el profesor/cliente]`
- **Supuestos Explícitos:** `[Suposición provisional formulada con su justificación; marcar si requiere confirmación]`
