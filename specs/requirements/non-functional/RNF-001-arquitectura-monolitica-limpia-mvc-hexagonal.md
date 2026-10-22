# Requisito No Funcional: RNF-001 - Arquitectura Monolítica Limpia MVC Desacoplada

- **Identificador Único:** `RNF-001`
- **Nombre:** Arquitectura Monolítica Limpia MVC con Capacidad de Evolución a Hexagonal y Microservicios
- **Tipo de Requisito:** No Funcional (Arquitectura y Mantenibilidad)
- **Prioridad:** Crítica
- **Estado:** Aprobado (Derivado del Caso de Estudio)
- **Fecha de Registro:** 2026-10-22
- **Autor / Analista:** Equipo de Arquitectura SDD

---

## 1. Descripción General
El software debe construirse inicialmente como una **Arquitectura Monolítica Limpia basada en el patrón Modelo-Vista-Controlador (MVC)**, organizando todo el código en una sola unidad de desarrollo y despliegue dentro de `src/`. Sin embargo, el diseño debe estar rigurosamente desacoplado mediante la inversión de dependencias y la separación de responsabilidades, de tal forma que permita una transición fluida hacia **Arquitectura Hexagonal (Puertos y Adaptadores)** o **Clean Architecture**, y siente las bases técnicas para escalar hacia **microservicios** cuando la empresa crezca.

---

## 2. Justificación y Fuente del Caso de Estudio
- **Fuente:** Caso de estudio oficial ABC Software Corp, Sección 2 ("Arquitectura Base y Futura").
- **Cita Textual:**
  > "El software debe construirse inicialmente bajo una arquitectura monolítica limpia utilizando el patrón MVC (Modelo-Vista-Controlador). Sin embargo, el diseño debe desacoplarse de tal forma que permita una fácil migración a futuro hacia una Arquitectura Hexagonal o Clean Architecture, preparándose para escalar a microservicios cuando la empresa crezca, este detalle debe verse claramente en los specs."

---

## 3. Criterios de Aceptación Verificables

#### Criterio 1: Separación estricta de capas
- **Dado:** El código fuente desarrollado en `src/`.
- **Cuando:** Se realiza una auditoría de dependencias estáticas.
- **Entonces:**
  - Los controladores en `src/controllers/` no contienen sentencias SQL, llamadas a ORMs ni lógica de cálculo tributario; únicamente deserializan peticiones y delegan a `src/services/`.
  - El dominio en `src/domain/` no tiene ninguna dependencia hacia librerías web (HTTP), motores de base de datos o herramientas de serialización XML.
  - La infraestructura en `src/infrastructure/` implementa las interfaces de persistencia y comunicación sin filtrar detalles de bajo nivel hacia el dominio.

#### Criterio 2: Viabilidad de migración a Arquitectura Hexagonal sin reescritura
- **Dado:** La definición de casos de uso y servicios de aplicación.
- **Cuando:** Se requiere cambiar un adaptador secundario (ej. sustituir persistencia SQLite por PostgreSQL o cambiar el emisor de firmas digitales).
- **Entonces:** La modificación se limita a crear un nuevo adaptador en `infrastructure/` sin modificar las entidades de dominio ni las reglas de negocio de facturación.

#### Criterio 3: Preparación para Microservicios
- **Dado:** El crecimiento futuro del volumen de ventas de ABC Software Corp.
- **Cuando:** La empresa decida segregar el módulo de Facturación UBL del módulo de Gestión de Almacén/Mercadería.
- **Entonces:** Los límites de contexto (Bounded Contexts) ya definidos en los servicios de aplicación permiten extraer cada módulo como un microservicio independiente sin rehacer el modelo lógico.

---

## 4. Trazabilidad
- **Documento Arquitectónico:** `specs/architecture/mvc-and-hexagonal-architecture.md` (`ARC-001`).
- **Decisión Registrada:** `docs/architecture/adrs/0001-baseline-architecture.md`.
