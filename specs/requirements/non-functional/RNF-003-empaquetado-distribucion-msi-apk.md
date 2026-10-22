# Requisito No Funcional: RNF-003 - Empaquetado y Distribución en Formatos MSI y APK

- **Identificador Único:** `RNF-003`
- **Nombre:** Empaquetado de Instalación Amigable en Formato MSI (Windows) y APK (Android)
- **Tipo de Requisito:** No Funcional (Distribución y Despliegue)
- **Prioridad:** Alta
- **Estado:** Aprobado (Derivado del Caso de Estudio)
- **Fecha de Registro:** 2026-10-22
- **Autor / Analista:** Equipo de Arquitectura SDD

---

## 1. Descripción General
El sistema debe proveer mecanismos de empaquetado y distribución amigables para el usuario no técnico de **ABC Software Corp**:
1. Para entornos de escritorio Windows: Generación obligatoria de un instalador estándar en formato **Microsoft Installer (`.msi`)**, con asistente paso a paso, creación de accesos directos y desinstalación limpia.
2. Para entornos móviles Android: Generación obligatoria de un paquete **Android Application Package (`.apk`)** firmado, instalable en dispositivos de la empresa mediante carga directa (sideloading) o gestión de dispositivos.

---

## 2. Justificación y Fuente del Caso de Estudio
- **Fuente:** Caso de estudio oficial ABC Software Corp, Sección 2 ("Distribución y Multiplataforma").
- **Cita Textual:**
  > "...debe empaquetarse obligatoriamente en un formato de instalación amigable (instalador MSI para entornos de escritorio o un APK para distribuciones Android)."

---

## 3. Criterios de Aceptación Verificables
- **Criterio 1:** El instalador `.msi` instala todos los binarios necesarios, configura la base de datos local embebida y permite actualizar versiones sin perder datos de facturación previa.
- **Criterio 2:** El paquete `.apk` cuenta con manifiesto válido, solicita los permisos mínimos necesarios (almacenamiento para guardar PDFs/XMLs y red para comunicación con SUNAT) y es compatible con arquitecturas ARM/x86 modernas.

---

## 4. Trazabilidad
- **Decisión Registrada:** [`ADR-0002`](file:///d:/Projects/MedioCurso/MedioCurso/specs/architecture/decisions/0002-plataformas-distribucion-cliente.md).
