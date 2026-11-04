# INFORME DE RECTIFICACIONES — Ronda 14
## Contraste técnico de la Auditoría Global V1.3

**Fuente:** `home @ 549700b9` (canónica) · A0 solo lectura · contrasta y corrige el informe global
(commit `463d996`).

## Tabla de rectificaciones (afirmación → evidencia → corrección)

| # | Afirmación original (auditoría) | Evidencia encontrada (549700b9) | Conclusión corregida | Efecto en el plan |
|---|---|---|---|---|
| 1 | "Conectores solo-lectura → no hay capacidad de escritura" | `home_ha_adapter_core/action_executor.py` implementa `HomeActionExecutor` (POST `/api/services/<domain>/<service>` a HA, solo acciones declaradas en `ActionRegistry`, `verified` solo con read-back). `state_reader.py`, `client.py`, `domain_catalog.py`, `registry.py` presentes | **Sí existe capacidad de escritura codificada**, vía HA, con deny-by-default y verificación por read-back | El piloto sí tiene vía de actuación; el bloqueo real es **CCR/recuperación**, no la ausencia de escritura |
| 2 | "Egress declarativo, sin enforcement" | `home_security/apply_egress.py` aporta `render_ruleset` → `compile_nftables` → `write_ruleset` → `apply_ruleset` (`nft -f`, A3, con snapshot/rollback). Tests: `tests/test_security_egress.py`, `test_egress_compile.py`, `test_apply_egress.py`. Unit `kaenori-egress.service` | **El enforcement está codificado** (camino nftables), pero exige **A3 (root + nft)** y **no hay evidencia de aplicación efectiva** | Ajustar de "no implementado" a "implementado sin verificar (A3)" |
| 3 | "Backup sin test dedicado" | `tests/test_backup.py` existe | **Sí hay test de backup** | Eliminar esa carencia del registro |
| 4 | "203 ficheros de test" | Recontado sobre 549700b9: `tests/*.py`=132 · `tests_bdm/*.py`=34 · `core/tests/*.py`=8 · `factory/tests`=6 → **180** (mi "203" contaba ficheros no-.py). `ci.yml` ejecuta **solo `pytest tests/`** (132); deja fuera `tests_bdm/`, `core/tests/` y `factory/tests` | **La cifra correcta es ~180**, y **el CI solo ejecuta 132** (tests/), no toda la suite | Revisar el CI para cubrir tests_bdm/core/factory |
| 5 | "12 categorías en `home_connectivity/vocabulary.py`" | `CATEGORY_KEYWORDS` (12) está en **`home_onboarding/vocabulary.py`** (y `home_scout/conversation/categories.py`); `home_connectivity/vocabulary.py` es otro vocabulario (conectividad) | **Corregir la ruta**: la taxonomía técnica vive en `home_onboarding/vocabulary.py` | — |
| 6 | "Duplicación `home_ems` vs `home_discovery_source/ems.py`" | `home_discovery_source` es consumido por 4 módulos (`home_node/catalog_service.py`, `home_connectivity/discovery_bridge.py`, `home_catalog/observation.py`, `core/home_experience/brain.py`); `home_ems` **no tiene consumidores** en esta rama | **`home_discovery_source` es el activo; `home_ems` parece muerto/paralelo.** La consolidación debe decidir cuál es el canónico | Consolidar a favor de `home_discovery_source` o reconectar `home_ems` |

## Confirmado (sin rectificación)

- Proyección 12→5 verticales de producto: **NO codificada** (grep de los 5 nombres de vertical = vacío).
- CCR / Override / Oportunidades (Anexo A): **NO codificados** (solo especificación).
- Kill-switch solo bloquea nuevas órdenes: **confirmado**.
- Instalación real: **NO VERIFICADA** (manifiesto "NO ejecutado").

*Informe A0. No autoriza desarrollo, despliegue ni actuación.*
