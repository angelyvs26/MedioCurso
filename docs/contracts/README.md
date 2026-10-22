# Contratos de API y Esquemas de Datos (`docs/contracts/`)

Este directorio contiene los contratos formales de interfaz y esquemas de validación de datos del proyecto, asegurando que la comunicación entre clientes y el backend esté estrictamente definida antes de implementar rutas y controladores.

---

## 1. Propósito

En la metodología **Spec-Driven Development (SDD)**, los contratos de interfaz actúan como un pacto vinculante entre los consumidores y los proveedores del servicio:
- Permiten generar pruebas de contrato automatizadas.
- Sirven de base para la generación de mocks y documentación interactiva.
- Evitan discrepancias de tipos, estructuras o códigos de respuesta HTTP durante la implementación.

---

## 2. Formato de los Contratos

- Los contratos se documentarán en formato **YAML** o **JSON Schema** según corresponda.
- No se ata el proyecto a una versión rígida de OpenAPI ni a un framework específico hasta evaluar los requisitos de comunicación del caso de estudio (ej. REST OpenAPI 3.x, GraphQL Schema, o especificaciones RPC).

---

## 3. Estado Actual

> **Pendiente:** Los contratos definitivos se redactarán una vez especificados los requisitos funcionales y casos de uso del caso de estudio.
