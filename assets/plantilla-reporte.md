# Reporte de cumplimiento — Ley N° 21.719

**Proyecto:** {{nombre_proyecto}}
**Tipo de sistema:** {{CRM / RR.HH. / SaaS / pagos / soporte / etc.}}
**Stack:** {{lenguaje, framework, BD}}
**Fecha de auditoría:** {{YYYY-MM-DD}}
**Auditado por:** Claude (skill auditoria-ley-21719)

> Pauta técnica de cumplimiento, **no asesoría legal**. Los puntos
> administrativos, contractuales y de alto riesgo requieren revisión legal
> formal. Vigencia general de la ley: **1 de diciembre de 2026**.

---

## 1. Resumen ejecutivo

{{2–4 frases: estado general del proyecto frente a la ley, nivel de madurez,
y el mensaje principal.}}

**Cumplimiento por categoría (aprox.):**

| Categoría | Implementado | Parcial | Faltante | N/A o por confirmar |
|---|---:|---:|---:|---:|
| A. Inventario y clasificación | | | | |
| B. Finalidad y bases lícitas | | | | |
| C. Consentimiento | | | | |
| D. Derechos de los titulares | | | | |
| E. Transparencia | | | | |
| F. Control de acceso | | | | |
| G. Autenticación | | | | |
| H. Cifrado y secretos | | | | |
| I. Logs y auditoría | | | | |
| J. Backups | | | | |
| K. Dev/testing | | | | |
| L. Conservación/eliminación | | | | |
| M. Adjuntos | | | | |
| N. Proveedores/transferencias | | | | |
| O. Alto riesgo / IA | | | | |
| P. Incidentes | | | | |
| Q. Gobierno interno | | | | |

**Top riesgos a resolver primero:**

1. {{riesgo crítico 1}}
2. {{riesgo crítico 2}}
3. {{riesgo crítico 3}}

---

## 2. Inventario de datos personales detectado

| Módulo | Tabla / entidad | Campo | Tipo | Titular | Sensible | Riesgo |
|---|---|---|---|---|---|---|
| {{...}} | | | personal/financiero/laboral/salud | cliente/trabajador/usuario | sí/no | bajo/medio/alto |

{{Comentario sobre los hallazgos de datos más relevantes y los datos sensibles
detectados.}}

---

## 3. Hallazgos por prioridad

### 3.1 Prioridad CRÍTICA (resolver antes de la vigencia)

| ID | Ítem | Estado | Evidencia | Acción recomendada |
|---|---|:--:|---|---|
| {{H2}} | {{Secretos fuera del código}} | ❌ | `config/db.php:12` (credenciales hardcodeadas) | Mover a variables de entorno / secret manager |

### 3.2 Prioridad ALTA

| ID | Ítem | Estado | Evidencia | Acción recomendada |
|---|---|:--:|---|---|
| | | | | |

### 3.3 Prioridad MEDIA

| ID | Ítem | Estado | Evidencia | Acción recomendada |
|---|---|:--:|---|---|
| | | | | |

---

## 4. Ítems que requieren confirmación (no verificables desde el código)

Estos puntos dependen de procesos, contratos o infraestructura externa. Listados
para que el responsable los confirme:

- {{N2 — ¿Existe DPA con el proveedor de hosting?}}
- {{P1 — ¿Hay procedimiento documentado de respuesta a incidentes?}}
- {{Q1 — ¿Hay responsable de privacidad designado?}}

---

## 5. Backlog sugerido (orden de ejecución)

1. {{tarea}}
2. {{tarea}}
3. {{...}}

---

## 6. Nota final

Este reporte refleja el estado del proyecto **a la fecha de auditoría** y según
lo verificable en el repositorio. La ley exige poder **demostrar** el
cumplimiento; conservar evidencia (versiones de política, registros de
consentimiento, solicitudes de titulares, contratos con proveedores, pruebas de
restauración) es tan importante como implementar los controles. Se recomienda una
revisión legal formal para los puntos administrativos y de alto riesgo.
