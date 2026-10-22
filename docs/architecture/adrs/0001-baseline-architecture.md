# ADR 0001: Adopción de Arquitectura Monolítica Limpia MVC y Metodología Spec-First SDD

- **Estado:** Aceptado
- **Fecha:** 2026-10-22
- **Autor:** Equipo de Arquitectura
- **Contexto:** Fase 1 - Diseño y Creación de Estructura Inicial

---

## 1. Contexto

Se requiere establecer la base arquitectónica y el flujo metodológico para un nuevo proyecto de software cuyo caso de estudio completo aún no ha sido provisto. 

Se busca evitar dos problemas comunes en proyectos tempranos:
1. **Desarrollo sin especificación (Code-First):** Implementar código antes de validar requisitos y contratos de interfaz.
2. **Monolito acoplado o microservicios prematuros:** Caer en controladores sobrecargados con consultas directas a base de datos (Fat Controllers) o, en el extremo opuesto, fragmentar prematuramente en microservicios introduciendo sobrecarga operacional injustificada.

---

## 2. Decisión

1. **Metodología Spec-Driven Development (SDD):** El proyecto adoptará estrictamente el enfoque Spec-First. No se escribirá código funcional hasta que los requisitos (`docs/requirements/`), decisiones arquitectónicas (`docs/architecture/`), contratos de interfaz (`docs/contracts/`) y la planificación (`docs/planning/`) estén definidos y consensuados.
2. **Patrón Monolítico Limpio MVC:** Se adopta una arquitectura monolítica organizada bajo el patrón MVC con capas complementarias de Dominio, Servicios de Aplicación e Infraestructura:
   - `src/controllers/` y `src/views/`: Capa de presentación (interacción con usuarios o clientes HTTP/CLI).
   - `src/domain/`: Reglas de negocio puras y modelos de dominio desacoplados de infraestructura.
   - `src/services/`: Orquestación de casos de uso del negocio.
   - `src/infrastructure/`: Implementaciones de persistencia (BD) y servicios externos.
3. **Facilitación de Transición a Arquitectura Hexagonal:** Mantener aisladas las reglas de negocio del framework y base de datos garantiza que, si el dominio lo exige, se pueda evolucionar formalmente a Puertos y Adaptadores (Arquitectura Hexagonal) sin rehacer la lógica central.

---

## 3. Consecuencias

### Positivas
- Claridad absoluta en el orden de trabajo: Especificación -> Contrato -> Planificación -> Código -> Pruebas.
- Alta testabilidad: Las reglas de negocio pueden probarse unitariamente de forma aislada sin levantar base de datos ni servidores web.
- Reducción drástica de deuda técnica y código huérfano (no especificado).
- Simplicidad operativa: Despliegue y mantenimiento monolítico sin complejidad de red distribuida.

### Compromisos y Mitigaciones
- Requiere disciplina estricta para no colocar lógica de negocio en controladores ni en vistas.
- Se debe mantener actualizada la documentación y la matriz de trazabilidad con cada cambio.

---

## 4. Decisiones Pendientes (Bloqueadas por el Caso de Estudio)

Las siguientes decisiones quedan explícitamente registradas como **pendientes** hasta contar con el enunciado o caso de estudio definitivo:

| Decisión Pendiente | Factor de Dependencia | Alternativas Consideradas |
|---|---|---|
| **Lenguaje de Programación** | Naturaleza del problema, restricciones de rendimiento y entorno de ejecución del cliente | TypeScript/Node.js, Python, Java, C#, Go |
| **Framework Web / MVC** | Lenguaje seleccionado y tipo de entrega (renderizado del servidor vs API REST) | Dependiente del lenguaje |
| **Motor de Persistencia** | Modelo relacional vs documental, volumen transaccional y consistencia requerida | PostgreSQL, SQLite, MySQL, MongoDB |
| **Mecanismo de Vistas** | Requisito de interfaz (HTML server-side, plantillas, CLI o SPA desacoplada) | SSR Templates, JSON ViewModels |
| **Esquema de Autenticación / Seguridad** | Requisitos de acceso y roles de usuario del caso de estudio | Sesiones con cookies, JWT, OAuth2 |
