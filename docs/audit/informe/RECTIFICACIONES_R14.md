# INFORME DE RECTIFICACIONES — Ronda 14
## Contraste técnico independiente · `home @ 549700b9`

**Autoridad:** A0, solo lectura. **No** modifica producto, permisos ni despliegue.
**Fuente:** `home @ 549700b9c25c2336af8f699cbc1cf1270145fc96`.
**Contrasta:** Auditoría Global V1.3 (documental `463d996`).

## Tabla de rectificaciones

| # | Afirmación original | Evidencia encontrada | Corrección | Efecto |
|---|---|---|---|---|
| 1 | “Conectores solo-lectura; no hay capacidad de escritura” | `home_ha_adapter_core/action_executor.py:51-85` implementa `HomeAssistantActionExecutor`; `home_node/composition.py:238-264` lo cablea en `CoreService`; `kaenori-node.service` ejecuta `home_node` | **Sí hay actuación codificada e integrada** vía HA. `kaenori-ha-adapter.service` es el adaptador de telemetría read-only; no es el executor | El bloqueo es CCR/gate, no ausencia de escritura |
| 2 | “Egress declarativo, sin enforcement” | `home_security/egress.py:8-11` declara `is_enforced=False`; `apply_egress.py:49-51` ejecuta `nft -f`; `kaenori-egress.service:14-18` aplica y **borra la tabla al parar** | **Enforcement codificado, no verificado en hierro**; sin snapshot/rollback real; stop = fail-open; sin persistencia tras reboot | Pasa a “implementado sin verificación A3”; añadir rollback/persistencia |
| 3 | “Backup sin test dedicado” | `tests/test_backup.py` existe; prueba backup/verify/restore dry-run+real | **Sí hay test**, pero **no cubre fallo de creación** | Ver rectificación 7 |
| 4 | “203 ficheros de test” | `tests/*.py`=132, `tests_bdm/*.py`=34, `core/tests/*.py`=8, `deployment/factory/tests/*.sh`=6 → **180**; `pytest --collect-only` da **1569 + 879 + 85 = 2533** casos | **180 ficheros; 2533 casos**. CI ejecuta solo `tests/` = **1569**; quedan fuera **964 (38,1 %)** | Corregir cifra y ampliar CI |
| 5 | “12 categorías en `home_connectivity/vocabulary.py`” | `home_onboarding/vocabulary.py:144-157` define `CATEGORY_KEYWORDS` (12); `home_connectivity/vocabulary.py` es otro vocabulario | **Ruta corregida**: taxonomía en `home_onboarding/vocabulary.py` | Sin cambio de alcance |
| 6 | “Duplicación `home_ems` vs `home_discovery_source/ems.py`” | `home_discovery_source` es importado por `home_node/catalog_service.py`, `home_connectivity/discovery_bridge.py`, `home_catalog/observation.py`, `core/home_experience/brain.py`; `home_ems` no tiene consumidores externos | **`home_discovery_source` es el activo**; `home_ems` parece muerto/paralelo | Consolidar a favor del activo, con deprecación segura |
| 7 | “Backup con test = backup fiable” | `backup.sh:30,34` usa `tar ... 2>/dev/null || true`; con `data` vacío devuelve **rc=0**, archivo de **45 bytes**, `backup-verify.sh` devuelve **OK** | **Un backup incompleto puede declararse satisfactorio.** El test no cubre `tar` fallido | **P0**: quitar `|| true`, comprobar `tar`, validar contenido/manifest y añadir test de fallo |
| 8 | “Bug de clima: compara modo con `expected_state`” | `domain_catalog.py:125-137` declara `climate.set_temperature` y `set_hvac_mode` con `expected_state=None`; `action_executor.py:74-84` envía primero y, si no hay `expected_state`, devuelve `verification_unavailable` | **No compara modo**: no verifica nada y **la orden ya salió**. Además `set_brightness` verifica solo `on`, no el nivel; `set_position`/`set_volume` no verifican el atributo | **No confundir `verified=False` con ausencia de efecto**; corregir read-back por atributo |
| 9 | “Safe-stop/kill-switch implementado” | `home_security/killswitch.py:68-75` solo `arm/disarm`; `policy.py:68-70` deny-all al armar; no hay máquina de estados CCR ni safe-stop | **No implementado**; solo flag deny-all | La recuperación segura es propuesta, no funcionalidad |

## Confirmado sin rectificación

- Proyección 12→5: **no codificada**.
- CCR/Override/Oportunidades: **no codificados**; solo especificación.
- Kill-switch armado: **deny-all**; no hay bypass.
- Instalación real/aislamiento efectivo: **NO VERIFICADO**.
- Cifra 203: incorrecta; usar 180 ficheros/2533 casos.

## Evidencia reproducible

- `python3 -m pytest tests/ tests_bdm/ core/tests/ --collect-only` → 2533.
- `backup.sh` con directorio vacío → rc=0, 45 bytes, verify OK (prueba de fallo silencioso).
- `git show 549700b9:...` para cada archivo citado.

*Documento A0. No autoriza implementación ni despliegue.*
