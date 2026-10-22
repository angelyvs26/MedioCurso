# Plantilla de Especificación de Funcionalidad (Feature Spec)

> **Instrucciones:** Copia este archivo dentro de `specs/features/` bajo el nombre `[FEAT-XXX-nombre-funcionalidad].md`. Completa cada sección con información contrastada y validada contra los requisitos aprobados. No asumas reglas de negocio sin respaldo.

---

# [FEAT-XXX]: [Nombre de la Funcionalidad]

## 1. Identificación y Metadatos
- **Identificador Único:** `[FEAT-XXX]`
- **Nombre:** `[Nombre de la funcionalidad]`
- **Estado:** `[Borrador | En revisión | Aprobada | En implementación | Verificada | Pendiente de definir]`
- **Requisitos de Origen:** `[RF-XXX, RF-YYY]`
- **Fecha de Creación:** `[AAAA-MM-DD]`
- **Última Modificación:** `[AAAA-MM-DD]`

---

## 2. Objetivo y Alcance
- **Objetivo Principal:** `[Propósito central de la funcionalidad; qué problema del negocio resuelve o qué capacidad habilita]`
- **Alcance Incluido (In-Scope):**
  - `[Capacidad 1 cubierta expresamente]`
  - `[Capacidad 2 cubierta expresamente]`
- **Fuera del Alcance (Out-of-Scope):**
  - `[Elemento expresamente excluido de esta funcionalidad para evitar ampliaciones no acordadas]`

---

## 3. Actores y Participantes
- **Actor Principal:** `[Usuario final / Rol del sistema / Sistema externo / 'No aplica, justificar']`
- **Sistemas o Servicios Involucrados:** `[Base de datos / Servicio de correo / API externa / 'No aplica, justificar']`

---

## 4. Precondiciones
- `[Precondición 1: Estado o requisitos del sistema previos a la ejecución]`
- `[Precondición 2: Permisos, autenticación o datos preexistentes necesarios]`

---

## 5. Flujos de Comportamiento

### 5.1. Flujo Principal (Happy Path)
1. `[Paso 1: El actor o cliente inicia la acción enviando los datos requeridos]`
2. `[Paso 2: El sistema valida los datos de entrada]`
3. `[Paso 3: El sistema ejecuta las reglas de negocio pertinentes]`
4. `[Paso 4: El sistema persiste el nuevo estado o consulta la información requerida]`
5. `[Paso 5: El sistema genera y retorna la respuesta exitosa al actor]`

### 5.2. Flujos Alternativos
- **Flujo Alt-1 `[Título]`:**
  1. `[Condición alternativa bajo la cual se desvía el flujo]`
  2. `[Acción que ejecuta el sistema]`
  3. `[Reincorporación al flujo principal o finalización con éxito alternativo]`

### 5.3. Flujos de Excepción y Manejo de Errores
- **Excepción 1 `[Datos de entrada inválidos o faltantes]`:**
  - **Condición:** `[Falla en validación de formato o presencia de campos requeridos]`
  - **Respuesta:** `[Código o mensaje de error descriptivo retornado; no se altera el estado]`
- **Excepción 2 `[Conflicto de estado o regla de negocio violada]`:**
  - **Condición:** `[Recurso duplicado o condición no permitida]`
  - **Respuesta:** `[Mensaje de rechazo con código semántico adecuado]`

---

## 6. Reglas de Negocio Invariantes
- **[RN-01]:** `[Regla estricta del dominio que debe cumplirse siempre; ej. "El saldo no puede ser negativo"]`
- **[RN-02]:** `[Condición lógica que valida el servicio o la entidad de dominio]`

---

## 7. Validaciones de Datos
| Campo / Parámetro | Tipo de Dato | Obligatorio | Formato / Rango | Mensaje de Error Asociado |
|---|---|---|---|---|
| `[campo_1]` | `[String / Int / etc.]` | `[Sí / No]` | `[Longitud / Patrón / Rango]` | `[Descripción del error al fallar]` |
| `[campo_2]` | `[Tipo]` | `[Sí / No]` | `[Regla]` | `[Mensaje]` |

---

## 8. Entradas y Salidas

### 8.1. Estructura de Entrada
```json
{
  "campo_ejemplo": "[Tipo y valor esperado de entrada]"
}
```

### 8.2. Estructura de Salida Exitosa
```json
{
  "resultado": "[Tipo y valor esperado de respuesta]"
}
```

---

## 9. Criterios de Aceptación
- [ ] **CA-01:** `[Criterio verificable 1 - Caso exitoso]`
- [ ] **CA-02:** `[Criterio verificable 2 - Validación de entradas]`
- [ ] **CA-03:** `[Criterio verificable 3 - Control de excepciones]`

---

## 10. Trazabilidad y Dependencias
- **Requisitos Funcionales Asociados:** `[RF-XXX]`
- **Requisitos No Funcionales Asociados:** `[RNF-XXX | No aplica]`
- **Contratos de Interfaz:** `[CONT-XXX]`
- **Decisiones Arquitectónicas Vinculadas:** `[ADR-XXXX]`
- **Tareas de Implementación:** `[TASK-XXX | Pendiente de planificar]`
- **Plan de Pruebas Asociado:** `[TEST-XXX | Pendiente de planificar]`

---

## 11. Casos Pendientes de Aclaración
- `[Punto 1 que requiere consulta con el docente o caso de estudio pendiente de confirmar]`
