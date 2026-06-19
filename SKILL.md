---
name: auditoria-ley-21719
version: 1.1
description: >-
  Audita un proyecto/codebase contra la Ley N° 21.719 de Protección de Datos
  Personales de Chile y produce un reporte de cumplimiento priorizado. Úsalo
  siempre que el usuario pida revisar, auditar o diagnosticar un sistema frente a
  la ley de protección de datos chilena, mencione "Ley 21719", "21.719", "ley de
  datos personales", "protección de datos", "Agencia de Protección de Datos",
  "qué necesito implementar para cumplir", "cumplimiento de datos personales",
  "RGPD/GDPR chileno", "datos sensibles", "derechos del titular", o quiera saber
  qué controles técnicos y legales le faltan a un proyecto (CRM, ERP, SaaS,
  RR.HH., soporte, pagos, etc.) antes del 1 de diciembre de 2026. Aplica aunque
  el usuario no nombre el número de la ley pero describa la necesidad de cumplir
  con privacidad/datos personales en Chile.
---

# Auditoría de cumplimiento — Ley N° 21.719 (Protección de Datos, Chile)

## Qué hace este skill

Analiza un proyecto (código, configuración, base de datos, documentación) y
determina, ítem por ítem, qué controles de **seguridad y de cumplimiento** exige
la Ley N° 21.719 y cuáles ya están implementados, parciales o faltantes. El
resultado es un **reporte de cumplimiento priorizado** que el usuario puede usar
como backlog de trabajo antes de la entrada en vigencia general (1 de diciembre
de 2026).

El alcance es **técnico + legal/administrativo**: no solo revisa el código, sino
también la existencia de política de privacidad, gestión de proveedores,
transferencias internacionales y procedimientos. Lo que no se puede verificar
mirando el repositorio se marca explícitamente como "requiere confirmación" en
vez de inventarse un estado.

> Este skill es una pauta técnica de cumplimiento, **no asesoría legal**. El
> reporte debe cerrar siempre recordando que una revisión legal formal es
> recomendable para los puntos administrativos y de alto riesgo.

## Cuándo usarlo

Cuando el usuario quiere saber, para un proyecto concreto, **qué le falta
implementar** para cumplir con la ley chilena de datos personales. Típicamente
trabajará proyecto por proyecto (cada sistema tiene su propio tratamiento de
datos, así que cada uno necesita su propio reporte).

## Cómo trabajar (workflow)

### Paso 0 — Cargar el checklist y entender el proyecto

1. Lee `references/checklist-auditoria.md`. Es la lista maestra de ítems
   auditables, agrupados por categoría, cada uno con **cómo verificarlo en el
   código** y su **prioridad** (Crítica / Alta / Media). Es el corazón del skill:
   no inventes ítems fuera de esta lista, pero sí puedes profundizar en los que
   apliquen al proyecto.
2. Si necesitas precisar un concepto legal (qué es dato sensible, plazos de
   derechos, cuándo notificar un incidente), consulta
   `references/contexto-legal.md`.
3. Identifica el **stack y la naturaleza del proyecto** antes de auditar, porque
   cambia qué ítems aplican y cómo se verifican:
   - Lee el `CLAUDE.md` del proyecto si existe (ahí suele estar stack, módulos,
     despliegue).
   - Detecta framework/lenguaje (`package.json`, `composer.json`,
     `requirements.txt`, `*.csproj`, etc.).
   - Detecta el motor de base de datos y revisa el esquema/migraciones.
   - Clasifica el tipo de sistema (CRM, RR.HH., pagos, soporte, CCTV, con IA…),
     porque eso eleva el riesgo de ciertos módulos (ver checklist, categoría O).

### Paso 1 — Descubrir los datos personales (lo primero y más importante)

Antes de auditar controles, hay que saber **qué datos personales trata el
sistema**, porque toda la ley se construye sobre eso. Sin este paso el reporte
es genérico e inútil.

- Revisa esquemas de BD, modelos/entidades, migraciones y formularios buscando
  campos que identifiquen a una persona natural: nombre, RUT/RUN de persona,
  correo nominativo, teléfono, dirección, IP, geolocalización, fotos,
  documentos adjuntos, datos laborales, financieros o de salud.
- Marca los **datos sensibles** (salud, biometría, datos de menores, afiliación
  sindical, religión, orientación sexual, situación socioeconómica según
  contexto). Estos disparan los requisitos más exigentes.
- Distingue dato de persona jurídica (razón social, RUT de empresa) de dato de
  persona natural asociada (representante legal, contacto). Solo el segundo es
  dato personal protegido.

Produce un **inventario resumido** de hallazgos (tabla módulo → tabla → campo →
tipo → titular → sensibilidad). No tiene que ser exhaustivo a nivel de cada
columna trivial; prioriza lo relevante y de mayor riesgo.

### Paso 2 — Auditar cada categoría del checklist

Recorre las categorías de `references/checklist-auditoria.md`. Para cada ítem
aplicable, **busca evidencia real en el proyecto** (no asumas). Ejemplos de
señales a buscar:

