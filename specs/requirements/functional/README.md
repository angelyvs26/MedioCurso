# Requisitos Funcionales (`specs/requirements/functional/`)

Este subdirectorio alberga el catálogo formal de requisitos funcionales extraídos directamente del caso de estudio de **ABC Software Corp**.

---

## 1. Índice de Requisitos Funcionales Aprobados

| ID | Nombre del Requisito | Módulo / Área | Fuente en Caso de Estudio |
|---|---|---|---|
| [`RF-001`](file:///d:/Projects/MedioCurso/MedioCurso/specs/requirements/functional/RF-001-migracion-conciliacion-inventario-legado.md) | Migración y Conciliación del Inventario Legado (RS 234-2006) | Almacén / Kardex / Legado | Sección 1 y 2 ("Legado y Migración de Datos") |
| [`RF-002`](file:///d:/Projects/MedioCurso/MedioCurso/specs/requirements/functional/RF-002-validacion-ruc-dni-clientes.md) | Validación Algorítmica y Consulta de Documentos de Identidad (RUC y DNI) | Clientes / Validación Tributaria | Sección 1 ("no valida RUCs") y Sección 3 (Nubefact) |
| [`RF-003`](file:///d:/Projects/MedioCurso/MedioCurso/specs/requirements/functional/RF-003-calculo-impuestos-igv-isc-icbper.md) | Cálculo Integral de Tributos: IGV, ISC (Catálogo 08) e ICBPER | Motor Tributario / Impuestos | Sección 1 ("no calcula impuestos") y Sección 2 (Catálogo 08) |
| [`RF-004`](file:///d:/Projects/MedioCurso/MedioCurso/specs/requirements/functional/RF-004-emision-comprobantes-facturas-boletas.md) | Emisión, Numeración y Control de Facturas y Boletas Electrónicas (UBL 2.1) | Facturación / UBL / Firma | Sección 1 (RS 097-2012, UBL 2.1) |
| [`RF-005`](file:///d:/Projects/MedioCurso/MedioCurso/specs/requirements/functional/RF-005-comunicacion-sunat-ose-gestion-cdr.md) | Comunicación con SUNAT / OSE y Gestión de Constancia de Recepción (CDR) | Integración / OSE / SUNAT | Sección 1 ("no se comunica con OSE o SUNAT") |

---

## 2. Nomenclatura y Plantilla
Cada nuevo requisito debe seguir el estándar `RF-XXX-[nombre-del-requisito].md` y utilizar [`specs/templates/requirement-template.md`](file:///d:/Projects/MedioCurso/MedioCurso/specs/templates/requirement-template.md).
