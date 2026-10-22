# Caso de Estudio: ABC Software Corp - Transformación a Facturación Electrónica SUNAT / UBL 2.1

- **Fecha de Recepción:** 2026-10-22
- **Estado:** Texto Oficial Íntegro Recibido
- **Organización:** ABC Software Corp
- **Sector de Negocio:** Comercialización de abarrotes y mercadería al por mayor y menor

---

## 1. Descripción del Caso (Texto Original Íntegro)

> "Ha sido contratado por ABC Software Corp, una empresa mediana que comercializa abarrotes y mercadería al por mayor y menor. Tienen un software operativo básico que cumple con lo justo y necesario para registrar el día a día del almacén (controlan fichas sencillas con campos como fecha, ingreso, salida, producto y un número de documento interno bastante informal).
> 
> El problema grave es que la SUNAT, mediante la RS N° 097-2012, RS N° 234-2006 y las normativas modernas de masificación de comprobantes electrónicos, les exige llevar registros formales y emitir comprobantes legales estructurados bajo el estándar internacional UBL (Universal Business Language), puede estudiar el estándar en la web de OASIS. El sistema actual tiene vacío por todos lados: no valida RUCs, no calcula correctamente los impuestos (IGV, ISC, ICBPER, etc), no genera XMLs firmados digitalmente y menos se comunica con un Operador de Servicios Electrónicos (OSE) o directamente con la SUNAT.
> 
> Tu misión como líder de equipo de desarrollo no es ponerte a codificar a lo que salga, sino mediante la metodología Spec-First: primero definir las reglas, contratos de datos, validaciones y contratos de interfaz antes de escribir una sola línea de lógica de negocio, estudiando el archivo UBL-Invoice-2.1.xsd, pero deberás estudiar todas las reglas antes, apoyándote de AI."

---

## 2. Alcance Técnico y Restricciones Arquitectónicas (Texto Original Íntegro)

> "La gerencia ha puesto sobre la mesa las siguientes condiciones técnicas inevitables:
> 
> - **Legado y Migración de Datos:** Toda la información histórica del software básico de mercadería (fecha, ingreso, salida, producto, nro_documento) debe acoplarse y mapearse limpiamente hacia las nuevas entidades del sistema sin perder trazabilidad, por lo que el sistema debe manejar migración de datos.
> 
> - **Normativa Tributaria Peruana:** Se deberá de dar estricto cumplimiento de los requerimientos de la SUNAT (catálogos de tipos de documentos, afectación al IGV, códigos de moneda, series, correlativos y validaciones de emisión), un punto clave aquí es el catálogo 8 de SUNAT.
> 
> - **Arquitectura Base y Futura:** El software debe construirse inicialmente bajo una arquitectura monolítica limpia utilizando el patrón MVC (Modelo-Vista-Controlador). Sin embargo, el diseño debe desacoplarse de tal forma que permita una fácil migración a futuro hacia una Arquitectura Hexagonal o Clean Architecture, preparándose para escalar a microservicios cuando la empresa crezca, este detalle debe verse claramente en los specs.
> 
> - **Distribución y Multiplataforma:** Por ahora, la aplicación debe ser capaz de correr de manera nativa tanto en entornos Windows como en dispositivos Android, y debe empaquetarse obligatoriamente en un formato de instalación amigable (instalador MSI para entornos de escritorio o un APK para distribuciones Android)."

---

## 3. Descubrimiento de los Casos de Uso (Texto Original Íntegro)

> "La gerencia desea que detalle los casos posibles, revisando opiniones de usuarios de facturación electrónica, por lo que puede revisar otros softwares en línea como es el caso de Nubefact, donde podría crearse un usuario DEMO."

---

## 4. Síntesis Estructurada de Requerimientos y Fuerzas del Negocio

| Dimensión | Situación Actual (Legada) | Exigencia / Estado Objetivo |
|---|---|---|
| **Giro del Negocio** | Comercializadora de abarrotes y mercadería al por mayor y menor. | Requiere venta ágil en mostrador (boletas/tickets) y distribución al por mayor (facturas con RUC). |
| **Control de Almacén** | Fichas básicas con: `fecha`, `ingreso`, `salida`, `producto`, `nro_documento` informal. | Registro de Inventario Permanente Valorizado / Kardex formal según **RS N° 234-2006/SUNAT**, migrando el historial sin pérdida. |
| **Validación de Identidad** | Sin validaciones (vacío de datos). | Validación estricta de **RUC** (11 dígitos, algoritmo módulo 11) y **DNI** (8 dígitos), con consulta de validez. |
| **Régimen Tributario** | Sin cálculo de impuestos o cálculos erróneos. | Cálculo exacto de: **IGV (18%)**, **ISC según Catálogo 08** (bebidas azucaradas/alcohólicas de abarrotes), e **ICBPER** (impuesto a las bolsas plásticas). |
| **Comprobantes Emitidos** | Documentos internos informales. | Facturas (`01`), Boletas (`03`), Notas de Crédito (`07`), Notas de Débito (`08`), generadas en **XML UBL 2.1**. |
| **Firma Digital y Envío** | Inexistente. | Firma digital **XAdES-BES**, empaquetado ZIP y transmisión directa a **SUNAT o vía OSE** con obtención y validación del **CDR**. |
| **Arquitectura** | Software básico no escalable. | **Monolito MVC Limpio**, preparado para evolucionar a **Hexagonal / Clean Architecture** y futuros **microservicios**. |
| **Despliegue y Plataforma** | Indefinido. | Aplicación nativa en **Windows** (instalador `.msi`) y **Android** (paquete `.apk`). |
| **Experiencia de Usuario** | Sin automatización comercial. | Flujo de facturación rápido inspirado en líderes del mercado peruano como **Nubefact**. |
