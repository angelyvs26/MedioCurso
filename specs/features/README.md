# Especificaciones de Funcionalidades (`specs/features/`)

Este directorio aloja las especificaciones funcionales detalladas (**Feature Specs**) extraídas del caso de estudio de **ABC Software Corp**, la normativa de la **SUNAT** y el análisis de casos de uso de plataformas líderes del mercado peruano (como **Nubefact**).

---

## 1. Índice de Funcionalidades Especificadas

| ID | Nombre de la Funcionalidad | Área / Módulo | Requisitos de Origen | Estado |
|---|---|---|---|---|
| [`FEAT-001`](file:///d:/Projects/MedioCurso/MedioCurso/specs/features/FEAT-001-migracion-mercaderia-kardex.md) | Migración de Movimientos Legados y Apertura del Kardex Formal (RS 234-2006) | Almacén / Kardex / Inventario | `RF-001`, `RNF-005` | Aprobada |
| [`FEAT-002`](file:///d:/Projects/MedioCurso/MedioCurso/specs/features/FEAT-002-emision-rapida-mostrador-boleta-factura.md) | Emisión Rápida en Mostrador de Facturas y Boletas (IGV, ISC Catálogo 08, ICBPER) | Ventas / Mostrador / Motor Tributario | `RF-002`, `RF-003`, `RF-004` | Aprobada |
| [`FEAT-003`](file:///d:/Projects/MedioCurso/MedioCurso/specs/features/FEAT-003-transmision-sunat-ose-cdr.md) | Transmisión Automatizada a SUNAT / OSE con Cola de Reintentos y Gestión de CDR | Integración / OSE / SUNAT | `RF-004`, `RF-005` | Aprobada |

---

## 2. Nomenclatura y Criterio de Implementación
Cada funcionalidad debe redactarse según [`specs/templates/feature-spec-template.md`](file:///d:/Projects/MedioCurso/MedioCurso/specs/templates/feature-spec-template.md). Una funcionalidad puede iniciar su implementación técnica en `src/` una vez que su especificación y contratos estén cerrados sin ambigüedades.
