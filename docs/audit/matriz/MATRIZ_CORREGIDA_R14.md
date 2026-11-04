# MATRIZ DE TRAZABILIDAD CORREGIDA — R14

**Base:** `home @ 549700b9`. **Solo filas corregidas**; la matriz completa sigue en
`MATRIZ_TRAZABILIDAD_AUDITADA.md`.

| Capacidad | Implementación | Tests | Integración | Evidencia | Estado |
|---|---|---|---|---|---|
| Actuación HA | `HomeAssistantActionExecutor` (`action_executor.py:51-85`) | `tests/test_ha_adapter_core.py` (envío, deny, verificación) | `home_node/composition.py:238-264` + `kaenori-node.service` | Código + tests | 🟢 cod+int; 🟡 verificación por atributo |
| Egress jail | `egress.py`, `egress_compile.py`, `apply_egress.py` | `test_security_egress.py`, `test_egress_compile.py`, `test_apply_egress.py` | `kaenori-egress.service` | Tests; sin aplicación OS | 🟡 cod+tst; ❌ enforcement verificado |
| Backup | `backup.sh`, `backup-verify.sh`, `backup-restore.sh` | `tests/test_backup.py` | scripts de fábrica | Test verde; prueba vacía rc=0 | 🟡 test; ❌ fallo incompleto |
| Suite/CI | 132+34+8 `.py` + 6 `.sh` = **180** | **2533** casos pytest; 6 shell | CI solo `tests/` (1569) | `pytest --collect-only`; `ci.yml` | 🟡 38,1 % fuera de CI |
| Taxonomía 12 | `home_onboarding/vocabulary.py:144` | tests de onboarding | consumidores | Código | 🟢 |
| EMS activo | `home_discovery_source` | tests propios | 4 consumidores | grep | 🟢; `home_ems` sin consumidores |
| Proyección 12→5 | — | — | — | no código | ⚪ no codificada |
| Kill-switch recovery | `killswitch.py` arm/disarm; `policy.py` deny-all | tests de policy | — | sin CCR/safe-stop | ❌ no implementado |
