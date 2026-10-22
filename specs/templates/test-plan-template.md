# Plantilla de Plan y Especificación de Pruebas (Test Plan)

> **Instrucciones:** Copia este archivo dentro de `specs/planning/` o vincula las especificaciones de prueba en este formato. Sirve para definir la estrategia de validación formal antes y durante la fase de codificación.

---

# [TEST-XXX]: [Nombre del Plan o Caso de Prueba]

## 1. Identificación y Metadatos
- **Identificador Único:** `[TEST-XXX]`
- **Nombre:** `[Nombre representativo de la prueba]`
- **Tipo de Prueba:** `[Unitaria (Dominio/Servicios) | Integración (Controlador/Persistencia) | Extremo a Extremo (E2E) | Contrato]`
- **Nivel de Automatización:** `[Automatizada | Manual]`
- **Estado:** `[Planificada | En ejecución | Superada | Fallida | Pendiente de definir]`

---

## 2. Objetivo del Plan de Pruebas
`[Describir qué comportamiento, regla de negocio, invariante o contrato se validará con esta suite de pruebas.]`

---

## 3. Elementos Verificados (Trazabilidad)
- **Requisitos Funcionales Asociados:** `[RF-XXX]`
- **Requisitos No Funcionales Asociados:** `[RNF-XXX | No aplica]`
- **Funcionalidad (Feature) Asignada:** `[FEAT-XXX]`
- **Contrato de Interfaz Verificado:** `[CONT-XXX | No aplica]`

---

## 4. Precondiciones y Datos de Prueba Requeridos
- **Estado del Entorno:** `[Base de datos de prueba en memoria / Mocks preparados / Variables de entorno]`
- **Datos de Entrada (Fixtures / Datasets):**
```json
{
  "dato_prueba": "[Valor controlado para reproducir el escenario]"
}
```

---

## 5. Escenarios de Prueba Detallados

### Escenario 1: [Prueba de Flujo Exitoso / Caso Nominal]
- **Identificador de Escenario:** `[ESC-01]`
- **Precondición:** `[Dado que el sistema tiene el estado inicial X]`
- **Acción / Estímulo:** `[Cuando se invoca el método o endpoint con parámetros válidos]`
- **Resultado Esperado:** `[Entonces el retorno es exitoso con código/estructura Y y el estado final es Z]`

### Escenario 2: [Prueba de Condición de Borde o Excepción de Regla de Negocio]
- **Identificador de Escenario:** `[ESC-02]`
- **Precondición:** `[Dado que el elemento ya existe o no cumple la regla de validación]`
- **Acción / Estímulo:** `[Cuando se intenta registrar el elemento inválido]`
- **Resultado Esperado:** `[Entonces se captura la excepción correspondiente y no se persiste ningún cambio]`

---

## 6. Criterios de Aprobación
- [ ] La totalidad de los escenarios automatizados se ejecutan en verde (sin errores ni fallos).
- [ ] Las aserciones cubren tanto los códigos de respuesta como los efectos colaterales de persistencia.
- [ ] No se producen excepciones no controladas.

---

## 7. Registro de Evidencias y Resultados Obtenidos
- **Fecha de Ejecución:** `[AAAA-MM-DD | Pendiente de ejecución]`
- **Resultado Global:** `[Éxito (Passed) | Falla (Failed) | No ejecutada]`
- **Salida / Logs de la Ejecución:**
```text
[Pegar salida del runner de pruebas o adjuntar reporte de cobertura cuando esté disponible]
```

---

## 8. Incidencias Detectadas y Trazabilidad de Defectos
- **Incidencia 1:** `[Descripción del defecto detectado durante la prueba | Ninguna]`
- **Acción Correctiva:** `[Tarea asociada para resolver el defecto | No aplica]`
