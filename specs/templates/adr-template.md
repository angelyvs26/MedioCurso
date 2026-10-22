# Plantilla de Registro de Decisión Arquitectónica (ADR)

> **Instrucciones:** Copia este archivo dentro de `specs/architecture/decisions/` con el formato `NNNN-[titulo-en-kebab-case].md`. Registra el análisis técnico de decisiones significativas. Si una decisión está aún bajo evaluación, márcala como `Propuesto` o `En evaluación`; no declares una decisión como `Aceptado` sin aprobación explícita.

---

# [ADR-XXXX]: [Título Descriptivo de la Decisión]

## 1. Metadatos de la Decisión
- **Identificador Único:** `[ADR-XXXX]` (ej. `ADR-0002`)
- **Fecha de Propuesta:** `[AAAA-MM-DD | Pendiente de definir]`
- **Fecha de Decisión:** `[AAAA-MM-DD | Pendiente de definir]`
- **Estado:** `[Propuesto | En evaluación | Aceptado | Rechazado | Obsoleto | Superado por ADR-YYYY]`
- **Autores / Decisores:** `[Nombres o Roles involucrados]`
- **Requisitos Relacionados:** `[RF-XXX, RNF-YYY | Pendiente de definir]`

---

## 2. Contexto y Planteamiento del Problema
`[Describir la situación técnica o de negocio que motiva la necesidad de tomar esta decisión. ¿Qué problema u oportunidad se aborda? ¿Cuáles son las fuerzas en tensión, restricciones de tiempo, coste, licenciamiento o complejidad?]`

---

## 3. Requisitos y Criterios de Evaluación
- **Criterio 1:** `[Rendimiento / Latencia requerida]`
- **Criterio 2:** `[Facilidad de mantenimiento y curva de aprendizaje]`
- **Criterio 3:** `[Compatibilidad con arquitectura monolítica limpia MVC]`

---

## 4. Alternativas Consideradas

### Opción 1: `[Nombre de la Alternativa 1]`
- **Descripción:** `[Resumen técnico de la alternativa]`
- **Ventajas:**
  - `[Ventaja 1]`
  - `[Ventaja 2]`
- **Desventajas / Inconvenientes:**
  - `[Inconveniente 1]`
  - `[Inconveniente 2]`

### Opción 2: `[Nombre de la Alternativa 2]`
- **Descripción:** `[Resumen técnico de la alternativa]`
- **Ventajas:**
  - `[Ventaja 1]`
- **Desventajas / Inconvenientes:**
  - `[Inconveniente 1]`

---

## 5. Decisión Adoptada
`[Declarar formalmente cuál es la opción elegida. Si el estado es 'Propuesto', redactar: "Se propone adoptar la Opción X, sujeto a validación del caso de estudio."]`

---

## 6. Justificación
`[Explicar detalladamente por qué la opción elegida supera a las demás alternativas según los criterios de evaluación y restricciones del proyecto.]`

---

## 7. Consecuencias

### 7.1. Consecuencias Positivas (Beneficios)
- `[Impacto positivo 1]`
- `[Impacto positivo 2]`

### 7.2. Consecuencias Negativas y Riesgos (Compromisos / Trade-offs)
- `[Compromiso 1 y cómo se mitigará]`
- `[Compromiso 2]`

---

## 8. Decisiones y Artefactos Relacionados
- **ADRs Vinculados:** `[ADR-0001 | No aplica]`
- **Especificaciones / Contratos Afectados:** `[FEAT-XXX, CONT-YYY | No aplica]`
