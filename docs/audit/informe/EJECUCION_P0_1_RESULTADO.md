# P0.1 — Ejecución real de la suite hermética (resultado reproducible)
## Evidencia A1 · `549700b9` · pytest 9.1.1

**Comando:** `PYTHONPATH=.:core python3 -m pytest tests/ --ignore=tests/synthetic -q`
(equivalente a lo que ejecuta `ci.yml`, sin red ni hardware).

## Resultado

```
1 failed, 1563 passed, 5 skipped, 71 subtests passed in 65.04s
```

## Regresión detectada (1 fallo)

**Test:** `tests/test_home_experience_enroll.py::TestSetupUiContract::test_s11_read_only_ready_gate`

- El test exige que la pantalla S11 contenga `enroll.status === "completed"`.
- El código actual de S11 aplica el cambio **ING-020** (consentimiento explícito antes de
  `approve→enroll`) y **ya no contiene** ese literal: el enrolado se dispara solo tras marcar el
  checkbox de consentimiento.
- **Causa probable:** el cambio ING-020 no actualizó el test de contrato de S11 → **regresión de contrato
  UI** en la rama canónica.

## Conclusión P0.1

- **La suite hermética NO está en verde hoy**: hay **1 fallo** (regresión de contrato S11).
- El criterio de aceptación "ausencia de regresiones" **no se cumple** en `549700b9`.
- **Acción P0.1 prioritaria:** corregir el test S11 (o el código) para reconciliar el contrato con el
  cambio ING-020, antes de ampliar el CI.

## Estado de cobertura (reconfirmado)

- CI hoy: **1569** tests (`tests/` sin synthetic) → **1 fallo**.
- Fuera de CI: **1029** (tests_bdm 920 + core/tests 85 + synthetic 18 + factory 6).

*Evidencia A1 hermética. No modifica código, no despliega.*
