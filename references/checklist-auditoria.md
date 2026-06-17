# Checklist maestro de auditoría — Ley N° 21.719

Lista de ítems auditables, agrupados por categoría. Cada ítem trae **cómo
verificarlo en el proyecto** y su **prioridad base**:

- **Crítica (C):** debe estar resuelto antes de la vigencia general (2026-12-01).
- **Alta (A):** importante; brecha real de cumplimiento o seguridad.
- **Media (M):** mejora de madurez / continuidad.

La prioridad puede **subir** si el proyecto trata datos sensibles, de menores o a
gran escala. Asigna a cada ítem uno de los estados: ✅ Implementado, ⚠️ Parcial,
❌ Faltante, ➖ No aplica, ❓ Requiere confirmación.

## Índice de categorías

- A. Inventario y clasificación de datos
- B. Finalidad y bases lícitas
- C. Consentimiento
- D. Derechos de los titulares
- E. Transparencia e información
- F. Control de acceso y autorización
- G. Autenticación
- H. Cifrado y gestión de secretos
- I. Logs y auditoría
- J. Backups
- K. Ambientes de desarrollo y testing
- L. Conservación, eliminación y anonimización
- M. Archivos adjuntos
- N. Proveedores / encargados y transferencias internacionales
- O. Tratamientos de alto riesgo (IA, perfilamiento, CCTV, salud)
- P. Incidentes de seguridad
- Q. Gobierno interno y evidencias

---

## A. Inventario y clasificación de datos

| ID | Ítem | Cómo verificar | Prioridad |
|---|---|---|---|
| A1 | Identificar todos los campos que son datos personales | Revisar esquema BD, modelos, migraciones, formularios | C |
| A2 | Identificar campos sensibles (salud, biometría, menores, etc.) | Buscar columnas tipo diagnóstico, licencia médica, huella, etc. | C |
| A3 | Existe una clasificación de datos en el sistema | Buscar tabla tipo `data_field_registry` o documentación equivalente | A |
| A4 | Separar dato de persona jurídica vs persona natural | Revisar si RUT/razón social se distingue de RUN/contacto | M |
| A5 | Separar datos sensibles en tablas/permisos más restrictivos | Revisar si salud/sueldos están aislados con control propio | A |

## B. Finalidad y bases lícitas

| ID | Ítem | Cómo verificar | Prioridad |
|---|---|---|---|
| B1 | Cada tratamiento/módulo tiene finalidad documentada | Buscar documentación o campo de finalidad por módulo | A |
| B2 | Cada finalidad tiene una base lícita registrada | Buscar matriz de bases lícitas (contrato, ley, consentimiento, interés legítimo) | C |
| B3 | Minimización: no se recolectan datos innecesarios | Revisar formularios/tablas por campos excesivos (ej. RUN cuando basta correo) | A |
| B4 | Calidad: el titular puede actualizar/corregir sus datos | Buscar flujos de edición y validación | M |

## C. Consentimiento

| ID | Ítem | Cómo verificar | Prioridad |
|---|---|---|---|
| C1 | Existe registro de consentimientos | Buscar tabla tipo `consent_records` (titular, finalidad, versión, fecha, ip) | A |
| C2 | Consentimientos separados por finalidad (no un "acepto todo") | Revisar formularios de registro/marketing | A |
| C3 | Sin casillas premarcadas | Revisar checkboxes en frontend (`checked` por defecto) | A |
| C4 | Se versiona el texto de política aceptado | Verificar que el consentimiento guarda versión de política | M |

## D. Derechos de los titulares

| ID | Ítem | Cómo verificar | Prioridad |
|---|---|---|---|
| D1 | Acceso: se puede exportar los datos de un titular | Buscar endpoint/flujo de exportación por persona | C |
| D2 | Rectificación: se pueden corregir datos | Buscar flujo de edición/corrección | A |
| D3 | Supresión: se puede borrar o anonimizar | Buscar lógica de delete/anonimización por titular | C |
| D4 | Oposición: se puede excluir de una finalidad (ej. marketing) | Buscar flag/exclusión por finalidad | A |
| D5 | Portabilidad: exportación en formato estructurado (CSV/JSON) | Verificar formato de la exportación de D1 | M |
| D6 | Bloqueo temporal: estado "bloqueado" en registros | Buscar estado de bloqueo/suspensión de tratamiento | A |
| D7 | Registro de solicitudes con folio y SLA | Buscar tabla tipo `privacy_requests` con `due_at` (30 días / 2 días hábiles bloqueo) | A |

