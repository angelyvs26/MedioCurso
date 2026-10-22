# Catálogo Consolidado de Requisitos del Sistema (`specs/requirements/`)

Este directorio contiene los requisitos formales extraídos y fundamentados en el caso de estudio oficial de **ABC Software Corp** y las normativas aplicables de la **SUNAT** (RS N° 097-2012, RS N° 234-2006) y **OASIS UBL 2.1**.

---

## 1. Organización

- **[`functional/`](file:///d:/Projects/MedioCurso/MedioCurso/specs/requirements/functional):** Requisitos funcionales (`RF-001` a `RF-005`): migración de datos legados, validación de RUC/DNI, motor de impuestos (IGV, ISC Cat. 08, ICBPER), emisión de comprobantes UBL 2.1 y comunicación con SUNAT/OSE.
- **[`non-functional/`](file:///d:/Projects/MedioCurso/MedioCurso/specs/requirements/non-functional):** Requisitos no funcionales y restricciones (`RNF-001` a `RNF-003`): arquitectura monolítica limpia MVC desacoplada para evolución a Hexagonal y microservicios, soporte multiplataforma Windows/Android y empaquetado en MSI/APK.

---

## 2. Trazabilidad hacia Casos de Uso y Especificaciones
Todos los requisitos aquí documentados rigen las especificaciones de funcionalidades en [`specs/features/`](file:///d:/Projects/MedioCurso/MedioCurso/specs/features) y los contratos de datos y API en [`specs/contracts/`](file:///d:/Projects/MedioCurso/MedioCurso/specs/contracts).
