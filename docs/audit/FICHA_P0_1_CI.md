# FICHA EJECUTABLE — Paquete P0.1 · CI y calidad
## Preparación A0 · pendiente de revisión 07-auditor y autorización A1/A2

**Rama futura propuesta:** `engineering/p0-1-ci` · **Fuente:** `home @ 549700b9` · **No implementa cambios.**

## 1. Inventario verificable de suites y casos

| Suite | Ficheros | Casos (`def test_`) | Ejecuta CI hoy |
|---|---|---|---|
| `tests/` | 132 | ~1558 | ✅ (`pytest tests/`) |
| `tests_bdm/` | 34 | ~920 | ❌ |
| `core/tests/` | 8 | ~85 | ❌ |
| `deployment/factory/tests/` | 6 (shell) | — | ❌ |
| **Total** | **180** | **~2563** | solo 1558 |

## 2. Clasificación de pruebas (riesgo/dependencias)

| Clase | Ejemplos | Riesgo | Ejecución CI |
|---|---|---|---|
| `unit` / `contract` | lógica pura, contratos Core | nulo | por defecto |
| `sim` | integración simulada (fixtures, mocks) | bajo | por defecto |
| `external` | red/servicios (HA, APIs) | medio | gate manual |
| `infra` | privilegios/root (nftables, systemd) | alto | gate manual |
| `hardware` | dispositivos reales (t520) | crítico | **nunca** por defecto |

## 3. Procedimiento seguro de recogida (sin red ni hardware)

- `PYTHONPATH=.:core python -m pytest --collect-only -q tests/ tests_bdm/ core/tests/` en entorno
  **aislado** (sin red local, sin `docker`, sin hardware), para detectar *import-time side effects* antes
  de ejecutar. Prohibido ejecutar las clases `external`/`infra`/`hardware` en la recogida.

## 4. Protección de contratos Core y guardrails

- Suite prioritaria e independiente **`contract_core_and_safety`**: verifica los 6 puertos de
  `core/kaenori_core/runtime/ports.py`, la política deny-by-default/fail-closed, el kill-switch
  persistente y que **`HomeActionExecutor` permanece bloqueado** (no activado). Es bloqueante en CI.

## 5. Propuesta de modificación mínima de CI

- Ampliar `ci.yml` a `pytest tests/ tests_bdm/ core/tests/` con **marcas pytest** (unit/contract/sim/
  external/infra/hardware). Jobs separados: hermético por defecto; external/infra con `workflow_dispatch`;
  hardware fuera de CI. Factory (shell) en job propio.

## 6. Criterios de aceptación, rollback y evidencia

- **Aceptación:** CI verde sobre las suites herméticas; reporte de cobertura por suite; lista de excluidos
  con causa; `--collect-only` sin efectos secundarios; dry-run en rama aislada.
- **Rollback:** workflows versionados en YAML; `workflow_dispatch` para desactivar suites nuevas; mantener
  el pipeline anterior hasta validar.
- **Evidencia:** log de CI + cobertura + nº de casos por suite + firma de "no regresión".

## 7. Rama futura

`engineering/p0-1-ci` (a abrir cuando se autorice A1/A2).

*Ficha A0. No autoriza implementación; requiere revisión 07-auditor y aprobación A1/A2.*
