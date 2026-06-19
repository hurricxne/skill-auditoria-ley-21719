# auditoria-ley-21719

Skill para **[Claude Code](https://claude.com/claude-code)** que audita un
proyecto/codebase contra la **Ley N° 21.719 de Protección de Datos Personales de
Chile** y genera un **reporte de cumplimiento priorizado**.

La Ley N° 21.719 (publicada el 13-12-2024) moderniza la antigua Ley N° 19.628,
crea la Agencia de Protección de Datos Personales y entra en **vigencia general
el 1 de diciembre de 2026**. Este skill ayuda a los equipos de desarrollo a
diagnosticar, sistema por sistema, qué controles técnicos y legales les faltan
antes de esa fecha.

> ⚠️ Es una **pauta técnica de cumplimiento, no asesoría legal**. No reemplaza
> una revisión legal formal, especialmente en tratamientos de alto riesgo
> (datos de salud, biometría, menores, perfilamiento con IA).

## Qué hace

Cuando le pides auditar un proyecto, el skill:

1. **Detecta el stack y tipo de sistema** (lee `CLAUDE.md` si existe, framework,
   motor de BD).
2. **Descubre los datos personales y sensibles** que trata el sistema (esquema
   de BD, modelos, migraciones, formularios) — el paso fundamental.
3. **Audita 17 categorías** (control de acceso, autenticación, cifrado, logs y
   auditoría, derechos del titular, consentimiento, transparencia, conservación,
   proveedores, transferencias internacionales, incidentes, IA/perfilamiento,
   gobierno interno, etc.), buscando **evidencia real en el código** y citándola
   con `archivo:línea`.
4. **Genera `CUMPLIMIENTO_LEY_21719.md`** en la raíz del proyecto auditado: con
   resumen ejecutivo, inventario de datos, hallazgos priorizados
   (Crítica / Alta / Media) con acción recomendada concreta, e ítems que
   requieren confirmación.

Cada ítem recibe un único estado: ✅ Implementado · ⚠️ Parcial · ❌ Faltante ·
➖ No aplica · ❓ Requiere confirmación. **No inventa estados**: lo que no se
puede verificar desde el repositorio (contratos, procesos, infraestructura
externa) se marca explícitamente como "requiere confirmación".

## Instalación

### Como skill personal (todos tus proyectos)

```bash
git clone https://github.com/hurricxne/skill-auditoria-ley-21719.git \
  ~/.claude/skills/auditoria-ley-21719
```

En Windows (PowerShell):

```powershell
git clone https://github.com/hurricxne/skill-auditoria-ley-21719.git `
  "$env:USERPROFILE\.claude\skills\auditoria-ley-21719"
```

### Como skill de un proyecto

Clónalo dentro de `.claude/skills/auditoria-ley-21719` en la raíz del proyecto.

## Uso

En Claude Code, dentro del proyecto que quieres auditar:

```
/auditoria-ley-21719
```

O simplemente pídelo en lenguaje natural:

> "Audita este proyecto contra la Ley 21.719 y dime qué me falta implementar."
> "Revisa el cumplimiento de protección de datos de este sistema."

El skill también se activa cuando mencionas "ley de datos personales",
"protección de datos", "datos sensibles", "derechos del titular", o pides saber
qué controles te faltan para cumplir la normativa chilena.

## Estructura

```
auditoria-ley-21719/
├── SKILL.md                          # Workflow de auditoría + formato del reporte
├── references/
│   ├── checklist-auditoria.md        # Checklist maestro: 17 categorías, ~70 ítems
│   └── contexto-legal.md             # Referencia legal condensada (autónoma)
└── assets/
    └── plantilla-reporte.md          # Plantilla del reporte de cumplimiento
```

## Categorías auditadas

| | Categoría | | Categoría |
|---|---|---|---|
| A | Inventario y clasificación de datos | J | Backups |
| B | Finalidad y bases lícitas | K | Ambientes de desarrollo y testing |
| C | Consentimiento | L | Conservación, eliminación y anonimización |
| D | Derechos de los titulares | M | Archivos adjuntos |
| E | Transparencia e información | N | Proveedores y transferencias internacionales |
| F | Control de acceso y autorización | O | Tratamientos de alto riesgo (IA, CCTV, salud) |
| G | Autenticación | P | Incidentes de seguridad |
| H | Cifrado y gestión de secretos | Q | Gobierno interno y evidencias |
| I | Logs y auditoría | | |

## Fuentes oficiales

- [BCN — Ley N° 21.719](https://www.bcn.cl/leychile/navegar?idNorma=1209272)
- [Gob.cl — nota explicativa](https://www.gob.cl/noticias/ley-proteccion-datos-personales-aprobacion-eleva-estandar-derechos/)
- [Secretaría de Gobierno Digital — guía práctica de implementación](https://wikiguias.digital.gob.cl/datos-personales/guia-practica-implementacion-nueva-ley-datos-personales)

## Licencia

MIT — ver [LICENSE](LICENSE).

## Aviso

Este proyecto es una herramienta de apoyo al cumplimiento y **no constituye
asesoría jurídica**. La responsabilidad del cumplimiento de la Ley N° 21.719
recae en el responsable del tratamiento de datos.
