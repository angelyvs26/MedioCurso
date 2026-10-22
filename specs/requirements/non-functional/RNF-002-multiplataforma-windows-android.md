# Requisito No Funcional: RNF-002 - Soporte Multiplataforma Nativo en Windows y Android

- **Identificador Único:** `RNF-002`
- **Nombre:** Ejecución y Soporte Nativo en Entornos Windows y Dispositivos Android
- **Tipo de Requisito:** No Funcional (Portabilidad y Plataforma)
- **Prioridad:** Alta
- **Estado:** Aprobado (Derivado del Caso de Estudio)
- **Fecha de Registro:** 2026-10-22
- **Autor / Analista:** Equipo de Arquitectura SDD

---

## 1. Descripción General
La aplicación debe ser capaz de ejecutarse de manera nativa y con alto rendimiento tanto en estaciones de trabajo de escritorio con sistema operativo **Microsoft Windows** (Windows 10 / 11) como en terminales móviles o tabletas con sistema operativo **Android** (Android 8.0 o superior). La interfaz debe adaptarse ergonómicamente al factor de forma (pantalla de escritorio con teclado/ratón o pantalla táctil de terminal móvil).

---

## 2. Justificación y Fuente del Caso de Estudio
- **Fuente:** Caso de estudio oficial ABC Software Corp, Sección 2 ("Distribución y Multiplataforma").
- **Cita Textual:**
  > "Por ahora, la aplicación debe ser capaz de correr de manera nativa tanto en entornos Windows como en dispositivos Android, y debe empaquetarse obligatoriamente en un formato de instalación amigable (instalador MSI para entornos de escritorio o un APK para distribuciones Android)."

---

## 3. Criterios de Aceptación Verificables
- **Criterio 1:** En Windows, la aplicación arranca sin necesidad de instalar entornos de ejecución manuales complejos por parte del usuario final.
- **Criterio 2:** En Android, la aplicación se instala limpiamente, respeta el ciclo de vida de la actividad móvil (pausa, reanudación) y soporta la emisión en orientación vertical/horizontal.
- **Criterio 3:** La lógica de cálculo tributario y validación es exactamente idéntica e invariante en ambas plataformas.

---

## 4. Trazabilidad
- **Decisión Registrada:** [`ADR-0002`](file:///d:/Projects/MedioCurso/MedioCurso/specs/architecture/decisions/0002-plataformas-distribucion-cliente.md).
- **Especificación de Arquitectura:** [`ARC-001`](file:///d:/Projects/MedioCurso/MedioCurso/specs/architecture/mvc-and-hexagonal-architecture.md).
