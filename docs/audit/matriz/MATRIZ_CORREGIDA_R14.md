# MATRIZ DE TRAZABILIDAD CORREGIDA — R14 (deltas sobre la matriz auditada)
## Solo filas corregidas tras el contraste técnico

| Capacidad | Impl corregida | Valid corregida | Evidencia | Nota |
|---|---|---|---|---|
| Capacidad de actuación (escritura) | 🟢 (era ⚪) | cod | `home_ha_adapter_core/action_executor.py` (HomeActionExecutor, HA services, ActionRegistry, read-back) | No "solo-lectura"; el bloqueo real es CCR |
| Egress jail | 🟡→🟡 (matiz) | cod + tst | `apply_egress.py` (nftables) + `tests/test_egress_compile.py`, `test_apply_egress.py`, `test_security_egress.py` | Enforcement codificado, requiere A3; no verificado |
| Backup | 🟢 (era 🟡) | cod + tst | `deployment/factory/scripts/backup*.sh` + `tests/test_backup.py` | Sí hay test |
| Suite de tests | 180 ficheros (era 203) | cod (no ejecutada) | `tests/*.py`=132 · `tests_bdm/*.py`=34 · `core/tests/*.py`=8 · `factory/tests`=6 | CI solo ejecuta `pytest tests/` (132) |
| Taxonomía 12 categorías | 🟢 | cod | **`home_onboarding/vocabulary.py`** (`CATEGORY_KEYWORDS`) | Corregida la ruta (no home_connectivity) |
| EMS (activo) | 🟢 | cod + int | **`home_discovery_source/`** (consumido por 4 módulos) | `home_ems/` sin consumidores (a consolidar) |
| Proyección 12→5 | ⚪ | doc | (no codificado) | sin cambio |

*Matriz delta A0. La matriz completa vigente sigue siendo `MATRIZ_TRAZABILIDAD_AUDITADA.md`.*
