# ACTA DEL CONCILIO — Ronda 19 (auditoría gate P0.3: test de equivalencia EMS) · 2026-11-04

**Concilio:** GPT-5 + Argon (rol 07-auditor, evidencia cruda) · **A1**.

## Objeto

Gate del test de equivalencia EMS (`tests_bdm/test_ems_equivalence.py`, rama `engineering/p0-3-ems`,
base `549700b9`).

## Veredicto: **APRUEBA-CON-CAMBIOS (gate superado tras mejoras)**

- GPT-5: aprueba-con-cambios — `pytest.approx`, casos surplus 0/negativo, parametrizar, separar el caso
  divergente.
- Argon: aprueba-con-cambios — cubrir precio intermedio (0.22), modo inactivo en consignas, documentar el
  supuesto `demand x3`.

**Mejoras incorporadas (commit `26e1fbd`):**
1. Precio intermedio `0.22` en ramas tarifarias.
2. Nuevo test `test_water_setpoint_inactive_mode_equivalence` (modo inactivo/cruzado → 0 en ambas).
3. Documentado el supuesto `demand x3` (home_ems desglosa 3 zonas; equivalencia solo cuando coinciden).

**Pendiente (no bloqueante):** `pytest.approx` y parametrización (opcional; los retornos son int/str,
igualdad exacta es segura hoy); casos surplus=0/negativo a añadir si el dominio lo soporta.

## Resultado

- **8 passed** (0 failed) · SHA-256 `8fa18bfba6834edd8b9dbbd10ab0dd6750d75d0236c5fc5c763f2ac190c7e548`.

## Votos

| IA | Voto |
|---|---|
| GPT-5 | aprueba-con-cambios (mejoras incorporadas) |
| Argon | aprueba-con-cambios (mejoras incorporadas) |

## Estado

- Rama `engineering/p0-3-ems` → commits `1c3d76f` (test) + `26e1fbd` (mejoras).
- **P0.3 (equivalencia) cerrado**: decisión canónica `home_ems` + test de equivalencia como regresión.
- Resto de P0.3 (retirada de `home_discovery_source/ems.py`) queda para un **paquete posterior** con
  autorización específica (no en P0.3).

*Acta A1. No autoriza A3 ni producción.*
