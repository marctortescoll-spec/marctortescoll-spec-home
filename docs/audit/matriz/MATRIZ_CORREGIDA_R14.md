# MATRIZ DE TRAZABILIDAD CORREGIDA — R14

**Base:** `home @ 549700b9`. **Solo filas corregidas**; la matriz completa sigue en
`MATRIZ_TRAZABILIDAD_AUDITADA.md`.

| Capacidad | Implementación | Tests | Integración | Evidencia | Estado |
|---|---|---|---|---|---|
| Actuación HA | `HomeAssistantActionExecutor` (`action_executor.py:51-85`) | `tests/test_ha_adapter_core.py` (envío, deny, verificación) | `home_node/composition.py:238-264` + `kaenori-node.service` | Código + tests | 🟢 cod+int; 🟡 verificación por atributo |
| Egress jail | `egress.py`, `egress_compile.py`, `apply_egress.py` | `test_security_egress.py`, `test_egress_compile.py`, `test_apply_egress.py` | `kaenori-egress.service` | Tests; sin aplicación OS | 🟡 cod+tst; ❌ enforcement verificado |
| Backup | `backup.sh`, `backup-verify.sh`, `backup-restore.sh` | `tests/test_backup.py` | scripts de fábrica | Test verde; prueba vacía rc=0 | 🟡 test; ❌ fallo incompleto |
| Suite/CI | 132+34+8 `.py` + **5 tests + 1 runner** `.sh` = **180** | **2533** casos recopilados; 5 shell tests | CI `tests/` sin synthetic: **1569 recopilados / 1563 passed / 1 failed / 5 skipped** | `pytest --collect-only`; ejecución exacta CI `549700b9` | ❌ CI rojo en SHA canónico; 38,1 % fuera |
| Egress persistencia | `kaenori-egress.service` `WantedBy=multi-user.target`; enabled/active en HOME0001 | tests unitarios | servicio habilitado | `systemctl is-enabled/is-active` | 🟡 reaplica si arranca; tabla no verificada sin root |
| Piloto solo lectura | telemetría/observación, sin actuación | tests de lectura | `home_ha_adapter` read-only | — | 🟢 separado del piloto físico |
| Taxonomía 12 | `home_onboarding/vocabulary.py:144` | tests de onboarding | consumidores | Código | 🟢 |
| EMS activo | `home_discovery_source` | tests propios | 4 consumidores | grep | 🟢; `home_ems` sin consumidores |
| Proyección 12→5 | — | — | — | no código | ⚪ no codificada |
| Kill-switch recovery | `killswitch.py` arm/disarm; `policy.py` deny-all | tests de policy | — | sin CCR/safe-stop | ❌ no implementado |
