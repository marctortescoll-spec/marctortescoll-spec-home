# PRECISIONES DEL OWNER — 5 comprobaciones (cierre técnico R14)
## Auditoría complementaria · Fuente `home @ 549700b9` · A0

## 1. EMS: implementación realmente utilizada

- **Servicios systemd** (`deployment/factory/units/`): arrancan `home_node` (nodo), `home_node.catalog_service`
  (catálogo), `home_catalog.service` (experiencia), `home_experience`, `home_ha_adapter`, `home_security.apply_egress`.
  **Ninguno arranca `home_ems` ni `home_discovery_source` como `-m`** (son librerías, no servicios).
- **Consumidores Python:** `home_discovery_source` es importado por `home_node/catalog_service.py`,
  `home_connectivity/discovery_bridge.py`, `home_catalog/observation.py`, `core/home_experience/brain.py`.
  `home_ems` **no tiene consumidores** (grep vacío fuera de `home_ems/` y tests).
- **Deployment:** las únicas referencias a "ems" en `deployment/` son paquetes YAML de HA
  (`deployment/debian/home-assistant/packages/ems/*.yaml`, observación V2C/ACS/energía/aquarea), **no** el
  módulo Python `home_ems`.

**Conclusión:** `home_discovery_source` es el EMS activo (vía `home_node`). `home_ems` (Python) parece
muerto, **pero** antes de eliminarlo hay que (a) comprobar sus tests propios, (b) confirmar que no hay
import dinámico/plugins, y (c) marcar una migración segura (deprecación + commit de retirada). **No se
elimina en A0.**

## 2. Tests y CI: ficheros ≠ casos ≠ suites

- **Ficheros** `.py` de test: 132 (tests/) + 34 (tests_bdm/) + 8 (core/tests/) + 6 (factory shell) = **180**.
- **Casos** (funciones `def test_`): tests/ = **1558** · tests_bdm/ = **920** · core/tests/ = **85** → **~2563 casos**.
- **CI (`ci.yml`)**: ejecuta **solo `pytest tests/`** = **1558 casos**. Quedan fuera **1005 casos** (tests_bdm/
  920 + core/tests/ 85) y los 6 shell de factory. → **~39 % de la suite no corre en CI.**

**Propuesta de cobertura completa:** ampliar `ci.yml` a `pytest tests/ tests_bdm/ core/tests/` + script
factory. No asumir "180 ficheros = 180 tests": son **~2563 casos**.

## 3. Egress: rollback, stop, DNS, recuperación

- `kaenori-egress.service` = `Type=oneshot`, `RemainAfterExit=yes`. `ExecStart` compila allowlist → nft
  (`--apply`). `ExecStop` = `nft delete table inet kaenori_egress` (**quita la jaula** al detenerse).
- `apply_egress.py`: `render_ruleset` (deny-by-default, config inválida → RuntimeError fail-closed),
  `write_ruleset` (0600), `apply_ruleset` (`nft -f`, root). **Snapshost/rollback declarado en el docstring,
  no en `main()`** (no vi captura de estado previo en el flujo real).
- **Allowlist** (`egress-allowlist.json`): solo CIDRs privadas/multicast/link-local IPv6 (192.168/16, 10/8,
  172.16/12, 224/4, ff02::/16) → **deniega Internet público**. **No hay regla específica de DNS/puertos**:
  DNS público (p. ej. 8.8.8.8) queda bloqueado; solo resolvería un DNS dentro de las CIDRs privadas.
- **Riesgos:** (1) *stop/crash* → `ExecStop` retira la jaula (fail-open al detenerse); (2) *reinicio* → las
  tablas nftables no persisten por defecto (sin save/restore visto) → aislamiento perdido tras reboot;
  (3) DNS grueso (CIDR, sin granularidad de puerto).

**Conclusión:** el código nftables existe, pero **el aislamiento efectivo no está garantizado** (fail-open
al parar, no persistente en reboot, DNS grueso). Requiere verificación A3 en hierro.

## 4. Actuación HA: órdenes sin confirmación (consignas de clima)

- `HomeAssistantActionExecutor.execute` **envía primero** (`call_service`) **y luego verifica**
  (`_verify_state`). Por tanto, **`verified=False` NO implica ausencia de efecto físico**: la orden ya
  salió por HTTP.
- Códigos de no-verificado: `verification_unavailable` (acción sin `expected_state` declarado),
  `verification_failed` (estado no confirmado en 3 s), `command_expired`, `action_undeclared`,
  `parameters_invalid`, `system.local_error`.
- **Bug de verificación en consignas de clima:** `_verify_state` compara `get_state(entity_id).get("state")`
  con `expected_state`. Para `climate.*`, `state` es el **modo** ("heat"/"cool"/"off"), **no la consigna de
  temperatura** (que es un **atributo** `temperature`). → La verificación de una consigna de clima **no
  comprueba la temperatura**; puede darse por "verificada" el modo pero no la consigna, o fallar sin
  confirmar un cambio físico que sí ocurrió.
- El target `entity_id` y los servicios salen **siempre** del `ActionRegistry` (cerrado), nunca del Command.

**Requisitos antes de activación física:** (1) declarar `expected_state` verificando el **atributo
correcto** (no el modo) para consignas; (2) read-back de temperatura/consigna con timeout; (3) idempotencia
y "última orden ganó"; (4) CCR/safe-stop implementados antes de exponer actuación.

## 5. CCR Clase B: el timeout de software no sustituye una garantía física

Estrategia de "estado seguro" de Clase B (sin TTL nativo) frente a modos de fallo:

| Fallo | ¿El safe-stop por software funciona? | Conclusión |
|---|---|---|
| Pérdida de red | No — no se puede enviar el revert (HA inalcanzable) | No cubre |
| Pérdida de alimentación | El equipo se apaga (sin efecto) o queda en estado a recuperar | No cubre la recuperación |
| Caída del proceso (Kaenori/HA) | No — no hay quién emita el revert | No cubre |
| Reinicio | Estado persistido (boot_id/expiración) pero el revert sigue necesitando HA | Cubre parcial |
| Expiración | Solo Clase A tiene TTL; Clase B no expira por sí solo | No cubre |

**Conclusión arquitectónica:** el "safe-stop acotado por software" **solo es válido durante una transición
conectada en vivo**; **no cubre** pérdida de red/alimentación/proceso. Para Clase B **sin fallback hardware
(relé local)**, no debe usarse actuación desatendida relevante para seguridad. La única garantía física
robusta es **CCR Clase A (TTL/dead-man del dispositivo)**. Clase B sin relé = actuación **solo supervisada
con runbook de recuperación manual**.

*Documento A0. No autoriza implementación, despliegue ni actuación.*