## E. Transparencia e información

| ID | Ítem | Cómo verificar | Prioridad |
|---|---|---|---|
| E1 | Existe política de privacidad visible y versionada | Buscar página/archivo de política y su versión | C |
| E2 | Avisos de privacidad en formularios que capturan datos | Revisar formularios públicos por texto + enlace a política | A |
| E3 | Se informa si se comparten datos con terceros | Revisar contenido de la política | M |
| E4 | Canal para ejercer derechos (correo/formulario de privacidad) | Buscar canal de contacto de privacidad | A |

## F. Control de acceso y autorización

| ID | Ítem | Cómo verificar | Prioridad |
|---|---|---|---|
| F1 | Roles/perfiles implementados | Buscar sistema de roles/permisos | C |
| F2 | Mínimo privilegio y separación lectura/edición/exportación/eliminación | Revisar granularidad de permisos | A |
| F3 | Autorización validada en TODOS los endpoints | Revisar que no haya endpoints sin guard/policy | C |
| F4 | Sin cuentas compartidas; accesos individuales | Revisar usuarios genéricos (admin/admin) | A |
| F5 | Revocación de accesos al terminar relación | Verificar baja de usuarios / desactivación | M |

## G. Autenticación

| ID | Ítem | Cómo verificar | Prioridad |
|---|---|---|---|
| G1 | Contraseñas con hash robusto (bcrypt/argon2), nunca texto plano | Revisar cómo se almacenan/validan passwords | C |
| G2 | MFA para administradores | Buscar implementación de 2FA/MFA | C |
| G3 | Protección contra fuerza bruta / rate limiting en login | Buscar throttling/rate limit | A |
| G4 | Expiración de sesión y revocación de tokens | Revisar config de sesión/JWT | A |
| G5 | Recuperación de contraseña segura (tokens expirables) | Revisar flujo de reset | A |
| G6 | Protección CSRF/XSS según arquitectura | Revisar middleware/escapado de salida | A |

## H. Cifrado y gestión de secretos

| ID | Ítem | Cómo verificar | Prioridad |
|---|---|---|---|
| H1 | HTTPS/TLS obligatorio | Revisar config de servidor / redirección a https | C |
| H2 | Secretos fuera del código (env/secret manager) | **Buscar credenciales hardcodeadas en el repo** (hallazgo crítico) | C |
| H3 | Cifrado de campos críticos cuando corresponda | Revisar si RUN/financieros/salud se cifran en BD | A |
| H4 | Cifrado de backups | Verificar en proceso/infra de respaldo | A |
| H5 | Gestión segura de llaves | Revisar dónde viven las llaves de cifrado | M |

## I. Logs y auditoría

| ID | Ítem | Cómo verificar | Prioridad |
|---|---|---|---|
| I1 | Existe bitácora de auditoría | Buscar tabla/servicio tipo `audit_log` | C |
| I2 | Se registran accesos administrativos y logins | Revisar qué eventos se loggean | A |
| I3 | Se registran exportaciones de datos | Revisar logging en flujos de export | A |
| I4 | Se registra acceso/modificación de datos sensibles | Revisar logging en módulos sensibles | A |
| I5 | Los logs tienen plazo de conservación y acceso restringido | Verificar retención de logs (ellos también son datos personales) | M |

## J. Backups

| ID | Ítem | Cómo verificar | Prioridad |
|---|---|---|---|
| J1 | Backups automáticos | Verificar job/cron de respaldo | A |
| J2 | Backups cifrados | Ver H4 | A |
| J3 | Pruebas de restauración | Verificar procedimiento (normalmente ❓ desde el repo) | M |
| J4 | Retención definida y acceso restringido | Verificar política de retención de backups | M |

## K. Ambientes de desarrollo y testing

| ID | Ítem | Cómo verificar | Prioridad |
|---|---|---|---|
| K1 | No usar datos reales en desarrollo | **Buscar dumps/seeders/fixtures con datos reales en el repo** | A |
| K2 | Usar datos sintéticos o anonimizados | Revisar seeders/factories | M |
| K3 | Separación producción/desarrollo | Revisar configs de entorno | A |

## L. Conservación, eliminación y anonimización

