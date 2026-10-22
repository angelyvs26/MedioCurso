# ADR 0002: Estrategia Arquitectónica para Distribución Multiplataforma (Windows MSI y Android APK)

- **Estado:** Propuesto
- **Fecha:** 2026-10-22
- **Autor:** Equipo de Arquitectura
- **Contexto:** Fase 2 - Diseño Lógico y Esqueleto Integrado
- **Requisitos Relacionados:** RNF-001 (Multiplataforma Windows/Android), RNF-002 (Distribución empaquetada MSI/APK), RF-001 (Emisión de Comprobantes)

---

## 1. Contexto y Planteamiento del Problema

El sistema de gestión de comprobantes electrónicos para **ABC Software Corp** debe dar soporte operativo tanto en estaciones fijas de escritorio (equipos de caja o administración con sistema operativo **Windows**) como en terminales móviles o tabletas en piso de venta (con sistema operativo **Android**), previendo mecanismos de empaquetado y distribución mediante instaladores estándar (`.msi` para Windows y `.apk` para Android).

Al mismo tiempo, el sistema opera bajo una **arquitectura monolítica limpia basada en MVC**, con estricto apego a las normas tributarias de la SUNAT (control secuencial de numeración correlativa por serie, firma digital y comunicación con el web service de facturación).

El problema radica en conciliar la arquitectura monolítica con la dualidad de plataformas de cliente, resolviendo:
1. ¿Dónde reside el control de la correlatividad para evitar que dos dispositivos emitan la misma serie y correlativo (`F001-00000001`)?
2. ¿Dónde se almacena y ejecuta el certificado digital de firma XAdES-BES para garantizar su seguridad y no exponer la clave privada en terminales móviles?
3. ¿Cómo estructurar el código en `src/` para que la lógica de negocio del dominio no dependa del sistema operativo?

---

## 2. Alternativas Consideradas

### Opción 1: Monolito Autónomo Embarcado en Cada Dispositivo (Full Standalone)
- **Descripción:** Cada paquete instalable (MSI en Windows y APK en Android) compila la aplicación completa, incluyendo motor de base de datos local embebida (ej. SQLite) y certificado digital local.
- **Ventajas:** Funcionamiento 100% desconectado (offline-first).
- **Inconvenientes:** 
  - Alto riesgo de colisión de numeración correlativa a menos que cada dispositivo tenga series físicas asignadas independientes (ej. Caja 1: `F001`, Caja 2: `F002`, Móvil 1: `F003`).
  - Distribución insegura del archivo de certificado digital `.p12` a dispositivos móviles de campo.
  - Complejidad en la conciliación y consolidación de mercadería.

### Opción 2: Monolito Backend Servidor + Clientes Delgados por Plataforma (Client-Server Monolith)
- **Descripción:** La aplicación monolítica se ejecuta como un servicio centralizado (en red local o nube privada de ABC Software Corp) que contiene la base de datos central, la custodia del certificado digital y la API MVC. Los entregables MSI y APK son clientes de interfaz gráfica livianos que consumen el backend.
- **Ventajas:**
  - Correlatividad estrictamente secuencial y atómica garantizada por la base de datos central.
  - Máxima seguridad de la clave privada del certificado digital (nunca sale del servidor).
  - Trazabilidad y conciliación inmediata de stock de mercadería.
- **Inconvenientes:**
  - Requiere conectividad de red local (Wi-Fi/LAN) entre los terminales y el servidor central.

### Opción 3: Monolito Híbrido Desacoplado con Series por Punto de Emisión
- **Descripción:** El monolito implementa el patrón MVC donde los Controladores pueden invocarse localmente mediante llamada directa en memoria (en modo monousuario de escritorio) o mediante transporte HTTP (en modo cliente-servidor). Cada dispositivo o caja opera con una Serie independiente aprobada por SUNAT (`F001`, `F002`, etc.).
- **Ventajas:** Máxima flexibilidad para ABC Software Corp; soporta tanto despliegue centralizado en LAN como emisión en terminales autónomos.
- **Inconvenientes:** Requiere mayor rigor en la gestión de catálogos y configuración de series.

---

## 3. Decisión Propuesta

Se propone adoptar la **Opción 3 (Monolito Híbrido Desacoplado con Controladores Agnósticos y Series por Punto de Emisión)**:

1. **Aislamiento en `src/`:** La lógica de Dominio (`src/domain/`) y los Servicios de Aplicación (`src/services/`) se mantienen 100% neutrales y agnósticos de Windows y Android.
2. **Control de Series por Terminal:** Para habilitar la coexistencia de Windows y Android sin colisiones de correlatividad, cada punto de venta configurará una serie asignada independiente según la normativa SUNAT (ej. `F001` para Terminal Windows Principal, `F002` para Terminal Android Móvil).
3. **Adaptadores de Presentación Separados:** Los controladores (`src/controllers/`) estructuran la mediación de datos utilizando los DTOs definidos en `specs/contracts/schemas/`, permitiendo que la interfaz gráfica (UI) se enlace directamente sin contaminar el núcleo.

---

## 4. Aspectos Pendientes que Requieren Aprobación Definitiva

Antes de cerrar este ADR en estado `Aceptado`, se debe confirmar con el caso de estudio completo:
1. **Entorno de Red de ABC Software Corp:** ¿Cuenta con una red local cableada/Wi-Fi en sus locales para un esquema cliente-servidor, o requiere emisión itinerante en campo sin internet?
2. **Políticas de Certificado Digital:** ¿SUNAT proveerá certificado tributario del contribuyente o se empleará un certificado de pruebas en software?
3. **Selección del Stack Tecnológico:** La elección final del lenguaje (ej. C#/.NET con MAUI/WPF, Java/Kotlin, Node.js/TypeScript o Python) dependerá de las restricciones de empaquetado para generar de forma nativa los archivos `.msi` y `.apk`.
