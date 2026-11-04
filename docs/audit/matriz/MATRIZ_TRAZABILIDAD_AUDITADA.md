# MATRIZ DE TRAZABILIDAD AUDITADA — Kaenori vs Experience V1.3
## Soporte · Auditoría Global · Fuente: `home/integration/connectors-20261022 @ 549700b9`

**Leyenda implementación:** 🟢 implementado · 🟡 parcial · ⚪ pendiente · 🔴 bloqueado
**Leyenda validación:** doc=documentado · cod=codificado · tst=test acreditado · int=integrado · real=
validado en entorno real · **nov=NO VERIFICADO**

| Capacidad V1.3 | Impl | Valid | Evidencia (repo:ruta) | Dependencia / próximo paso |
|---|---|---|---|---|
| Core + 6 puertos | 🟢 | cod | `core/kaenori_core/runtime/ports.py` | — |
| AuthorizationPolicy deny-by-default | 🟢 | cod | `home_security/policy.py` | — |
| Kill-switch (solo bloquea nuevas) | 🟡 | cod | `home_security/killswitch.py` | falta recuperación de ya-ordenados |
| Secretos efímeros TTL | 🟢 | cod | `home_security/ephemeral.py` | — |
| Egress jail | 🟡 | cod | `home_security/egress.py` (`is_enforced=False`) | enforcement real pendiente |
| **CCR Clase A/B + safe_stop** | ⚪ | doc | (solo Anexo A) | **bloqueante seguridad** |
| **Override (manual prevalece)** | ⚪ | doc | (solo Anexo A) | **bloqueante convivencia** |
| Recuperación física por dispositivo | ⚪ | doc | (solo Anexo A) | **bloqueante** |
| Instalación reproducible (factory) | 🟡 | cod | `deployment/factory/` | no ejecutada → nov |
| First-boot + identidad Ed25519 | 🟢 | cod | `deployment/factory/first-boot.sh` | no hardware → nov |
| Backup/restore | 🟡 | cod | `deployment/factory/scripts/backup*.sh` | sin test dedicado |
| Descubrimiento pasivo/activo | 🟢 | cod | `home_discovery/engine.py`, `home_netdiscovery/`, `home_scout/` | — |
| Catálogo + dedupe | 🟢 | cod | `core/kaenori_core/discovery/{catalog,dedupe,unified}.py` | — |
| Conectores solo-lectura (9) | 🟢 | cod | `home_connectivity/` (v2c, shelly, victron…) | — |
| **Proyección 12 categorías → 5 verticales** | ⚪ | doc | (solo V1.3 §5) | falta mapeo en código |
| Wizard (máquina de estados) | 🟢 | cod | `home_catalog/wizard.py` | — |
| Onboarding conversacional | 🟢 | cod | `home_onboarding/` | — |
| Liloo Lite (NLU) | 🟢 | cod | `home_liloo_lite/` | — |
| Cerebro experiencia (sesiones/memoria/voz) | 🟢 | cod | `core/home_experience/` | — |
| **Dashboard "cuenta la historia"** | ⚪ | doc | (solo Anexo D) | falta implementar |
| **Continuidad por usuario (mural/voz)** | ⚪ | doc | (solo Anexo D) | dependencia identidad/rol |
| EMS Shadow (propone sin ejecutar) | 🟢 | cod | `home_ems/shadow.py` | — |
| Matemática EMS | 🟢 | cod | `home_ems/{math,plan}.py` | — |
| **Oportunidades (5 momentos + A/B/C)** | ⚪ | doc | (solo Anexo A) | **bloqueante producto** |
| CI + suite de tests (203) | 🟡 | cod | `.github/workflows/ci.yml`; `tests/`, `tests_bdm/` | no ejecutada → nov |
| **Instalación real en nodo (t520)** | 🔴 | nov | `deployment/nodes/HOME0001/install-manifest.yaml` ("NO ejecutado") | **bloqueante entorno** |

**Resumen:** gobierno y lectura fuertes (codificado); **recuperación física (CCR/Override) y la capa de
Oportunidades solo existen en papel**; **instalación real no verificada**. Estados reales y CI no
ejecutados quedan **NO VERIFICADO**.