| ID | Ítem | Cómo verificar | Prioridad |
|---|---|---|---|
| L1 | Plazos de conservación definidos por tipo de dato | Buscar reglas/tabla de retención | A |
| L2 | Mecanismo de eliminación de datos vencidos | Buscar jobs de limpieza/expiración | A |
| L3 | Mecanismo de anonimización | Buscar lógica de anonimización | A |
| L4 | No se guardan datos indefinidamente sin justificación | Revisar si hay borrado lógico permanente sin criterio | M |

## M. Archivos adjuntos

| ID | Ítem | Cómo verificar | Prioridad |
|---|---|---|---|
| M1 | Adjuntos almacenados fuera del directorio público | Revisar ruta de uploads | A |
| M2 | Validación de tipo y tamaño | Revisar validación de subida | M |
| M3 | Control de permisos / enlaces temporales para descarga | Revisar acceso a archivos | A |
| M4 | Registro de accesos/descargas de adjuntos | Revisar logging de descargas | M |

## N. Proveedores / encargados y transferencias internacionales

| ID | Ítem | Cómo verificar | Prioridad |
|---|---|---|---|
| N1 | Identificar terceros que procesan datos | Revisar dependencias: hosting, email, pagos, analytics, IA, CRM | A |
| N2 | Existe contrato/anexo de tratamiento (DPA) con cada encargado | Normalmente ❓ desde el repo; listar proveedores a regularizar | A |
| N3 | Identificar transferencias internacionales (datos fuera de Chile) | Detectar servicios en EE.UU./UE (AWS, Google, Stripe, OpenAI, etc.) | A |
| N4 | Documentar país de almacenamiento y garantías | Marcar para matriz de transferencia | M |
| N5 | Prohibir que el proveedor use datos para fines propios / entrenar IA | Revisar términos del proveedor | A |

## O. Tratamientos de alto riesgo (IA, perfilamiento, CCTV, salud)

| ID | Ítem | Cómo verificar | Prioridad |
|---|---|---|---|
| O1 | Detectar decisiones automatizadas / scoring / perfilamiento | Buscar lógica que clasifique o decida sobre personas | A |
| O2 | Detectar envío de datos personales reales a IA externa | Buscar llamadas a APIs de IA y qué datos se mandan | C |
| O3 | Revisión humana en decisiones con efectos relevantes | Verificar que la IA no decide sola | A |
| O4 | Registro de decisiones automatizadas | Buscar log de decisiones (input, modelo, resultado) | M |
| O5 | Evaluación de impacto para tratamientos de alto riesgo | Verificar existencia de DPIA (normalmente ❓) | A |
| O6 | CCTV/biometría con finalidad, aviso, plazo y acceso restringido | Aplica si hay cámaras/biometría | A |

## P. Incidentes de seguridad

| ID | Ítem | Cómo verificar | Prioridad |
|---|---|---|---|
| P1 | Existe procedimiento de respuesta a incidentes | Buscar documento/proceso (normalmente ❓) | C |
| P2 | Registro de incidentes | Buscar tabla tipo `security_incidents` | A |
| P3 | Capacidad de notificar a la Agencia y a titulares | Verificar que el proceso lo contemple | A |
| P4 | Detección/alertas de accesos inusuales | Buscar monitoreo/alertas | M |

## Q. Gobierno interno y evidencias

| ID | Ítem | Cómo verificar | Prioridad |
|---|---|---|---|
| Q1 | Responsable de privacidad designado | Normalmente ❓ desde el repo | A |
| Q2 | Inventario de tratamientos mantenido | Ver categoría A / documentación | A |
| Q3 | Se guardan evidencias (versiones de política, consentimientos, solicitudes) | Revisar persistencia de evidencias | M |
| Q4 | Modelo de cumplimiento / auditorías periódicas | Normalmente ❓ | M |

---

## Checklist de salida a producción (resumen rápido)

Para sistemas nuevos, verificar antes de publicar: política de privacidad (E1),
aviso en formularios (E2), finalidad+base lícita por dato (B1/B2),
consentimientos si aplica (C1), derechos de acceso/rectificación/supresión/
bloqueo (D1–D6), control de acceso por rol (F1), MFA admins (G2), logs de
acciones críticas (I1), backups protegidos (J2), sin datos reales en testing
(K1), proveedores identificados (N1), transferencias documentadas (N3),
procedimiento de incidentes (P1), evaluación de alto riesgo/IA (O5), plazo de
conservación (L1).