- **Control de acceso / auth:** middleware de autenticación, guards/policies,
  roles, MFA, hashing de contraseñas (bcrypt/argon2), rate limiting, expiración
  de sesión, protección CSRF.
- **Cifrado / secretos:** HTTPS/TLS forzado, secretos en `.env` vs hardcodeados
  en el código (esto último es un hallazgo crítico), cifrado de campos/columnas,
  cifrado de backups.
- **Logs y auditoría:** existencia de una tabla/servicio de audit log; si se
  registran login, cambios de permisos, exportaciones y acceso a datos
  sensibles.
- **Derechos del titular:** endpoints o flujos de exportación, rectificación,
  supresión/anonimización y bloqueo; tabla de solicitudes.
- **Consentimiento:** tabla de consentimientos, checkboxes separados (no
  premarcados), versión de política registrada.
- **Transparencia:** existencia de política de privacidad y de avisos en
  formularios; enlace visible.
- **Conservación/eliminación:** existe alguna lógica/criterio de retención o
  anonimización, o los datos se guardan indefinidamente.
- **Dev/testing:** indicios de datos reales en seeders/fixtures/dumps en el
  repo (hallazgo relevante).
- **Proveedores / transferencias internacionales:** dependencias de terceros
  que procesan datos (hosting, email, pagos, analytics, IA), y dónde se alojan.
- **IA / decisiones automatizadas:** llamadas a APIs de IA o lógica de
  scoring/perfilamiento que decida sobre personas.

Para cada ítem asigna un **estado**:

| Estado | Símbolo | Significado |
|---|---|---|
| Implementado | ✅ | Hay evidencia concreta en el proyecto. |
| Parcial | ⚠️ | Existe en parte o con deficiencias (cítalas). |
| Faltante | ❌ | No hay evidencia y el ítem aplica. |
| No aplica | ➖ | El ítem no corresponde a este proyecto (justifica). |
| Requiere confirmación | ❓ | No es verificable solo desde el repo (proceso, contrato, infra externa, decisión legal). |

**Asigna un único estado por ítem.** No uses combinaciones tipo "✅/⚠️": si hay
una deficiencia, el estado es ⚠️ y la describes en la evidencia. Un ítem ambiguo
confunde al lector y no es accionable.

**Si una categoría completa no aplica** al proyecto (ej. no hay módulo de
adjuntos, o los backups viven solo en infraestructura externa), no llenes una
fila por cada ítem: resúmela en **una sola línea** ("Categoría M — No aplica: el
sistema no maneja archivos adjuntos") para no inflar el reporte. El reporte es un
backlog accionable, no un formulario que hay que completar entero.

**Cita la evidencia** con `ruta/archivo:línea` cuando exista, para que el
hallazgo sea defendible y accionable. La ley premia precisamente poder
**demostrar** el cumplimiento, así que la trazabilidad del reporte importa.

### Paso 3 — Priorizar y escribir el reporte

Usa la plantilla `assets/plantilla-reporte.md` como estructura exacta del
informe. Reglas:

- Ordena los hallazgos faltantes/parciales por prioridad: **Crítica** (debe
  estar antes de la vigencia), **Alta**, **Media**. La prioridad base de cada
  ítem está en el checklist; puedes subirla si el proyecto trata datos sensibles
  o de alto riesgo.
- En cada hallazgo accionable indica una **acción recomendada concreta** para
  ese stack (ej.: "agregar tabla `audit_log` y registrar exportaciones en el
  controlador `X`"), no una recomendación genérica.
- Incluye el **inventario de datos** del Paso 1 y un **resumen ejecutivo** con
  el % aproximado de cumplimiento por categoría y los 3–5 riesgos más urgentes.
- Incluye una sección de **controles ya implementados** (los ítems ✅) antes del
  backlog. Un reporte que solo lista lo que falta desmoraliza y oculta lo que el
  equipo hizo bien; resaltar lo correcto evita además que alguien "arregle" algo
  que ya cumple. Sé concreto: cita el control y su evidencia.
- Cierra con el recordatorio de que esto no reemplaza revisión legal formal.

Guarda el reporte como `CUMPLIMIENTO_LEY_21719.md` en la raíz del proyecto
auditado (o donde el usuario indique), y muéstrale el resumen ejecutivo en el
chat.

## Principios al auditar

- **No inventar estados.** Si algo no se ve en el repo, es ❓, no ❌ ni ✅. Un
  reporte que asume controles inexistentes o niega los que sí están es peor que
  no tener reporte.
- **Evidencia sobre opinión.** Cada ✅/⚠️/❌ se respalda con una ruta de archivo o
  con la ausencia comprobada de algo (ej.: "no existe ninguna tabla de
  consentimientos en las migraciones").
- **Proporcional al riesgo.** Profundiza en los módulos que tratan datos
  sensibles o masivos; no gastes el reporte detallando campos triviales.
- **Accionable.** El reporte es un backlog, no un ensayo. Cada faltante debe
  poder convertirse en una tarea.
