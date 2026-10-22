# Plantilla de Especificación de Contrato de Interfaz

> **Instrucciones:** Copia este archivo dentro de `specs/contracts/` bajo el nombre `[CONT-XXX-nombre-contrato].md` (o archivo `.yaml` correspondiente complementado con este documento). Sirve para contratos de interfaces internas (entre capas MVC) o externas (API HTTP, CLI o mensajería) sin presuponer exclusivamente REST.

---

# [CONT-XXX]: [Nombre Descriptivo del Contrato]

## 1. Identificación y Metadatos
- **Identificador Único:** `[CONT-XXX]`
- **Nombre:** `[Nombre del contrato / interfaz]`
- **Ámbito:** `[Interno (entre capas del monolito) | Externo (API hacia clientes o terceros) | Pendiente de definir]`
- **Tipo de Interfaz:** `[HTTP REST | Interfaz de Servicio / Inversión de Control | CLI / Consola | RPC | Pendiente de definir]`
- **Versión del Contrato:** `[v1.0.0 | Pendiente de definir]`
- **Estado:** `[Borrador | En revisión | Aprobado | Deprecado | Pendiente de definir]`

---

## 2. Objetivo y Contexto
`[Explicar el propósito del contrato y el acuerdo de comunicación que formaliza entre los componentes consumidores y proveedores.]`

---

## 3. Componentes y Sistemas Participantes
- **Consumidor (Cliente / Emisor):** `[Ej. Capa de Controladores / Cliente Web / Sistema Externo]`
- **Proveedor (Servicio / Receptor):** `[Ej. Capa de Servicios de Aplicación / Endpoint del Monolito]`

---

## 4. Operaciones o Métodos de la Interfaz

### Operación: `[nombreOperacion | Ej. registrarElemento / POST /recursos]`
- **Propósito:** `[Qué acción desencadena esta operación]`
- **Mecanismo de Invocación:** `[Método HTTP + Ruta / Llamada a función asíncrona / Comando CLI]`

#### 4.1. Parámetros y Datos de Entrada
| Parámetro / Propiedad | Ubicación / Tipo | Obligatorio | Validación / Restricción | Descripción |
|---|---|---|---|---|
| `[id]` | `[Path / Parámetro / Body]` | `[Sí / No]` | `[UUID / Int > 0]` | `[Identificador]` |
| `[campo_1]` | `[Payload / DTO]` | `[Sí / No]` | `[Regla de formato]` | `[Descripción del dato]` |

**Esquema de Carga Útil (Payload / DTO de Entrada):**
```json
{
  "$schema": "http://json-schema.org/draft-07/schema#",
  "title": "[NombreDTOEntrada]",
  "type": "object",
  "properties": {
    "campo_1": {
      "type": "string",
      "description": "[Descripción del campo]"
    }
  },
  "required": ["campo_1"]
}
```

#### 4.2. Respuestas y Salidas Exitosas
- **Código / Estado:** `[200 OK / 201 Created / Retorno DTO / void]`
- **Esquema de Datos de Salida:**
```json
{
  "type": "object",
  "properties": {
    "resultado": { "type": "string" }
  }
}
```

#### 4.3. Respuestas Excepcionales y Códigos de Error
| Código / Excepción | Condición Disparadora | Estructura del Error |
|---|---|---|
| `[400 Bad Request / ValidationError]` | Formato de entrada no conforme con el esquema | `{"error": "VALIDATION_FAILED", "details": [...]}` |
| `[404 Not Found / EntityNotFoundException]` | Recurso solicitado no existe | `{"error": "RESOURCE_NOT_FOUND", "message": "..."}` |
| `[409 Conflict / BusinessRuleViolationException]` | Invariante de negocio violada | `{"error": "BUSINESS_RULE_VIOLATION", "message": "..."}` |

---

## 5. Requisitos de Seguridad y Autenticación
- **Nivel de Acceso Requerido:** `[Público | Autenticado | Rol específico | No aplica, justificar]`
- **Mecanismo de Transporte Seguro:** `[Cabecera de autorización / Sesión / Parámetros internos]`

---

## 6. Trazabilidad
- **Requisitos Vinculados:** `[RF-XXX]`
- **Funcionalidades Vinculadas:** `[FEAT-XXX]`
- **Esquemas Detallados:** `[specs/contracts/schemas/[schema-name].json | Pendiente]`
- **Pruebas de Contrato:** `[tests/integration/test-contract-XXX | Pendiente]`

---

## 7. Aspectos Pendientes
- `[Formato tecnológico definitivo a confirmar con el caso de estudio]`
