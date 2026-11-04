# ÁREA F — Integración, calidad y preparación comercial
## Informe técnico · Fase 2 · Auditoría Global V1.3

**Fuente:** `home/integration/connectors-20261022 @ 549700b9` · A0 (solo lectura).

| Requisito V1.3 | Evidencia | Estado |
|---|---|---|
| CI | `.github/workflows/ci.yml` (pytest sobre `tests/`) | codificado |
| Suite de tests | 203 ficheros (154 `tests/` + 41 `tests_bdm/` + 8 `core/tests/`) | codificado (no ejecutado aquí) |
| Manifiestos / perfiles / inventario | `manifests/`, `profiles/`, `inventory/` | codificado |
| Instalación repetible | `deployment/factory/` (Área A) | parcial (no ejecutado) |

## Hallazgos

1. **CI y tests existen** (pytest central, no por módulo), pero **no ejecutados** desde este entorno
   (prohibido por el mandato). Resultado histórico: **NO VERIFICADO**.
2. **Duplicación:** `home_ems/` y `home_discovery_source/ems.py` (lógica EMS paralela); `home_discovery`
   y `home_discovery_source`. Riesgo de mantenimiento.
3. **43 ramas** en `home`; la integración canónica es `integration/connectors-20261022 @ 549700b9`; hay
   ramas `integration/*` divergentes (discovery-unified, p2-core-discovery, p3-debian-ha-foundation).

*Informe A0. No autoriza desarrollo.*
