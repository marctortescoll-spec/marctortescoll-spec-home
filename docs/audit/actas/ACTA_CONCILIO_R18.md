# ACTA DEL CONCILIO — Ronda 18 (auditoría gate P0.1: ampliación CI) · 2026-11-04

**Concilio:** GPT-5 + Argon (rol 07-auditor, evidencia cruda) · **A1**.

## Objeto

Gate de la ampliación de `ci.yml`: de `pytest tests/` a las 3 suites herméticas (`tests/ tests_bdm/
core/tests/`), con exclusiones documentadas.

## Evidencia revisada (cruda)

- `git diff` de `.github/workflows/ci.yml` (+13/−2).
- Comando real: **2528 passed, 5 skipped, 269 subtests passed** (0 failed).
- SHA-256: `5bbc2399…` (antes de trazabilidad) · `cadd7d9` (final).

## Veredicto: **APRUEBA-CON-CAMBIOS (gate superado tras condición)**

- GPT-5: **aprueba** (correcto, exclusiones justificadas, sin regresión oculta).
- Argon: **aprueba-con-cambios** — condición: trazabilidad obligatoria de los `--ignore` temporales antes
  de mergear.

**Condición cumplida:** se añadieron referencias `TODO P0-1-EXC-01..03` a cada exclusión (commit
`cadd7d9`).

## Mejoras registradas (no bloqueantes, a futuro)

1. **`test_wizard_proxy.py`**: la fuga de puerto podría indicar un **leak real en el proxy de producción**
   (sockets no cerrados). Investigar como `P0-1-EXC-03` (prioridad).
2. **`pyyaml`**: añadirlo a `[test]` extras de `pyproject.toml` en lugar de excluir `tests/synthetic`
   (`P0-1-EXC-01`).
3. **Mover exclusiones a `pyproject.toml`** (`testpaths`/`[tool.pytest.ini_options]`) + marcas pytest
   (`external`/`hardware`) en lugar de `--ignore` hardcoded.
4. Job nocturno separado para las suites no herméticas (network on, puertos efímeros).

## Votos

| IA | Voto |
|---|---|
| GPT-5 | aprueba |
| Argon | aprueba-con-cambios (condición cumplida) |

## Estado

- Rama `engineering/p0-1-ci` → commits `3829f70` (fix S11) + `e862514` (CI) + `cadd7d9` (trazabilidad).
- **P0.1 (CI) prácticamente cerrado** a falta del merge A2 a `integration/connectors-20261022`.

*Acta A1. No autoriza A3 ni producción.*
